# 🧪 HyperVSecLab

**Qubes-like disposable security lab for Windows 11+**  
Leverages Hyper-V, WireGuard, and automation to create secure, repeatable sandboxing environments for analyzing potentially malicious files.

---

## 📦 Features

- 🧱 Disposable `inspect-vm` (Xubuntu-based) for isolated file analysis
- 🔐 Persistent `net-vm` acting as:
  - WireGuard VPN server
  - Intrusion Detection System (Suricata/Nessus optional)
- 💽 Dual shared disks:
  - `transfer-config.vhdx` (512MB) — safe configs/scripts
  - `transfer-sandbox.vhdx` (5GB) — suspicious payloads
- 📤 Secure communication to host using Windows toast notifications
- 🔁 Automatic snapshotting, cleanup, and launcher integration
- 🧹 One-command cleanup for session or full lab wipe
- 📎 Desktop & Start Menu shortcuts with `.bat` launchers

---

## 🚀 Quick Start

### 1. 📥 Prerequisites

- ✅ Windows 11 Pro / Enterprise (Hyper-V enabled)
- ✅ WireGuard installed inside `net-vm`
- ✅ PowerShell 5.1+
- ✅ [Sysinternals `sdelete64.exe`](https://docs.microsoft.com/en-us/sysinternals/downloads/sdelete) at `C:\Tools\sdelete64.exe`

---

### 2. 📦 Install + Initialize (First Time Only)

```powershell
cd C:\HyperVSecLab\scripts

# Create shared disks
.\create-transfer-config-vhd.ps1
.\create-transfer-sandbox-vhd.ps1

# Setup net-vm
.\setup-netvm.ps1

# Optional: create desktop/start menu launchers
.\install-launchers.ps1
```

### 3. 🔬 Daily Usage
Start a disposable inspect-vm session:
```powershell
.\new-labsession.ps1
```
🟢 This will:
Create a differencing disk
Launch inspect-vm from xubuntu-base.vhdx
Mount:
transfer-config.vhdx (RW)
transfer-sandbox.vhdx (RO)
Analyze files in a fully isolated environment.

### 4. 🧼 Cleanup Options
Action	Command
🔁 Clean disposable session	
```powershell
.\burn-lab.ps1 -Mode session
```
🔥 Reset entire system state (keep project files)	
```powershell
.\burn-lab.ps1 -Mode all
```
🖥️ GUI Integration
Installed by:
```powershell
.\install-launchers.ps1
```

### Shortcut	Location
🧪 Start Lab Session	Start Menu + Desktop
🧹 Burn Lab Session	Start Menu + Desktop
🔥 Burn Full Lab	Start Menu + Desktop
📁 Repo Structure (Summary)
```graphql
C:\HyperVSecLab\
├── VMs\                          # VHDX disks (base + transfer)
├── scripts\                      # PowerShell automation
├── net-vm\                       # WireGuard server & health scripts
├── inspect-vm\                   # Sandbox forensic tooling
├── windows-host\                 # Toast listener
├── launchers\                    # .bat files for GUI launch
├── transfer\                     # WG client configs copied from net-vm
├── LICENSE, README.md, .gitignore
```

## 🔒 Security Model
Component	Purpose
net-vm	VPN tunnel, IDS, host traffic proxying
inspect-vm	Disposable sandbox (clean every session)
transfer-config.vhdx	Safe disk for scripts, configs
transfer-sandbox.vhdx	Isolated RO disk for malware
burn-lab.ps1	Controlled wipe of VMs and disks
wireguard-healthcheck.sh	Detects tunnel failures + notifies host


## 🧑‍💻 License
Licensed under the Business Source License 1.1 (BSL-1.1).
Non-commercial use only until Change Date: 2028-04-05.

## 🤝 Credits
Built by edprybylko in collaboration with 🧠 ChatGPT assistant.
Project inspired by Qubes OS — reimagined for Windows-native workflows.
