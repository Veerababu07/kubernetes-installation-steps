Kubernetes installation 
Launch master and server nodes {million}

Install docker and all master and nodes
Docker install script:
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

install Kubernetes agent kubeadm marign Kubernetes

clone
git clone https://github.com/Mirantis/cri-dockerd.git

commands
# Run these commands as root
###Install GO###

wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile

cd cri-dockerd
mkdir bin
go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket

after that install kubeadm in master and node

1.	Update the apt package index and install packages needed to use the Kubernetes apt repository:
   sudo apt-get update
   sudo apt-get install -y apt-transport-https ca-certificates curl
2.	Download the Google Cloud public signing key:
sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
3.	Add the Kubernetes apt repository:
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-        keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

4.	Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
   sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl




in master we need to have a networking component for one of the component we use is fannel but applying cd .. from

kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

apply command in root user

we get some commands and join nodes to cluster command

note: those commands should be used only if you want to run the cluster in normal user apart from root user
be catious while using these commands

mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config


join command use in nodes to join to the cluster network

kubeadm join 172.31.17.18:6443 --token fguie7.c0ob8edfrud9hx3k \                                                   
 --discovery-token-ca-cert-hash sha256:9ae6d165a981764a0860309719c2563f5aedd7bf011e405c88cc64e7e712f3fe --cri-socket "unix:///var/run/cri-dockerd.sock"

Add this to end of your join command
--cri-socket "unix:///var/run/cri-dockerd.sock"


