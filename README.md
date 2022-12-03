# kube-setup

ubuntu T2 medium 1 master and 2 worker and make sure all the traffic is open for the worker 

ssh into the three instances 
update the name of the host 

ALL the server 

sudo hostname master 

sudo hostname worker-node1 

sudo hostname worker-node2

sudo su 

apt update -y

apt install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

apt update -y

apt-cache policy docker-ce

apt install -y docker-ce

###ls /etc to check docker folder

## setting docker in systemd to use systemctl commands 
cat <<EOF | sudo tee /etc/docker/daemon.json
{
"exec-opts": ["native.cgroupdriver=systemd"]
}
EOF
systemctl enable --now docker

usermod -aG docker ubuntu
## restart docker deamon 
systemctl restart docker

swapoff -a

##pesist swap config using fstab
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

sysctl net.bridge.bridge-nf-call-iptables=1

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF


apt update -y

apt install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl

echo "source <(kubectl completion bash)" >> ~/.bashrc


mv /etc/containerd/config.toml /etc/containerd/config.toml.bak
ls
systemctl restart containerd


### At the level of master to pull down images that are serving (Api server, kube controler manager, kube scheduler)

kubeadm config images pull

kubeadm init

exit as root then run this 

mkdir -p $HOME/.kube 

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

export KUBECONFIG=/etc/kubernetes/admin.conf



###setting up weave download weave service  
https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#install
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml


systemctl restart kubelet 



### Run this on the worker node 
run the command key token on the worker node

systemctl restart kubelet 
