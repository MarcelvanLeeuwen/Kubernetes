# **** WARNING: this is not a complete guide yet. I'm still investigating ****




#### Before you begin

- A compatible Linux host
- 2GB or more RAM
- 2 CPUs or more for control plane machines
- Full network connectivity
- Unique hostname, MAC address
- Certain ports open


#### Prefered OS 

Ubuntu Server LTS 24.04 (because most Kubernetes development is done on Ubuntu)


#### Install Ubuntu Server

 - install
 - set static ip during installation
 - is it possible to set hostname during installation? I guess it is.., check!
 - reboot

 - login 
 - set hostname
    - set hostname k8s-contol-plane-1 (example)

 - disable swap file
    - sudo swapoff -a
    - Comment it out by adding a # in front # /swap.img none swap sw 0 0
    
    
#### Install container runtime

    - sudo apt install docker.io -y


#### Install Kubelet, Kubeadm, Kubectl

 - sudo apt-get install -y apt-transport-https ca-certificates curl gpg

 - curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

 - echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

 - sudo apt-get update

 - sudo apt-get install -y kubelet kubeadm kubectl

 - sudo apt-mark hold kubelet kubeadm kubectl --> tells the APT package manager to prevent these three packages from being automatically upgraded

 - sudo systemctl enable --now kubelet



# NIEUW


#### disable linux swap and remove any existing swap partitions

sudo swapoff -a
sudo sed -i '/\sswap\s/ s/^\(.*\)$/#\1/g' /etc/fstab



#### set required sysctl params, these persist across reboots

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

sudo sysctl --system


### install required packages

sudo apt-get install apt-transport-https ca-certificates #curl gnupg lsb-release software-properties-common wget jq


#### install kubernetes packages

- curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

apt-get update

apt-get install kubelet kubeadm kubectl



#### install containerd

sudo apt-get install docker.io containerd


#### install containerd over apt-installed-version

sudo systemctl stop containerd

sudo wget https://github.com/containerd/containerd/releases/download/v2.1.1/containerd-2.1.1-linux-amd64.tar.gz

sudo tar xvf containerd-2.1.1-linux-amd64.tar.gz

sudo mv bin/* /usr/bin

sudo rm -rf bin containerd-2.1.1-linux-amd64.tar.gz

sudo systemctl start containerd
