# 🧩 Join Worker Nodes to the Cluster

This step walks you through how to join your worker nodes (`workernode-1`, `workernode-2`) to the Kubernetes control-plane.

---

## 📋 Get the `kubeadm join` Command

On the **control-plane node**, run:

```bash
kubeadm token create --print-join-command
```
Example output:
```bash
kubeadm join 192.168.178.110:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:<HASH>
```
📌 Copy this full command. You’ll use it on each worker node.

🖥️ Join Each Worker Node
On each worker node, paste and run the full kubeadm join command:
```bash
kubeadm join 192.168.178.110:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:<HASH>
```
🖥️ Join Each Worker Node
On each worker node, paste and run the full kubeadm join command:
```bash
kubeadm join 192.168.178.110:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:<HASH>
```
You should see a message like:
```bash
This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.
```
✅ Verify Nodes on Control Plane
Back on the control-plane node, confirm all nodes have joined:
```bash
kubectl get nodes -o wide
```
Expected output:

```bash
NAME             STATUS   ROLES           AGE     VERSION   INTERNAL-IP     
controlplane-1   Ready    control-plane   10m     v1.33.x    192.168.178.110
workernode-1     Ready    <none>          1m      v1.33.x    192.168.178.111
workernode-2     Ready    <none>          1m      v1.33.x    192.168.178.112
```
✅ All nodes should be in Ready state. If not, troubleshoot via kubectl describe node <name> and journalctl -xeu kubelet.
