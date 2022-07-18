---
sidebar_position: 2930
title: 🎪🎪🐬☸️ Cluster ➜➜ K3s
---

# K3s




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  LAB. K3s ➜ vps  
🔵 why  

    k8s / minikube need 2G+ ram. 
    k3s only need. 521 MB  + 


🔵 K3s cluster.

    k3s: master
    vps: worker



🔵 Hostname & hosts 

  🔶 hostname 

    hostnamectl set-hostname K3s.MGR
    hostnamectl set-hostname K3s.DKT
    hostnamectl set-hostname K3s.VPS


  🔶 hosts file 

    vi /etc/hosts

    172.16.1.33      K3s.MGR
    172.16.1.144     K3s.DKT
    10.214.214.214   K3s.VPS


      if node use diff ip.  
      just make sure node can ping other nodes.
      choose any ip works.


🔵 install docker:  all node 



🔵 master node

  🔶 install last version   ➜    curl -sfL https://get.k3s.io | INSTALL_K3S_CHANNEL=latest sh -  
  
  🔶 uninstall   - option   ➜    /usr/local/bin/k3s-uninstall.sh

  🔶 check  Status          ➜    systemctl status k3s
  
  🔶 check  Node            ➜    sudo kubectl get nodes -o wide

  🔶 open firewall          ➜    ufw allow 6443,443 proto tcp

  🔶 get join token         ➜    sudo cat /var/lib/rancher/k3s/server/node-token



🔵 work node join k3s 

    curl -sfL http://get.k3s.io | K3S_URL=https://<master_IP>:6443 K3S_TOKEN=<join_token> INSTALL_K3S_CHANNEL=latest sh - 

    curl -sfL http://get.k3s.io | K3S_URL=https://10.214.214.33:6443 K3S_TOKEN=K10cdcb44scaeacc87089a29910422e3d873f2eb4a245fdbc9c14::server:9bb20d4d146766c028555f520af8c243 INSTALL_K3S_CHANNEL=latest sh - 
    curl -sfL http://get.k3s.io | K3S_URL=https://172.16.1.33:6443 K3S_TOKEN=K10cdcbs6as10422e3d873f2eb4a245fdbc9c14::server:9bb20d4d146766c028555f520af8c243 INSTALL_K3S_CHANNEL=latest sh - 


  🔶 leave k3s   ➜     /usr/local/bin/k3s-killall.sh






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 

🔵 lens add k3s 

  /etc/rancher/k3s/k3s.yaml
    change ip inside. 
      server: https://127.0.0.1:6443
      server: https://172.16.1.144:6443



