# ✅ Cluster Verification

This step ensures your Kubernetes cluster is up and running correctly.

---

## 🔍 Check Cluster Nodes

On the **control-plane** node, verify all nodes are joined and in `Ready` status:

```bash
kubectl get nodes -o wide
```
You should see:
```bash
NAME            STATUS   ROLES           AGE     VERSION   INTERNAL-IP
controlplane-1  Ready    control-plane   5m      v1.33.x    192.168.178.110
workernode-1    Ready    <none>          3m      v1.33.x    192.168.178.111
workernode-2    Ready    <none>          2m      v1.33.x    192.168.178.112
```
📦 Check System Pods
Verify that all essential pods are running:
```bash
kubectl get pods -n kube-system
```
Expected output:
```bash
coredns, kube-proxy, and calico-* pods should be in Running or Completed state.
```
No pods should be stuck in CrashLoopBackOff or Pending.

🚦 Check Cluster Info
```bash
kubectl cluster-info
```
Output:
```bash
Kubernetes control plane is running at: https://<controlplane-ip>:6443
```
CoreDNS is running at: https://<cluster-ip>/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

🧪 Run a Test Pod
```bash
kubectl run nginx --image=nginx --restart=Never
kubectl get pod nginx
kubectl logs nginx
```
You should see NGINX starting up successfully.

✅ If all checks pass, your cluster is operational and ready for workloads!
