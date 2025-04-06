# 🛡️ HyperVSecLab

**HyperVSecLab** is a modular, disposable, and automated malware analysis lab built on **Hyper-V** for Windows 11.

It allows you to safely open and inspect suspicious DOCX, PDF, ZIP, or EXE files in an **isolated virtual environment**, with full support for:
- Disposable analysis VMs (inspect-vm)
- Secure file transfer via VHDX (no shared folders)
- Suricata/Nessus monitoring via a hardened `net-vm`
- VPN tunneling, proxy control, and threat detection
- One-click lab automation with PowerShell

---

## 🧰 Features

- 🔒 **Disposable inspect-vm** with forensic tools pre-installed
- 💾 **Isolated transfer.vhdx** shared between host and VM (read-only)
- 📡 **net-vm** with Wireguard, IDS (Suricata), and vulnerability scanning (Nessus)
- 🧪 Scripts to automate VM creation, teardown, session launch, and **real-time notifications**
- ⚡ Runs entirely on Hyper-V

---

## 📥 Requirements

- Windows 11 Pro or Enterprise with **Hyper-V enabled**
- At least 8–16 GB RAM
- Admin PowerShell
- **Virtual Machine ISOs**:
  - [Xubuntu Desktop ISO (for inspect-vm)](https://cdimage.ubuntu.com/xubuntu/releases/)
  - [Ubuntu Server/Minimal ISO (for net-vm)](https://ubuntu.com/download/server)

---

## 🧱 Folder Structure

```plaintext
C:\HyperVSecLab\
├── VMs\
│   ├── xubuntu-base.vhdx
│   ├── net-vm-base.vhdx
│   ├── inspect-session.vhdx          # Diff-based disposable VM disk
│   ├── transfer-config.vhdx          # Safe config files (RW)
│   ├── transfer-sandbox.vhdx         # Suspicious files (R)
│
├── scripts\                          # Windows PowerShell automation
│   ├── create-base-vm.ps1
│   ├── create-transfer-config-vhd.ps1
│   ├── create-transfer-sandbox-vhd.ps1
│   ├── cleanup-disposable-vm.ps1
│   ├── wipe-sandbox-vhdx.ps1         # Secure erase with sdelete
│   ├── check-netvm-status.ps1
│   ├── configure-netvm-network.ps1
│   ├── enable-netvm-autostart.ps1
│   ├── setup-netvm.ps1
│   ├── new-labsession.ps1            # Now mounts both transfer disks
│
├── net-vm\
│   ├── wireguard-setup.sh
│   ├── wireguard-healthcheck.sh
│   ├── export-client-config.sh
│   ├── wg0.conf
│   ├── server_private.key
│   ├── server_public.key
│   ├── clients\
│   │   ├── inspect01.conf
│   │   └── winhost.windows.conf
│
├── inspect-vm\
│   └── install-analysis-tools.sh
│
├── transfer\
│   └── (WG client files auto-copied here by export-client-config.sh)
│
├── windows-host\
│   └── toast-listener.ps1
│
├── LICENSE
├── .gitignore
└── README.md

```
---

## ⚙️ Setup Guide
1️⃣ Create Your Base VM
```
cd C:\HyperVSecLab\scripts
.\create-base-vm.ps1
```
Choose 1 for xubuntu-base (inspect-vm GUI)
Choose 2 for net-vm-base (minimal CLI)
After installing Ubuntu manually, shut down VM
Script will export and snapshot it

2️⃣ Create the Transfer Disk
```
cd C:\HyperVSecLab\transfer
.\create-transfer-vhd.ps1
```
Mount the transfer.vhdx
Copy suspicious files into it (DOCX, PDFs, etc.)
It will be mounted read-only in the inspect-vm

3️⃣ Launch Full Session (Auto)
```
cd C:\HyperVSecLab\scripts
.\new-labsession.ps1
```
This will:
Start net-vm (or import it if missing)
Create a disposable inspect-vm from snapshot
Attach transfer.vhdx
Start everything for analysis

4️⃣ Inside the Inspect VM
Run the forensic tool installer:
```
cd ~/HyperVSecLab/inspect-vm/
chmod +x install-analysis-tools.sh
./install-analysis-tools.sh
```
Tools:
libreoffice, oletools, binwalk, clamav, qpdf, steghide, exiftool

5️⃣ Cleanup After Use
```
cd C:\HyperVSecLab\scripts
.\cleanup-disposable-vm.ps1
```
This:
Deletes the disposable VM
Removes the inspect-session.vhdx disk

## 🧪 Optional
Configure net-vm to route traffic via VPN (setup-vpn.sh)
Enable Suricata for threat detection
Run toast-listener.ps1 on Windows host to receive live alerts
