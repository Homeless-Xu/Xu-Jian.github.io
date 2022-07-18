---
sidebar_position: 1930
title: 🎪-9️⃣📀📀📀 CEPH ➜ Build
---

# Storage ✶ CEPH ➜ Build

⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️
https://www.ityww.cn/1588.html



🔵 VM Prepair   ✅

    🔶 K8s Role Desc 

        only two kinds Role :  manager and worker(osd) 
            no need manual set/choose monitor node 
                during install. ceph automatic choose/set all the role.


    🔶 VM System Version 

        https://docs.ceph.com/en/quincy/start/os-recommendations/

        ‼️ Ceph 17.2 is test on Ubuntu_20 only.  No Ubuntu_22 yet ‼️
        ‼️ Ceph 17.2 is test on Ubuntu_20 only.  No Ubuntu_22 yet ‼️
        ‼️ Ceph 17.2 is test on Ubuntu_20 only.  No Ubuntu_22 yet ‼️


    🔶 VM Number: must at least two   

        CEPH-Mgr    x 1  

        CEPH-Node   x 1 
            3 x 345G  Physical iscsi disk from nas 
            1 x 1T    Physical iscsi disk from nas 


    🔶 VM NIC 

        ◎ Manager
            VLAN-STO-1012-CEPH_internel   10.12.12.0/24     Must   ➜  ceph cluster internal commute.

        ◎ Worker
            VLAN-STO-1010-NAS_ISCSI       10.10.10.0/24     Option ➜  nas. provide disk to worker
            VLAN-STO-1012-CEPH_internel   10.12.12.0/24     Must   ➜  ceph cluster internal commute.



    🔶 VM Basic Setup 

        ◎ Static IP 
        ◎ update apt Source 
        ◎ install docker 




🔵 VM Mount iscsi disk   ✅

    https://manjaro.site/how-to-connect-to-iscsi-volume-from-ubuntu-20-04/

    🔶 iSCSI Initiator Install 

        sudo apt install open-iscsi


    🔶 Config network card for iscsi 

        ◎ why 
            we have three nic.
            give nic a nickname  
            easy for manager.



    network:
    ethernets:
        ens160:
        match:
            macaddress: 00:50:56:85:b8:cc
        set-name: Nic-CEPH_1012
        dhcp4: false
        addresses: [10.12.12.77/24]
        gateway4: 10.12.12.11
        nameservers:
            addresses: [10.12.12.11,10.12.12.1]
        optional: true
        ens224:
        match:
            macaddress: 00:50:56:85:ec:43
        set-name: Nic-NAS_1010
        dhcp4: false
        addresses: [10.10.10.77/24]
        optional: true
        ens192:
        match:
            macaddress: 00:50:56:85:55:a5
        set-name: Nic-CEPH_1011
        dhcp4: false
        addresses: [10.11.11.77/24]
        optional: true
    version: 2





    🔶 config iscsi auth 

        ◎ if you nas need chap.  do this.    otherwise skip.
            sudo vi /etc/iscsi/iscsid.conf
                node.session.auth.authmethod = CHAP
                node.session.auth.username = username
                node.session.auth.password = password


    🔶 enable server on boot 

        sudo systemctl enable open-iscsi
        sudo systemctl enable iscsid


    🔶 Discover remote nas iscsi target

        CEPH-99 ~ sudo iscsiadm -m discovery -t sendtargets -p 10.10.10.88
        10.10.10.88:3260,1 iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb
        10.10.10.88:3260,1 iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH22.b6f8cfe4bdb
        10.10.10.88:3260,1 iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH33.b6f8cfe4bdb


    🔶 manual connect target 

    sudo iscsiadm --mode node --targetname iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb --portal 10.10.10.88 --login
    sudo iscsiadm --mode node --targetname iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH22.b6f8cfe4bdb --portal 10.10.10.88 --login
    sudo iscsiadm --mode node --targetname iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH33.b6f8cfe4bdb --portal 10.10.10.88 --login


    🔶 Comnand 
        
        ◎ restart services                 sudo systemctl restart iscsid open-iscsi 
        ◎ List connected iscsi node        sudo iscsiadm -m node -l
        ◎ Disconnect all iscsi node        sudo iscsiadm -m node -u

        ◎ List Disk                        sudo fdisk -l
            now you should see your iscsi disk at here.
            next is auto connect iscsi tatget at boot.





    🔶 auto mount:  Config iscsi node 

        use root user.
        cd cd /etc/iscsi/nodes
            config every iscsi taeget you need auto connect.
        enter iscsi tagert  config default file.
            change 
            node.startup = manual    to     node.startup = automatic
                sed -i 's/node.startup = manual/node.startup = automatic/g' ./default

                cat ./default | grep node.startup
                    node.startup = automatic

            reboot server. check result.


    CEPH-99 ~ sudo su -
    CEPH-99.Root ~ cd /etc/iscsi/nodes
    CEPH-99.Root nodes ls
    iqn.2000-01.com.synology:NAS-DS2015XS.Target-0000.b6f8cfe4bdb
    iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb
    iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH22.b6f8cfe4bdb
    iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH33.b6f8cfe4bdb
    CEPH-99.Root nodes cd iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb
    CEPH-99.Root iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb ls
    10.10.10.88,3260,1
    CEPH-99.Root iqn.2000-01.com.synology:NAS-DS2015XS.Target-CEPH11.b6f8cfe4bdb cd 10.10.10.88,3260,1
    CEPH-99.Root 10.10.10.88,3260,1 ls
    default
    



