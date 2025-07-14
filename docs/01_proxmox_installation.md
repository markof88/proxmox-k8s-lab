# 🖥️ Proxmox Installation

This guide covers the installation of Proxmox VE on bare-metal hardware.

---

## 📥 Download Proxmox VE ISO

1. Go to the official [Proxmox Downloads Page](https://www.proxmox.com/en/downloads).
2. Download the latest **Proxmox VE 8.x ISO Installer**.
3. Flash the ISO to a USB drive using [balenaEtcher](https://etcher.io) or `dd`:

   ```
   sudo dd if=proxmox-ve_8.x.iso of=/dev/sdX bs=4M status=progress && sync
   
🧰 Install Proxmox on Hardware
Boot from the USB stick.

Select Install Proxmox VE.

Accept the license and select your target disk (e.g. SSD/HDD).

Configure:

Country, time zone, keyboard layout

Root password & email

Hostname (e.g. proxmox.local)

Static IP address (e.g. 192.168.178.100)

Wait for the installation to complete.

Reboot the system and remove USB.

🌐 Access the Web Interface
Once the system reboots:

Open: https://<your-ip>:8006

Login: root with the password you set

Accept the self-signed SSL certificate warning
