### 🚀 Kubernetes Installation (kubeadm)

This guide walks you through installing a minimal Kubernetes setup (control-plane only) using kubeadm.

### 🛠 Prepare the System  
Run on all nodes (control-plane and workers):

```bash
# Enable kernel modules
modprobe overlay
modprobe br_netfilter
```

```bash
# Set sysctl params
cat <<EOF | tee /etc/sysctl.d/kubernetes.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sysctl --system
```

📦 Install containerd
```bash
apt update && apt install -y containerd

# Generate default config
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml

# Set SystemdCgroup = true
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

systemctl restart containerd
systemctl enable containerd
```

🔐 Disable Swap & Load Kernel Mods
```bash
swapoff -a
sed -i '/ swap / s/^/#/' /etc/fstab
```

Ensure modules are loaded persistently:

```bash
cat <<EOF | tee /etc/modules-load.d/k8s.conf
br_netfilter
overlay
EOF
```

🔧 Install kubeadm, kubelet & kubectl
```bash
apt update && apt install -y apt-transport-https curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list

apt update
apt install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```
