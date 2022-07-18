# 🎪 HomeLAB Build Doc 🎪


Blog Link:  https://mirandaxx.github.io



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






🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐🌐 NET

🔵 Structure

    ✅ VPN:            Wireguard + Netmaker
    ✅ DNS:            AdGuard 
    ✅ Proxy:          Traefik
    🚫 VXLAN:          NSXT      (use too much cpu ram)


🔵 VPN

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





🔵 IP Tables

    xxxx.001 ★ Firewall  

    xxxx.011 ★ RB4011
    xxxx.012 ✩ CHR   
    xxxx.022 ★ CRS328
    1111.013 ★ AP

    xxxx.088 ★ NAS.HW
    xxxx.089 ✩ NAS.VM

    1720.070 ✩ CEPH.S
    1720.077 ✩ CEPH.C

    1720.080 ✩ K8s.S
    1720.083 ✩ K8s.C

    1721.033 ✩ K3s.S.MGR
    1721.144 ✩ K3s.C.DKT
    1214.214 ★ K3s.C.VPS


    1111.099 ★ iMAC
    0099.111 ✩ Win7-Canmera 
    1721.123 ✩ HomeAssist






📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀📀 STO

    ✅ NAS:   Synology
    ✅ S3:    MinIO 
    ✅ RBD:   Ceph



🔵 CEPH-RBD 

    Pool_BD-K8s-DB
    Pool_BD-K8s-APP

    Pool_BD-K3s-AIO




🔵 NAS 

    🔶 NAS.HW 


    🔶 NAS.VM
        - Docker 

        - Cloud Sync: 
            Dropbox       * 4
            Google Driver * 2





🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐🔐 Auth


    ✅ LDAP:  openLDAP     ad.rv.ark
    ✅ LDAP:  Synology    adnas.rv.ark

    ✅ Radius  RB4011 

    ❌ SSO:    Authelia




🔵 LDAP Account 

    🔶 nas 
        adu.nas ➜   user 
        ada.nas ➜   admin 




🔵 LDAP client

    Mikrotik. AP  ❌ 



🔵 Radius 

    ✅ AP-Guest



🔵 SSO 





🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢🛢 DB 






💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠 APP 

✅ dashy 






🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰🧰 Tool  

 K8s:  GUI:  lens   

✅ code-server:    remote config server in web vscode

✅ DB Redis-CLI GUI     redis-insight







🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉 Misc / ToDo




💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠💠 Moniter 

metric + influxdb + grafana
通过 Prometheus 采集数据


