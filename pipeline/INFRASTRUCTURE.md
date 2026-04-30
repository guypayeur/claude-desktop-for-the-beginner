# Infrastructure — local Hyper-V VM for the autonomous bot

The LLM-in-the-loop pipeline needs a running Claude Desktop instance it can drive on its own — to verify chapter claims, take screenshots, and try new features as they ship. This document is the procedure for standing up that instance as a local Hyper-V VM on the maintainer's Windows machine.

For the *why* behind this architecture, see [README.md](README.md).

## Why local Hyper-V

- **Free** — no cloud bill; uses your existing Windows 11 Pro license
- **Real Windows desktop** — full app compatibility (unlike Windows containers, which are server-oriented and rough on GUI apps)
- **Isolated from your work session** — the bot has its own user, its own signed-in Claude account, its own desktop
- **Replaceable with a cloud VM later** — same RDP target, same MCP interface; the pipeline doesn't care where the VM lives

## Decisions to make before you start

These shape the rest of the setup. Pick now or you'll re-do work later.

| Decision | Options | Recommendation |
|---|---|---|
| **Bot Anthropic account** | Reuse your main account, or create a dedicated one | Dedicated. Isolates risk if the account ever gets flagged or rate-limited. |
| **Plan tier for the bot** | Free, Pro, Max | Pro at minimum — free can't access several features the book covers (Cowork, advanced Skills, etc.). Match the most common reader's plan. |
| **Resources for the VM** | RAM, vCPU, disk | 8 GB RAM (dynamic), 4 vCPU, 80 GB dynamic VHDX. Adjust down to 4 GB / 2 vCPU if your host is tight. |
| **Windows license inside the VM** | Microsoft eval VM (90 days), or full license | Eval VM for the proof-of-concept, full license once the pipeline is working and you decide to keep it. |
| **AI driver** | Anthropic computer use (vision-based) vs custom MCP server (UI Automation) | Computer use first. Switch to a custom MCP server later only if vision-driven control proves too slow or unreliable. |

**Worth checking before you scale**: Anthropic's Terms of Service. Driving the desktop app from an automated session with a paid account isn't obviously prohibited but isn't obviously permitted either. A polite ask to Anthropic — or at minimum a careful read of the ToS — is worth the few minutes before you commit serious work.

---

## Prerequisites on the host

- **Windows 11 Pro, Enterprise, or Education** (Hyper-V is not available on Home edition)
- **Hardware virtualization enabled in BIOS** (most modern machines have this on by default)
- **At least 16 GB host RAM** if you want 8 GB for the VM and still use your computer comfortably
- **At least 100 GB free disk** (80 for the VM, headroom for snapshots)
- **Local admin rights** on your Windows account

Verify edition and free disk:

```powershell
Get-WindowsEdition -Online   # should say Professional / Enterprise / Education
Get-PSDrive C                # check 'Free' column
```

---

## Phase 1 — Enable Hyper-V (10 minutes, includes one reboot)

1. Open PowerShell **as Administrator**.
2. Enable the feature:
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
   ```
3. Reboot when prompted.
4. After reboot, confirm Hyper-V Manager appears in the Start menu and opens.

**Gotcha**: if the command fails with "the requested operation requires elevation," PowerShell isn't actually admin — close it and right-click → Run as Administrator.

---

## Phase 2 — Get a Windows 11 ISO (15 minutes, mostly download time)

Two paths:

- **Quick start (recommended for the proof-of-concept)**: Microsoft's free [Windows 11 Development Environment VM](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/) — pre-built `.vhdx`, ~20 GB, 90-day evaluation. Good enough to validate the pipeline before committing to a license.
- **Permanent setup**: download the [Windows 11 ISO](https://www.microsoft.com/software-download/windows11) and use a license key you own. ~6 GB ISO, full Windows install required.

This guide assumes the **ISO path**. The dev-VM path skips Phase 3 (the VM is pre-made) and Phase 4 install, but you'll still configure it in Phase 5.

---

## Phase 3 — Create the VM (10 minutes)

In Hyper-V Manager → **Action → New → Virtual Machine**:

| Setting | Value |
|---|---|
| Name | `claude-bot-vm` |
| Generation | **2** (UEFI, required for modern Windows) |
| Startup memory | 8192 MB, **Use Dynamic Memory** checked |
| Network | **Default Switch** (NAT'd to host — internet works out of the box) |
| Hard disk | New, 80 GB, Dynamic VHDX, default location |
| Install option | Install from bootable image → point at your Windows 11 ISO |

Before starting the VM, edit its settings:

- **Security → Secure Boot**: keep enabled, template **Microsoft Windows**
- **Processor**: 4 virtual processors
- **Checkpoints**: enable, type **Production**

Start the VM, press a key when prompted to boot from the ISO, and proceed to install Windows.

---

## Phase 4 — Install and configure Windows in the VM (30 minutes, mostly install time)

Walk through the Windows installer normally. During OOBE (the "Out Of Box Experience"):

- **Edition**: Windows 11 Pro
- **Account type**: Local account (the OOBE makes this hard now — see gotcha below)
- **Username**: `claude-bot`
- **Privacy settings**: turn off everything you can; this VM doesn't need telemetry, location, ad ID, etc.

Once at the desktop, configure for unattended use. Open PowerShell **as Administrator** inside the VM:

```powershell
# Disable lock screen idle timeout
powercfg /change monitor-timeout-ac 0
powercfg /change standby-timeout-ac 0

