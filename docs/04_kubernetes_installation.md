## 🚀 Kubernetes Installation (kubeadm)

This guide precisely documents the exact steps executed on the master and worker nodes during Kubernetes installation using `kubeadm`.

---

## 🧠 Master Node – Step-by-Step

### 🔧 Basic Checks and System Prep
```bash
uname -r
ifconfig -a
sudo cat /sys/class/dmi/id/product_uuid
nc 127.0.0.1 6443 -zv -w 2
```

### 🔐 Disable Swap
```bash
sudo swapoff -a
vi /etc/fstab  # comment out swap line
cat /etc/fstab
```

### 🧮 Enable IP Forwarding
```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
```

### 📦 Install kubeadm, kubelet, kubectl
```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
kubeadm version
```

### 📦 Install and Configure containerd
```bash
sudo apt update
sudo apt install -y containerd
ps -p 1
cd /etc/
sudo mkdir -p /etc/containerd
containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sudo tee /etc/containerd/config.toml
cat /etc/containerd/config.toml | grep -i SystemdCgroup
sudo systemctl restart containerd
```

### 🌐 Verify IP Address
```bash
ip add
```

### 🧪 Initialize the Control Plane
```bash
sudo kubeadm init --apiserver-advertise-address 192.168.178.101 --pod-network-cidr "10.244.0.0/16" --upload-certs
```

### 📁 Set Up kubectl Access
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 🛰 Install Calico (Tigera Operator)
```bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.2/manifests/operator-crds.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.2/manifests/tigera-operator.yaml
cd /root
curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.30.2/manifests/custom-resources.yaml
vi custom-resources.yaml  # edited to use 10.244.0.0/16 and VXLAN
kubectl create -f custom-resources.yaml
```

### 🔍 Watch Pods and Nodes
```bash
watch kubectl get pods -A
kubectl get nodes
sudo systemctl enable --now kubelet
```

---

## ⚙️ Worker Node – Step-by-Step

### 🔧 Basic Checks and System Prep
```bash
uname -r
ifconfig -a
sudo cat /sys/class/dmi/id/product_uuid
nc 127.0.0.1 6443 -zv -w 2
```

### 🔐 Disable Swap
```bash
sudo swapoff -a
vi /etc/fstab  # comment out swap line
cat /etc/fstab
```

### 🧮 Enable IP Forwarding
```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
```

### 📦 Install kubeadm, kubelet, kubectl
```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
kubeadm version
```

### 📦 Install and Configure containerd
```bash
sudo apt update
sudo apt install -y containerd
ps -p 1
sudo mkdir -p /etc/containerd
containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sudo tee /etc/containerd/config.toml
cat /etc/containerd/config.toml | grep -i SystemdCgroup -B 50
sudo systemctl restart containerd
```

### 🔗 Join the Cluster
Use the token and hash generated from the control-plane output:
```bash
sudo kubeadm join 192.168.178.101:6443 --token jjz1ql.xxxxxxxxxxxxxxxxxx\
  --discovery-token-ca-cert-hash sha256:adxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

```bash
sudo systemctl enable --now kubelet
```

---

✅ You now have a fully working Kubernetes cluster with Calico networking.

