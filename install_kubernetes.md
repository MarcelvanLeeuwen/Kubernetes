# WARNING: I'm still debuging this guide.

[How to control cluster from client](client.md)



### Before you begin

- A compatible Linux host (I use the prefered OS Ubuntu LTS 24.04 because most Kubernetes development is done on Ubuntu).
- 2GB or more RAM
- 2 CPUs or more for control plane machines
- Full network connectivity
- Unique hostname, MAC address
- Certain ports open



### Prefered OS 

Ubuntu Server LTS 24.04 (because most Kubernetes development is done on Ubuntu)



### Install Ubuntu Server

install
set static ip during installation
is it possible to set hostname during installation? I guess it is.., check!
reboot
login
sudo apt-get update && sudo apt-get upgrade -y 
set hostname {name}  (k8s-contol-plane-1, example)



### disable linux swap and remove any existing swap partitions

sudo swapoff -a
sudo sed -i '/\sswap\s/ s/^\(.*\)$/#\1/g' /etc/fstab



### set required sysctl params, these persist across reboots

sudo nano /etc/modules-load.d/containerd.conf

Add:

overlay
br_netfilter

sudo modprobe overlay
sudo modprobe br_netfilter


sudo nano /etc/sysctl.d/99-kubernetes-cri.conf

Add:

net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1



### Apply sysctl params without reboot
sudo sysctl --system



### install containerd

sudo apt-get install docker.io containerd



### install containerd latest version over apt-installed-version

sudo systemctl stop containerd

sudo wget https://github.com/containerd/containerd/releases/download/v2.1.1/containerd-2.1.1-linux-amd64.tar.gz

sudo tar xvf containerd-2.1.1-linux-amd64.tar.gz

sudo mv bin/* /usr/bin

sudo rm -rf bin containerd-2.1.1-linux-amd64.tar.gz



### containerd config

sudo mkdir -p /etc/containerd

sudo nano /etc/containerd/config.toml

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

sudo systemctl start containerd



### install required packages

sudo apt-get install apt-transport-https



### install kubernetes packages

sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install kubelet kubeadm kubernetes-cni kubectl #### control plane

sudo apt-get install kubelet kubeadm kubernetes-cni #### worker



### crictl uses containerd as default

echo "runtime-endpoint: unix:///run/containerd/containerd.sock" | sudo tee /etc/crictl.yaml



### kubelet should use containerd

echo 'KUBELET_EXTRA_ARGS="--container-runtime-endpoint=unix:///run/containerd/containerd.sock"' | sudo tee /etc/default/kubelet > /dev/null



### start services

sudo systemctl daemon-reload
sudo systemctl enable containerd
sudo systemctl restart containerd
sudo systemctl enable kubelet && systemctl start kubelet



### init k8s control plane

sudo kubeadm init 

To start using your cluster, you need to run the following as a regular user:

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


Alternatively, if you are the root user, you can run:

export KUBECONFIG=/etc/kubernetes/admin.conf



### CNI (install calico network plugin ** required **)

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/calico.yaml



### COMMAND TO ADD A WORKER NODE

kubeadm token create --print-join-command --ttl 0



### Change roles labe

kubectl label node k8s-worker-1 node.role.kubernetes.io/worker=worker / for some reason the "roles" doesn't change after this command.