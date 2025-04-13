## HyperVSecLab - Security lab w/ disposable VMs and mobile-device monitoring

A modular, Qubes-inspired virtual lab for Windows 11 using Hyper-V, pfSense, Xubuntu, and Tailscale VPN — designed to sandbox suspicious files, monitor host and mobile network traffic, and forward real-time alerts directly to your Windows desktop.

HyperVSecLab turns your Windows laptop into a personal security operations center (SOC), complete with deep packet inspection, behavioral analysis, and live notifications — no dual-booting required.

Built for analysts, homelabbers, and security-conscious users who want hardened isolation, full-device monitoring, and zero-trust network control — even for mobile devices.

With built-in support for Tailscale VPN, you can route your phone's traffic through the lab to detect malware behavior, DNS tunneling, C2 beacons, and more using enterprise-grade tools like Suricata and Zeek.

-----

### 📦 FEATURES

✓ Disposable inspect-vm for analyzing suspicious files (PDF, DOCX, EXE, etc.)

✓ Persistent net-vm with pfSense + Suricata + Zeek for deep traffic inspection

✓ Tailscale VPN support for remote/mobile devices with subnet routing

✓ Suricata + Zeek detect malicious activity across LAN/VPN

✓ Real-time alerts sent to Windows via toast notifications

✓ Transfer.vhdx drive auto-syncs logs and config files between host and VMs

✓ PowerShell-based automation (build, setup, snapshot, destroy)

-----

### 🧰 SYSTEM REQUIREMENTS

- Windows 11 Pro / Enterprise (Hyper-V enabled)
- Surface Pro 9 or similar device with > 8GB RAM
- Hyper-V Virtualization enabled in BIOS
- At least 40 GB of free disk space

-----

### 🧱 REPOSITORY STRUCTURE

C:\HyperVSecLab\
├── build.ps1                  → Interactive script to build net-vm / inspect-base
├── new-lab.ps1                → Launches disposable inspect-vm with VHDs
├── setup.ps1                  → Provisions pfSense net-vm (Suricata, Zeek, Tailscale)
├── create-transfer.ps1        → Creates and populates transfer.vhdx
├── burn.ps1                   → Wipes lab session or full net-vm setup
├── tray-monitor.ps1           → Windows tray icon and state monitor
├── transfer\
│   ├── config\
│   │   ├── hypervseclab.rules     → Suricata custom rules
│   │   ├── tailscale.zeek         → Zeek detection script for UDP 41641
│   │   ├── netvm-alert-forwarder.sh → Shell watcher to forward alerts
│   │   └── logs\
│   │       ├── suricata\
│   │       ├── zeek\
│   │       └── alerts-summary.txt
├── isos\
│   ├── netgate-installer-amd64.iso → pfSense ISO
│   └── xubuntu-XX.XX-desktop.iso   → Xubuntu ISO (optional minimal/server)

-----

### 🧩 REQUIRED DOWNLOADS

1. **PuTTY Tools (plink + pscp)**  
   Download from: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html  
   - Required for setup.ps1 SSH automation  
   - Add `plink.exe` and `pscp.exe` to your system PATH  

2. **pfSense ISO (Net-VM)**
   Download from: https://www.netgate.com/downloads  
   - Choose pfSense CE (Community Edition)  
   - Extract the `.gz` to get the `.iso`  

3. **Xubuntu ISO (Inspect-VM)**
   Download from: https://xubuntu.org/download/  
   - You can choose minimal install during setup  
   - ISO must be placed in the `/isos` folder  

4. **Tailscale Account (Free)**
   Register at: https://tailscale.com  
   - Used to connect your devices (mobile, host, net-vm)
   - Subnet routing enabled through pfSense net-vm

-----

### 🚀 GETTING STARTED

1. Clone the repo into:  
   `C:\HyperVSecLab\`

2. Download and move ISO files into the `/isos` folder:
   - `netgate-installer-amd64.iso`
   - `xubuntu-*.iso`

3. Run:  
   `build.ps1`  
   → Choose option to build net-vm (pfSense) and/or inspect-vm base

4. Boot pfSense, complete guided install:
   - Assign interfaces: WAN (external), LAN (internal)
   - LAN IP recommended: 10.10.10.1/24
   - Disable IPv6 (optional)
   - Enable SSHD via console menu (option 14)

5. From host, run:  
   `setup.ps1`  
   → Installs Suricata, Zeek, Tailscale  
   → Sets up alert forwarders and syncs logs

6. On mobile or external device, install Tailscale app and connect
   → Route traffic through net-vm

7. To analyze suspicious files:
   - Run `new-lab.ps1` to spin up inspect-vm
   - Drop files into sandboxed `transfer-sandbox.vhdx`

8. Logs are synced to:  
   `transfer/config/logs/`  
   and alerts forwarded live to your Windows desktop

-----

### 📣 ALERTING & LOGGING

✓ Suricata and Zeek monitor all LAN/VPN traffic  
✓ Alerts logged and forwarded via:
   - netvm-alert-forwarder.sh
   - Windows toast notifications (via `toast-listener.ps1`)
✓ Logs written to:
   - `/mnt/transfer-config/logs/suricata/`
   - `/mnt/transfer-config/logs/zeek/`
   - `alerts-summary.txt` for daily review

-----

### 🧼 RESETTING THE LAB

Run:

- `burn.ps1 -mode session`  
  → Destroys only the disposable inspect-vm + session disks

- `burn.ps1 -mode all`  
  → Wipes net-vm, virtual switches, and resets lab to clean state

-----

Built with 💻 PowerShell, 🔐 pfSense, 🧠 Zeek, 🛡️ Suricata, and ☁️ Tailscale  

Inspired by Qubes OS, brought to your Windows desktop.
