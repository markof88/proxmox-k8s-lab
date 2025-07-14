# 📦 VM Creation in Proxmox

This guide walks you through creating VMs in Proxmox VE to host your Kubernetes nodes.

---

## 🖥️ VM Templates or Manual?

You can either:
- Use a prebuilt **Cloud-Init image (recommended)**  
- Or manually install Ubuntu/Debian and snapshot it

For this guide, we’ll use **manual installation**.

---

## 🧱 Create a New VM

1. Log in to the Proxmox web UI at [`https://<your-ip>:8006`](https://<your-ip>:8006)
2. Click **Create VM**
3. Fill in the basic details:
   - Node: `proxmox`
   - VM ID: auto or custom
   - Name: e.g. `controlplane-1`

---

## 📀 ISO & OS Settings

- **OS Type**: Linux
- **Version**: Ubuntu 24.04 LTS or Debian 12
- **CD/DVD**: Use the ISO image you uploaded earlier
- **Graphics card**: Default (Standard VGA)

---

## 💾 Hard Disk

- **Bus/Device**: VirtIO
- **Disk size**: 40 GB+
- **Storage**: local-lvm or appropriate

---

## 🧠 CPU & Memory

- **Cores**: 2–4
- **Sockets**: 1
- **Memory**: 4096–8192 MB

---

## 🌐 Network Settings

- **Model**: VirtIO (paravirtualized)
- **Bridge**: `vmbr0`  
- Assign a **static IP** later in the OS

---

## ✅ Final Steps

- Click **Finish** to create the VM
- Mount the ISO and start the VM
- Install Ubuntu or Debian manually inside the VM

---

📌 Repeat this process for:
- `workernode-1`
- `workernode-2`

Use consistent naming and static IPs in the same subnet (`192.168.178.0/24`).
