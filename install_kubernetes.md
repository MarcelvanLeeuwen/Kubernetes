# Install Kubernetes bare metal


#### Before you begin
<ul>
  <li>A compatible Linux host (I use the prefered OS Ubuntu LTS 24.04 because most</li>
  <li>Kubernetes development is done on Ubuntu)</li>
  <li>2GB or more RAM</li>
  <li>2 CPUs or more for control plane machines</li>
  <li>Full network connectivity</li>
  <li>Unique hostname, MAC address</li>
  <li>Certain ports open</li>
</ul>


#### Prefered OS 

Ubuntu Server LTS 24.04 (because most Kubernetes development is done on Ubuntu)



#### Install Ubuntu Server

install
set static ip during installation
reboot
login
```sh
sudo apt-get update && sudo apt-get upgrade -y 
set hostname {name}
```
example: k8s-control.plane-1

#### disable linux swap and remove any existing swap partitions
```sh
sudo swapoff -a
sudo sed -i '/\sswap\s/ s/^\(.*\)$/#\1/g' /etc/fstab
```


#### set required sysctl params, these persist across reboots
```sh
sudo nano /etc/modules-load.d/containerd.conf
```
Add:

overlay
br_netfilter
```sh
sudo modprobe overlay
sudo modprobe br_netfilter


sudo nano /etc/sysctl.d/99-kubernetes-cri.conf
```
Add:

net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1



#### Apply sysctl params without reboot
```sh
sudo sysctl --system
```


#### install containerd
```sh
sudo apt-get install docker.io containerd
```


#### install containerd latest version over apt-installed-version
```sh
sudo systemctl stop containerd

sudo wget https://github.com/containerd/containerd/releases/download/v2.1.1/containerd-2.1.1-linux-amd64.tar.gz

sudo tar xvf containerd-2.1.1-linux-amd64.tar.gz

sudo mv bin/* /usr/bin

sudo rm -rf bin containerd-2.1.1-linux-amd64.tar.gz
```


#### containerd config
```sh
sudo mkdir -p /etc/containerd

sudo nano /etc/containerd/config.toml
```
copy and paste following text into config.toml

version = 2
[plugins]
  [plugins."io.containerd.grpc.v1.cri"]
    [plugins."io.containerd.grpc.v1.cri".containerd]
      discard_unpacked_layers = true
      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
          runtime_type = "io.containerd.runc.v2"
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            SystemdCgroup = true
```sh
sudo systemctl start containerd
```


#### install required packages
```sh
sudo apt-get install apt-transport-https
```


#### install kubernetes packages
```sh
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install kubelet kubeadm kubernetes-cni kubectl #### control plane

sudo apt-get install kubelet kubeadm kubernetes-cni #### worker
```


#### crictl uses containerd as default
```sh
echo "runtime-endpoint: unix:///run/containerd/containerd.sock" | sudo tee /etc/crictl.yaml
```


#### kubelet should use containerd
```sh
echo 'KUBELET_EXTRA_ARGS="--container-runtime-endpoint=unix:///run/containerd/containerd.sock"' | sudo tee /etc/default/kubelet > /dev/null
```


#### start services
```sh
sudo systemctl daemon-reload
sudo systemctl enable containerd
sudo systemctl restart containerd
sudo systemctl enable kubelet && systemctl start kubelet
```


#### init k8s control plane
```sh
sudo kubeadm init 
```
To start using your cluster, you need to run the following as a regular user:
```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Alternatively, if you are the root user, you can run:
```sh
export KUBECONFIG=/etc/kubernetes/admin.conf
```


#### CNI (install calico network plugin ** required **)
```sh
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/calico.yaml
```


#### COMMAND TO ADD A WORKER NODE
```sh
kubeadm token create --print-join-command --ttl 0
```


#### Change roles labe
```sh
kubectl label node k8s-worker-1 node.role.kubernetes.io/worker=worker / for some reason the "roles" doesn't change after this command.
```