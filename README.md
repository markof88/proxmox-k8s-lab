# Proxmox Kubernetes Lab

A complete step-by-step home lab using Proxmox + Debian 12 + kubeadm to build a production-grade Kubernetes cluster.

![Lab Diagram](images/k8s-lab-diagram.png) <!-- Replace this with your diagram later -->

---

## 🧰 Lab Topology

- **Hypervisor**: Proxmox VE
- **Control Plane**: Ubuntu 24.04 (`controlplane-1`)
- **Worker Nodes**:
  - Ubuntu 24.04 (`workernode-1`)
  - Ubuntu 24.04 (`workernode-2`)
- **Networking**: Static IP on same subnet `192.168.178.0/24`

---

## ⚙️ Tools & Versions

| Component       | Version    |
|----------------|------------|
| Proxmox VE     | 8.x        |
| OS             | Debian 12 / Ubuntu 24.04 |
| Kubernetes     | v1.33.x    |
| Container Runtime | containerd |
| Bootstrap Tool | kubeadm    |

---

## 📁 Repo Structure

```bash
proxmox-k8s-lab/
├── README.md
├── docs/
│   ├── 01_proxmox_installation.md
│   ├── 02_vm_creation.md
│   ├── 03_debian_installation.md
│   ├── 04_kubernetes_installation.md
│   ├── 05_node_joining.md
│   ├── 06_cluster_verification.md
│   └── 07_next_steps.md
├── images/
│   └── k8s-lab-diagram.png
├── kubeadm/
│   └── kubeadm-config.yaml
└── scripts/
    ├── setup-sysctl.sh
    ├── install-containerd.sh
    ├── install-kubernetes.sh
    ├── join-node.sh
