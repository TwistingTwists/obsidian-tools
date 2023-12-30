---
tags:
  - kubernetes
  - devops
---
### Preparing for the exam: 

```bash

## vim tips 

cat > ~/.vimrc << EOF
set ts=2 sts=2 sw=2 et nu ai cuc cul 
EOF

# use SHIFT C to change the word below cursor 
# use SHIFT V -> y (yank) -> p (paste)

alias hg="history|grep"

mkdir `seq -f "%02g" 20`

alias kb="kubectl"

## have 1.yaml , 2.yaml in each folder. 
kb create -f 1.yaml 

## Using cli to take help 

kb explain pod.spec --recursive | grep -i nodesel -C5 
# -C5 = shows 5 lines above and below the search 


# systemctl 

systemctl status kubelet # check service status 
systemctl enable kubelet 

systemctl daemon-reload 


## ssh tips 
# if you become root, remember to exit root 


```


### creating a master and slave in kubernetes - ( ec2 )

```bash 

## add modules for containerd

cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF


sudo modprobe overlay 
sudo modprobe br_netfilter

# networking config

cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1 
net.ipv4.ip_forward = 1 
net.bridge.bridge-nf-call-ip6tables = 1 
EOF

sudo sysctl --system 


## install containerd
sudp apt-get update && sudo apt-get install -y containerd 

## default config file for containerd

sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml


sudo systemctl restart containerd
## verify that containerd is running 
sudo systemctl status containerd


## disable swap 

sudo swapoff -a 
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

```