# Allow auto-login (you'll be prompted to enter the password once)
netplwiz
# Uncheck "Users must enter a user name and password to use this computer"

# Enable Remote Desktop
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name 'fDenyTSConnections' -Value 0
Enable-NetFirewallRule -DisplayGroup 'Remote Desktop'

# Disable Windows Update automatic restarts (manual updates only)
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU' -Name 'NoAutoRebootWithLoggedOnUsers' -Value 1
```

**Gotcha (local account on Windows 11 OOBE)**: recent Windows 11 builds force a Microsoft account during OOBE. Workaround: at the network screen, press **Shift + F10**, run `OOBE\BYPASSNRO`, and the VM will reboot with a "I don't have internet" option that unlocks local-account creation. Alternatively, disconnect the VM's network adapter in Hyper-V Manager before starting.

---

## Phase 5 — Install Claude Desktop and sign in (10 minutes)

Inside the VM:

1. Open Edge → navigate to **claude.com/download**
2. Download the Windows installer, run it
3. Sign in with the **dedicated bot account** (decided in the up-front decisions table)
4. Open Settings inside Claude Desktop and:
   - Disable any "show update prompt" / "tips" toggles
   - Confirm the model picker is set to your default (the bot will use whatever the human reader would by default)

**Verify** the bot account works: send one message in Claude Desktop. If you see a normal response, you're good.

---

## Phase 6 — Test RDP from the host (5 minutes)

From the host (your real Windows session), open PowerShell:

```powershell
# Find the VM's IP — easiest from inside the VM:
#   ipconfig | findstr IPv4
# Then on the host:
mstsc /v:<VM-IP>
```

Sign in as `claude-bot`. You should see the VM's desktop with Claude Desktop ready to drive. Save credentials so you don't re-enter them.

**Gotcha**: the Default Switch in Hyper-V uses NAT, so the VM's IP is internal. RDP from the host works; RDP from another machine on your network does not, by design. If you want network-wide access later, switch to an External virtual switch.

---

## Phase 7 — AI driver: first experiment with Anthropic computer use (1 afternoon)

Goal: prove the AI can drive the VM end-to-end before you build pipeline code around it.

Outline of the experiment:

1. On the host, write a small Python script using the Anthropic SDK with the `computer_use` tool enabled.
2. The script captures the VM's screen via an RDP screenshot (or, simpler for a first test, has the human take a screenshot and feed it in).
3. Give Claude one task: *"Open the Customize panel in Claude Desktop and describe what you see, listing every visible item."*
4. Compare the AI's description to what you see in Chapter 1's "first look" section. Discrepancies are the first verification debt the pipeline will eventually surface automatically.

If the experiment works, expand to:

- Take a real screenshot from the VM, save to `book/images/`
- Diff against the previously committed image (perceptual diff, not byte-exact)
- Write the diff result to a PR comment

If the experiment doesn't work (auth blocks, computer use can't drive over RDP, screenshots come back unreadable), reconsider before building the full pipeline.

---

## Phase 8 — Wire into the pipeline (deferred until experiment succeeds)

Once Phase 7 proves the concept, the [pipeline](README.md) gains one new step:

```
┌──────────────────────────────────┐
│  draft_updates.py (Claude API)   │
│  → now also calls verify.py:     │
│     • opens RDP to claude-bot-vm │
│     • computer-uses Claude       │
│       Desktop to confirm each    │
│       claim in the affected      │
│       chapter                    │
│     • flags claims it can't      │
│       verify                     │
└──────────────────────────────────┘
```

The human reviewer still gates everything. The verification step adds evidence to the PR ("verified in v1.5354"), not autonomy.

---

## Maintenance

- **Snapshot the VM** after Phase 5 — gives you a known-good restore point if Claude Desktop updates or Windows updates break things
- **Monthly**: log in interactively, install pending Windows updates, reboot, snapshot again
- **When Anthropic ships a major Claude Desktop change**: expect to re-verify Phase 7 manually before trusting the next pipeline run

## Cost sanity check

| Item | Recurring cost |
|---|---|
| Host hardware | $0 (yours) |
| Windows license in VM | $0 (eval) or one-time license cost |
| Anthropic Pro for bot account | ~$20/mo |
| Anthropic API (computer use during pipeline runs) | scales with cadence; budget $5–20/mo at daily checks |

Total: $25–40/mo running cost once licensed, vs $40–110/mo for the equivalent cloud VM + Anthropic account.

---

## What you can hand to me to do for you

- Phase 1 commands (PowerShell — needs your admin confirmation to actually run)
- Phase 4 in-VM PowerShell (run via Enter-PSSession into the VM once RDP works)
- Phase 7 Python script (drafted on the host, you run it with your API key)
- Phase 8 pipeline integration (drafted, committed, PR-reviewed)

## What you have to do yourself

- Decisions in the table at the top
- Anthropic ToS check
- Clicking through the Windows installer in Phase 4
- Signing into Claude Desktop in Phase 5 (your password)
- The first manual verification of the experiment in Phase 7
