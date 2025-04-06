# 🧪 HyperVSecLab
**Disposable, isolated security lab for Windows 11 using Hyper-V**  
Inspired by Qubes OS, purpose-built for analysts and malware researchers.
---
## 🚀 Features
- 🔐 WireGuard-based network isolation
- 🧱 Disposable `inspect-vm` for malware/PDF/DOCX analysis
- 🌐 Persistent `net-vm` with Suricata & Nessus
- 💾 Dual transfer disks:
  - `transfer-config.vhdx` for provisioning
  - `transfer-sandbox.vhdx` for dangerous files (read-only)
- 🔁 Fully automated:
  - VM provisioning
  - Config disk population
  - VPN tunnel and rules
  - Secure session wipe
- 🧭 Windows-native: integrates into Hyper-V, Start Menu, and PowerShell
---
## 💾 Requirements
- Windows 11 Pro/Enterprise
- Hyper-V enabled
- PowerShell 5.1+
- ~20 GB disk space
- Internet access (or local ISO)
Optional:
- `sdelete64.exe` in `C:\Tools` (for secure sandbox disk wiping)
---
## ⚙️ Setup Instructions
### 1. Build a Base VM (Inspect or Net)
```powershell
cd C:\HyperVSecLab\scripts
.\build-vm.ps1
```
- Select VM type: `inspect-vm` or `net-vm`
- Choose ISO:
  - Auto-detect from `/isos`
  - Download default (Xubuntu or Ubuntu Server)
  - Provide custom ISO URL
- VM + base `.vhdx` created in `/VMs`
- `transfer-config.vhdx` is auto-created & populated
- If `inspect-vm`, a transfer-sandbox.vhdx is also created
➡ Boot the VM, complete the OS install, then shut it down and rename it:
- `inspect-vm` → for disposable analysis
- `net-vm` → for routing, VPN, Suricata, and Nessus

### 2. Provision net-vm
```powershell
.\setup-netvm.ps1
```
- Attaches `transfer-config.vhdx` to `net-vm`
- Calls `enable-netvm-autostart.ps1` to boot on host startup
Inside `net-vm`:
```bash
sudo mount /dev/sdX1 /media/config
cd /media/config
sudo bash install.sh             # Install Suricata, WireGuard, NAT
sudo bash init-wireguard.sh     # Generate wg0.conf and keys
sudo bash export-client-config.sh  # Create inspect-vm, winhost, mobile configs
```
### 3. Launch Inspect Session (Disposable)
```powershell
.\new-labsession.ps1
```
#### Creates new inspect-vm using differencing disk
 Attaches:
- `transfer-config.vhdx` (read/write)
- `transfer-sandbox.vhdx` (read-only)
Optionally installs shortcuts to Start Menu and Desktop
➡ Now open suspicious files inside inspect-vm safely.
---
## 🔒 Secure VPN Tunnels
- VPN client configs generated in `/clients`:
### Client	File	Usage
- inspect-vm	`inspect01.conf`	/etc/wireguard/ inside VM
- Windows host	`winhost.conf`	Import to WireGuard for Windows
- Mobile	`mobile.conf`	Scan QR in WireGuard mobile app
---
## 🧼 Wipe Session or Full Lab
```powershell
.\burn-lab.ps1 -Mode session
```
- Deletes `inspect-vm`
- Optionally wipes sandbox disk with `sdelete64.exe`
```powershell
.\burn-lab.ps1 -Mode all
```
- Deletes all VMs
- Removes transfer disks
- Cleans up virtual switches
- Removes desktop/start menu shortcuts
---
## 🔍 Suricata Detection Rules
Includes rules for:
- DNS hijacking
- ARP spoofing
- Suspicious file downloads
- C2 beaconing patterns
- TLS certificate anomalies
Custom rules located at:
`net-vm/etc/suricata/hypervseclab.rules`
---
## 🧑‍💻 License
### Business Source License 1.1
- Change Date: 2028-04-05
- See LICENSE for full terms
---
## 🤝 Credits
Inspired by the modular isolation principles of `Qubes OS`
