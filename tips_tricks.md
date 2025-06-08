# Tips & Trick
<br>

### Connect old k8s-worker to new control plane
<br>
Remove old configuration data

```sh
sudo kubeadm reset
```

```sh
sudo rm -rf /etc/cni/net.d
```

```sh
sudo rm -rf ~/.kube
```

```sh
sudo rm -rf /var/lib/kubelet/*
```
<br>
Done with wipping old configuration data
<br>
<br>

Command to add node. Execute this on the control plane.
<br>

```sh
kubeadm token create --print-join-command --ttl 0
```
<br>
Use output to add node. So execute this command on the node to add.
<br>
<br>

### Control your cluster from your client  **** CHECK ****
<br>
Install Kubectl on your client --> add how-to
<br>
<br>
copy the config file from ~/.kube to your home directory on your client.
<br>
<br>
Location Macbook:
<br>
<br>

```sh
mkdir -p /Users/{username}/.kube
```
<br>
<br>
check if it works:
<br>
<br>

```sh
kubectl get nodes
```