🔵 All Node: NTP & Timezone ✅

    🔶 NTP 

        ◎ status                  sudo systemctl status ntp
        ◎ add local ntp server    sudo vi /etc/ntp.conf 
        ◎ restart                 sudo service ntp restart
    

    🔶 Timezone 

        timedatectl list-timezones
        sudo timedatectl set-timezone America/Los_Angeles



🔵 All Node: Hostname & Hosts

    🔶 Change Hostname 

        hostnamectl set-hostname  CEPH-Mgr
        hostnamectl set-hostname  CEPH-Node 

        ‼️ ceph host name need like  CEPH-Node03   not  CEPH-Node03.ark (fqdn) ‼️
    

    🔶 Config Hosts file 

        >  /etc/hosts   means replace 
        >> /etc/hosts   means append  


    sudo bash -c 'echo "
    127.0.0.1   localhost
    10.12.12.70 CEPH-Mgr
    10.12.12.77 CEPH-Node" > /etc/hosts'







🔵 Mgr Node: install Tool: Cephadm

    🔶 Ubuntu 20

        sudo apt install -y cephadm


🔵 Mgr Node: deploy Monitor 

        ‼️ Moniter must install in local machine which have cephadm installed. here: Manger node ‼️
            ip choose ceph internel vlan. 

    sudo cephadm bootstrap --mon-ip 10.12.12.70

            during install . remember password. 

	     URL: https://CEPH-Mgr:8443/
	    User: admin
	Password: r3uq4z2agg


    🔶 what this do

        create ssh key. 
        later you need upload this public key to other node

        and create a lot docker to mgr. 


🔵 Monitor web login 

    https://10.12.12.70:8443/
    https://10.12.12.70:3000/
    


🔵 Monitor Enable ceph command 

    sudo cephadm install ceph-common

        if no this.
            you can not use ceph command under ceph-mgr     
                you have to enter cephadm shell first
                    then use ceph command under cephadm shell
                        sudo cephadm shell
                        ceph -s

            

 
🔵 upload ssh key To other node 

    cephadm already create key for you . 
    no need crtate ssh key yourself .

    as for user we use root.
    by default root is disabled in ubuntu 


    🔶 ALL Node: Config root User

        ◎ Enable root 
            sudo passwd root

        ◎ Allow Root ssh 

            sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
            sudo service sshd restart


    🔶 CEPH-Mgr Upload public key to other node 

        ssh-copy-id -f -i /etc/ceph/ceph.pub root@CEPH-Node


        
 
🔵 Add node to ceph Cluster. 

    🔶 add 

        CEPH-Mgr ~ ceph orch host add CEPH-Node 


    🔶 check host 

        CEPH-Mgr ~ sudo ceph orch  host ls
        HOST       ADDR       LABELS  STATUS
        CEPH-Mgr   CEPH-Mgr
        CEPH-Node  CEPH-Node


🔵 Auto Deploy Monitor 

    chepadm will automatic deploy manager & monitor node to some host under cluster.
    ceph need many manager & monitor for high ability.
    just wait. you need do nothing here. 
    no need manual choose which host use what service (manager / monitor)


    CEPH-Mgr ~ sudo ceph -s
    cluster:
        id:     b57b2062-e75d-11ec-8f3e-45be4942c0cb
        health: HEALTH_WARN
                OSD count 0 < osd_pool_default_size 3

    services:
        mon: 1 daemons, quorum CEPH-Mgr (age 17m)
        mgr: CEPH-Mgr.ljhrsr(active, since 16m), standbys: CEPH-Node.lemojs
        osd: 0 osds: 0 up, 0 in




🔵 OSD  setup .

🔶 All Node Check available disk 

    i only use ceph-node mount all physical disk.

    CEPH-Node ~ lsblk
        sdb                         8:16   0  345G  0 disk
        sdc                         8:32   0  345G  0 disk
        sdd                         8:48   0  345G  0 disk


 
🔶 add disk to ceph cluster 

    in ceph-mgr 

    sudo ceph orch daemon add osd CEPH-Node:/dev/sdb 
    sudo ceph orch daemon add osd CEPH-Node:/dev/sdc
    sudo ceph orch daemon add osd CEPH-Node:/dev/sdd


        if disk not empty. need use blow command first
         sgdisk --zap-all /dev/sdxxxx





