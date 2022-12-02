# kube-setup

ubuntu T2 medium 1 master and 2 worker and make sure all the traffic is open for the worker 

ssh into the three instances 
update the name of the host 

ALL the server 

sudo hostname master 
sudo hostname worker-node1 
sudo hostname worker-node2

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






