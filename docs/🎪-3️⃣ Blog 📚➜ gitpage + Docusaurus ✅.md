---
sidebar_position: 113
---


# 🎪-3️⃣ Blog 📚➜ gitpage + Docusaurus ✅.md


🔵 Hardware 

    xxxx.x    Starlink
    0219.1    FortiGate    60F

    0219.11   Mikrotik     RB4011 
    0219.22   Mikrotik     CRS328

    0219.33   Mikrotik     AP-Master
    1928.40   Ruckus       AP-Guest-Mesh_01
    1928.41   Ruckus       AP-Guest-Mesh_02

    0219.13   HP-Zbook_G3  Esxi-G3 
    0219.15   HP-Zbook_G5  Esxi-G5

    1001.88   Synology     NAS
    0099.xx   Camera*6






🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐 🦚 NET

🔵 Structure

    ✅ VPN:            Wireguard + Netmaker
    ✅ DNS:            AdGuard 
    ✅ Reverse Proxy:  Traefik
    🚫 VXLAN:          NSXT      (use too much cpu ram)


🔵 VPN ✅
    vps.s 1214.214

    ros.c 1214.011
    ros.c 1214.022

    k3s.c 1214.033
    dkt.c 1214.144
    mac.c 1214.099



🔵 VLAN 

    MGR_1219   10.219.219.0/24     Manager
    OWN_1111   10.111.111.0/24     Owner

    Gst_0168   192.168.168.0/24    Guest Wifi

    Srv_1721   172.16.1.0/24       Server 
    Srv_1728   172.16.8.0/24       Server 

    STO_1001   10.1.1.0/24         NAS_01G
    STO_1010   10.10.10.0/24       NAS_10G
    STO_1012   10.12.12.0/24       CEPH

    SEC_0099   192.168.99.0/24     Camera





🔵 IP

001 ★ Firewall  
011 ★ RB4011
022 ★ CRS328
013 ★ AP

088 ★ NAS
089 ✩ NAS


099 ★ iMAC


070 ✩ CEPH-S
077 ✩ CEPH-C

1720.080 K8s-S
1720.083 K8s-C 
1721.033 K3s 



111 ✩ Win7-Canmera 
123 ✩ HomeAssist

140 ✩ Docker_Prod
144 ✩ Docker_Test 

012 ✩ Ros-CHR       ???  nsx?







☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️☸️  K8s 

K8s.M  172.16.0.80 
K8s.C  172.16.0.83 


K3s.MGR  172.16.1.33
K3s.DKT  172.16.1.144
K3s.VPS  10.214.214.214




📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀 🦚 STO
    ✅NAS:   Synology
    ✅S3:    MinIO 
    ✅RBD:   Ceph



🔵 CEPH-RBD 

    Pool_BD-K8s-DB
    Pool_BD-K8s-APP




    Pool_BD-K3s-AIO
        PVC:  pvc-cephrbd-k3s-db-mysql
        PVC:  pvc-cephrbd-k3s-db-
        PVC:  pvc-cephrbd-k3s-db
        PVC:  pvc-cephrbd-k3s-app
        PVC:  pvc-cephrbd-k3s-app






DVM-Docker-DB: mariabd 




🔵 NAS 
    🔶 webdav ➜ for  omnifocus
    








🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐 Auth  LDAP & SSO 

LDAP: openLDAP  ✅   ad.rv.ark
LDAP:  Synology ✅   adnas.rv.ark


Radius          ✅   RB4011 
SSO:  Authelia  ❌


🔵 LDAP Account 

🔶 nas 
adu.nas ➜   user 
ada.nas ➜   admin 






🔵 LDAP client
    Mikrotik. AP  ❌ 


vpn. 

jekenis 
jumpserver 
jira / confluence 
nightingale 
gitlab 
grafana 
wifi..


🔵 Radius 
    1. AP-Guest ✅





🔵 SSO 



🔵 ACCT   

nas - dhw     ada.nas / adu.nas  




🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢 🦚 DB 






💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠 🦚 APP 

✅ dashy 

✅ Blog.W  - wikijs 
✅ Blog.LO - Docusaurus


HomeAssisit 



🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰 🦚 Tool  

 K8s:  GUI:  lens   

✅ code-server:    remote config server in web vscode



✅ Docker: xx  lazydocker.   cmd manager 



✅ DB Redis-CLI GUI     redis-insight







🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉 🦚 Misc / ToDo




💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠 🦚 Moniter 

metric + influxdb + grafana
通过 Prometheus 采集数据




💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠 🦚 ABBR 

tl;dr (TL;DR)      too long don`t remeber..


🦚  ;h0
🔵  ;h1  
🔶  ;h2 
🔻  ;h3


vsc disable auto indentation
On Mac Shift + Option + F
