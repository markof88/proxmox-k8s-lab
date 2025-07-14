# 🐧 Debian Installation (VM)

This guide walks you through installing Debian 12 on your virtual machines (`controlplane-1`, `workernode-1`, `workernode-2`).

---

## 📥 Download Debian ISO

1. Visit the official [Debian ISO Downloads](https://www.debian.org/distrib/) page.
2. Choose **64-bit PC netinst ISO** for Debian 12 (Bookworm).
3. Upload the ISO to Proxmox:  
   `Datacenter ➝ local ➝ ISO Images ➝ Upload`

---

## 🧰 Install Debian 12 on VM

1. Launch the VM and boot from the Debian ISO.
2. Follow the guided installation:
   - Language, location, keyboard layout
   - Hostname: `controlplane-1`, `workernode-1`, or `workernode-2`
   - Domain: leave blank
   - Set root password
   - Create user (optional, not required for Kubernetes)
   - Configure static IP manually:
     - Example: `192.168.178.110`, gateway `192.168.178.1`, DNS `1.1.1.1`
   - Partitioning: Use **Guided - use entire disk** (no LVM required)
   - Finish installation and reboot

---

## 🧼 Post-Install Cleanup

After reboot, login as `root` and run:

```bash
apt update && apt upgrade -y
```
Disable predictable network interface names (optional but helpful):

```bash
ln -s /dev/null /etc/systemd/network/99-default.link
```
Then reboot:

```bash
reboot
```
