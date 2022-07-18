---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️5️⃣ NET ➜ Basic
---


# NET ✶ Demo








🔵 K8s Network ✅

    wan traffic:   inside cluster <> outside cluster 
    lan traffic:   inside cluster <> inside  cluster 


    wan - level 4  loadbalance:  ➜   protocal +  port  to find pod  ➜ 1. LoadBalance
    wan - Level 7  loacbalance:  ➜   domain   +  url   to fins pod  ➜ 2. Ingress 
    lan -                        ➜   podname           to find pod  ➜ 3. ClusterIP 
    Docker                       ➜   Host                           ➜ 4. NodePort   




        loadbalance you can choose any you like:  nginx/traefik/HA 



🔵 How config network 

    not like docker . neet create docker network first ??
    





🔵 why 

    in one node. container network is easy
    in cluster.  container network is much hard.
        need connect pod in different physical node. 


    vlan  is virtual network over real network.
    vxlan is virtual network over vlan.

    k8s cluster use vxlan. so k8s will not impact your real vlan.
    just like docker network will not impact host network.

    so overlay/vxlan is just something like vlan. very easy. 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

📗 
    https://kubernetes.io/zh-cn/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/

    https://morningspace.github.io/tech/k8s-net-cni/

    https://cloud.tencent.com/developer/article/1804680



🔵 K8s default network 

    none 
    host 
    default bridge 
    custom ...      



    k8s use CNI to build cluster network. 
        CNI: Container Network Interface.
        CNI  have lots plugin to use. 
            Flannel  ➜ overlay 
            Calico





🔵 Overlay / Route / Underlay 




🔵 network server /network permit 

    any pod`s traffic (lan + wan)  
    need set/config  permit/service  first. 
    otherwise default is deny all.
















