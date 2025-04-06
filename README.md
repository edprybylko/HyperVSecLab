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
- 📡 **net-vm** with IDS (Suricata), VPN support, and optional proxy
- 🧪 Scripts to automate VM creation, teardown, session launch, and **real-time notifications**
- ⚡ Runs entirely on Hyper-V with no 3rd-party dependencies

---

## 📥 Requirements

- Windows 11 Pro or Enterprise with **Hyper-V enabled**
- At least 8–16 GB RAM
- Admin PowerShell
- **Xubuntu ISOs**:
  - [Xubuntu Desktop ISO (for inspect-vm)](https://cdimage.ubuntu.com/xubuntu/releases/)
  - [Uubuntu Server/Minimal ISO (for net-vm)](https://ubuntu.com/download/server)

---

## 🧱 Folder Structure

```plaintext
C:\HyperVSecLab\
├── VMs\                      # VHDX files: base, transfer, sessions
├── scripts\                 # PowerShell automation scripts
├── inspect-vm\             # Linux-side install script
├── net-vm\                 # VPN, IDS, and proxy setup
├── transfer\               # Transfer VHD creation
├── windows-host\           # Toast alert listener (optional)
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
