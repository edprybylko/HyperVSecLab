# 🛡️ HyperVSecLab

**HyperVSecLab** is a modular, automated security lab built on **Hyper-V** for analyzing suspicious files (like DOCX, PDFs, ZIPs), isolating malware in disposable VMs, and inspecting traffic through a hardened Linux gateway.

> Built for defenders, researchers, and tinkerers who want a personal sandbox with IDS, VPN, and forensic tools — without needing Qubes OS or a second physical machine.

---

## 🧰 Features

- ⚙️ **Disposable VMs** for inspecting malicious documents
- 🔐 **Isolated file transfer** using read-only VHD (no folder sharing)
- 🧪 **Forensics tools preinstalled** (oletools, binwalk, clamav, steghide)
- 📡 **net-vm** with Suricata, Squid proxy, and VPN support
- 🚀 **Automation scripts** to spin up a lab in seconds
- 🪟 **Toast notifications** to your Windows host for Suricata or Nessus alerts

---

## 📥 Quick Start

### ✅ 1. Prerequisites

- Windows 11 with **Hyper-V enabled**
- At least 8 GB RAM (16 GB recommended)
- Admin PowerShell access
- [Download Xubuntu 22.04.3 LTS Desktop ISO](https://cdimage.ubuntu.com/xubuntu/releases/22.04.3/release/)
- (Optional for net-vm) [Download Xubuntu Minimal / Server ISO](https://ubuntu.com/download/server)

---

## 📁 File Structure

```plaintext
C:\HyperVSecLab\
├── VMs\                     # All VM disks live here
│   ├── xubuntu-base.vhdx
│   ├── net-vm-base.vhdx
│   ├── transfer.vhdx
│
├── scripts\                # Automation tools
│   ├── create-base-vm.ps1
│   ├── create-transfer-vhd.ps1
│   ├── create-diff-inspect-vm.ps1
│   ├── clone-vm-from-template.ps1
│   ├── new-labsession.ps1
│
├── inspect-vm\
│   └── install-analysis-tools.sh
├── net-vm\
│   └── setup-vpn.sh, suricata-setup.sh, etc.
└── ...
