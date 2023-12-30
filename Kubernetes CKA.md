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



```
