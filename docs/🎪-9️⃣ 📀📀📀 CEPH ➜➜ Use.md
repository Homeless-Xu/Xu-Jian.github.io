---
sidebar_position: 1930
title: 🎪-9️⃣📀📀📀 CEPH ➜➜ Use
---

# # Storage ✶ CEPH ➜ Use



🔵 Good File 

⭐️⭐️⭐️⭐️⭐️
https://zhangzhuo.ltd/articles/2021/06/18/1623993444076.html


⭐️⭐️⭐️⭐️
https://www.koenli.com/ef5921b8.html



https://blog.51cto.com/renlixing/3134294


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Basic Info 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Basic Info
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Basic Info

🔵 POOL PG PGP Desc  

    🔶 Pool 

        ceph osd pool create poolname PG PGP 
        ceph osd pool create mytestpool 128 128 

        创建pool 时候必须指定 PG 数量.
        PG 数量 是自动分布到 所有 OSD的.
        所以你的 Pool 也是分布到整个 集群的.



    🔶 PGs Desc  ✅

        ceph cluster have lots physical disk.
            to manager disk easyier. we create PGs ( assign physical disk to some group )
                so we only need manage PGs.



    🔶 PG / PGP 

        PG 要根据 OSD 数量进行调整.
        PGP 和 PG 的值保持一致就行.
        改PG 的同时要 PGP 一起改.


        少于50 osd 可以用自动方式. 再多就要自己算了. 
            pg_autoscale_mode: on   就可以了

    

🔵 Storage Develop 

    DAS:  Dirtect Attached Storage 
        ➜ NAS:  Network Attached Storage 
            ➜ SAN:  Storage Area Network 
                ➜ Object Storage



🔵 CEPH Function:

    Object Storage ➜ like AWS S3    ➜ For Share              ➜ high speed 
    Block  Storage ➜ like iscsi     ➜ For One Device Only    ➜ Fast Speed 
    File   Storage ➜ like NFS/SMB   ➜ For Share              ➜ Slow Speed

        object storage have both advantage of  block storage and file storage.
            if data that don`t change too often.    ➜ Object storage is best.  like picture/video 
            if you install VM                       ➜ Block Storage is better.



 

🔵 CEPH FIle  Storage Workflow

    1. File   >> Object           cut big file to samll object(2M-4M)
        2. Object >> PG           give small object a PGs.
            3. PG     >> OSD      Write  small object to OSD


 
🔵 NEW OSD 

    when add new driver.
    it will automatic move date between osdes. 



🔵 Pool Commands 

    🔶 Pool 

        ◎ Create pool     ceph osd pool create CEPH_FS-APP 128 128 
        ◎ Rename Pool     ceph osd pool rename CEPH-Test CEPH_FS-NAS-Ceph
        ◎ List   pool     ceph osd lspools
        ◎ Delete Pool




    🔶 AutoScale Mode  

        ◎ Set Mode On    ceph osd pool set CEPH-Test pg_autoscale_mode on
        ◎ Check Mode     ceph osd pool autoscale-status
            default: on 


    🔶 Pool Size 

        ceph osd pool set CEPH-Test target_size_bytes 10G



    🔶 Pool replica size 

        ceph osd pool set CEPH-Test size 2
        
        default is 3 
        you can change this anytime.
        if you have space set 3/2
        if no space set to 1 at any time.


    🔶 Delete Pool ✅

        ◎ Allow Delete pool 
            by default it is not allowed.

            ceph tell mon.\* injectargs '--mon-allow-pool-delete=true'


        ◎ How Delete 

            ceph osd pool delete {pool-name} {pool-name} --yes-i-really-really-mean-it
                type pool name twice. 
                    and follew --yes-i-really-really-mean-it

            ceph osd pool delete test-pool test-pool --yes-i-really-really-mean-it

            ceph osd pool delete OB-Zone-Ark.rgw.meta OB-Zone-Ark.rgw.meta --yes-i-really-really-mean-it




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Filesystem Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Filesystem Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Filesystem Demo 

🔵 Server ✶ Pool Config 

    🔶 Create Pool for cephfs 

        ceph osd pool create CEPH_FS-NAS-Ceph 128 128 
        ceph osd pool create CEPH_FS-NAS-Ceph-metadata 128 128 


    🔶 Assign Pool Type to Filesystem Storage

        ◎ Desc 
            sudo ceph osd pool application enable PoolNAME StorageType
                ceph have tree storage type: file Type /Block Devie Type /Object Type.
                so you need set pool use which type.
                here cephfs type.
     
        ◎ Demo 
            sudo ceph osd pool application enable CEPH_FS-NAS-Ceph cephfs
            sudo ceph osd pool application enable CEPH_FS-NAS-Ceph-metadata cephfs


    🔶 Create fs:  cephfs

        ceph fs new cephfs CEPH_FS-NAS-Ceph-metadata CEPH_FS-NAS-Ceph


    🔶 Check fs status

        CEPH-MGR.Root ~ ceph fs ls
        name: cephfs, metadata pool: CEPH_FS-NAS-Ceph-metadata, data pools: [CEPH_FS-NAS-Ceph ]


    🔶 assign cephfs to node

        ceph orch apply mds cephfs --placement="1  CEPH-Node"


    🔶 check mds status 

        CEPH-MGR.Root ~ ceph orch ps --daemon-type mds
        NAME                         HOST       STATUS         REFRESHED  AGE  VERSION  IMAGE NAME             IMAGE ID      CONTAINER ID
        mds.cephfs.CEPH-Node.tevxqv  CEPH-Node  running (33m)  4m ago     33m  15.2.16  quay.io/ceph/ceph:v15  8d5775c85c6a  6e5f7903011d


🔵 Server ✶ User Create 

    🔶 Create user for ceph-fs mount

        ceph auth get-or-create client.miranda mon 'allow r' mds 'allow r, allow rw path=/' osd 'allow rw pool=data' -o ceph.client.miranda.keyring

                 ◎ username:  client.miranda

            ‼️ cd /etc/ceph/  firts.   user name must be like  client.xxxx /  only change miranda above. leave else ‼️ 
            ‼️ cd /etc/ceph/  firts.   user name must be like  client.xxxx /  only change miranda above. leave else ‼️ 
    

    🔶 cat user token (option)

        ceph auth get-key client.miranda




🔵 Client ✶ Mount (Ceph-fuse)

        ‼️ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! ‼️ 
        ‼️ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! ‼️ 
        ‼️ ceph-fuse is like cephadm. no ububtu22 now. use ubuntu 20! ‼️ 


    🔶 Insatll ceph-fuse 

        ◎ Ubuntu 20:   sudo apt install ceph-fuse  

    🔶 Make dir 

        sudo mkdir -p /etc/ceph


    🔶 Copy Moniter ceph.conf  to Client 

        sudo scp {user}@{server-machine}:/etc/ceph/ceph.conf /etc/ceph/ceph.conf
        sudo scp root@10.12.12.70:/etc/ceph/ceph.conf /etc/ceph/ceph.conf

            monitor node need enable root user & allow root ssh 


    🔶 Copy Moniter xxxx.keyring  to Client

        sudo scp {user}@{server-machine}:/etc/ceph/ceph.keyring /etc/ceph/ceph.keyring
        sudo scp root@10.12.12.70:/etc/ceph/ceph.client.miranda.keyring /etc/ceph/fs-miranda.keyring
            
            this download keyring  and renamed to fs-miranda.keyring 


    🔶 Make mount Folder 

        sudo mkdir /mnt/ceph-cephfs-MountPoint


    🔶 Mount cephfs 

        ceph-fuse [-n client.username] [ -m monaddr:port ] mountpoint [ fuse options ]
        sudo ceph-fuse -n client.miranda -m 10.12.12.70:6789 /mnt/ceph-cephfs-MountPoint


    🔶 check nfs 

        mount -l | grep ceph



🔵 Client ✶ Auto Mount - ceph fs  💯

    🔶 Create Script 

        sudo bash -c 'echo "
        #!/bin/bash
        ceph-fuse -n client.miranda -m 10.12.12.70:6789 /mnt/ceph-cephfs-MountPoint
        " > /root/crontab-script-ceph-fs-Mount-NoDEL.sh'


    🔶 Give Perssmion

            chmod +x /root/crontab-script-ceph-fs-Mount-NoDEL.sh



    🔶 crontab desc 

        ⦿  crontab -l   check  task
        ⦿  crontab -e   edit/add task
        ⦿  crontab -r   remove  task 


    🔶 enter crontab edit 

        crontab -e


    🔶 add a line & save file 

        @reboot /root/crontab-script-ceph-fs-Mount-NoDEL.sh


    🔶 reboot tast 


 
 









===============================================
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵Block Device Demo
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵Block Device Demo
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵Block Device Demo

https://documentation.suse.com/zh-cn/ses/7/html/ses-all/cha-ceph-as-iscsi.html
??



🔵 Server ✶ Create Block Device Pool
 



    🔶 create pool 

        ceph osd pool create CEPH_BD-K8s-Prod 128 128 
        ceph osd pool create CEPH_BD-K8s-Test 128 128 



    🔶 Change repeat size to 2

        ceph osd pool set CEPH_BD-K8s-Prod size 2
        ceph osd pool set CEPH_BD-K8s-Test size 2



    🔶 Init Pool 

        sudo rbd pool init CEPH_BD-K8s-Prod
        sudo rbd pool init CEPH_BD-K8s-Test 

            Filesystem and Object Gateway  No need Initialize 
            only Block Device need use rbd to initialize  


    🔶 list pool 

        ceph osd lspools


======================================

🔵 Server ✶ Create User for Mount 

    ‼️ By default Block Device Storage Use admin account to mount, No Need Create User. ‼️
    ‼️ By default Block Device Storage Use admin account to mount, No Need Create User. ‼️
    ‼️ By default Block Device Storage Use admin account to mount, No Need Create User. ‼️


    🔶 options 

        ceph auth get-or-create client.BD-User01 mon 'profile rbd' osd 'profile {profile name} [pool={pool-name}][, profile ...]' mgr 'profile rbd [pool={pool-name}]'

        ◎ read only user 
        ceph auth get-or-create client.BD-K8s-Prod-User01 mon 'profile rbd' osd 'profile rbd pool=CEPH_BD-K8s-Prod, profile rbd-read-only pool=images' mgr 'profile rbd pool=images'

        [client.BD-K8s-Prod-User01]
            key = AQCWq6FiJSG7Ah

        ceph auth get-or-create client.BD-K8s-Test-User01 mon 'profile rbd' osd 'profile rbd pool=CEPH_BD-K8s-Test, profile rbd-read-only pool=images' mgr 'profile rbd pool=images'

        [client.BD-K8s-Test-User01]
            key = AQCpq6Fiw/sJNBAAQx



========================================

🔵 Server ✶ Create Image ✅

    🔶 Image Desc 

        Pool is for ceph cluster;   Image is for Client to Mount;
            Image is like Hardware Disk let client mount & use.
                Create Image for pool_xxx  first.
                    Create User for Mount.
                        Client Mount.

        
        image is created under/inside pool. 


    🔶 Create Image 

        rbd create --size {megabytes} {pool-name}/{image-name}

        rbd create --size 10240 CEPH_BD-K8s-Prod/IMG-K8s-Prod
        rbd create --size 10240 CEPH_BD-K8s-Test/IMG-K8s-Test

            image is under pool.
            create 10G image under pool 


    🔶 List Image 

        rbd ls {poolname}
        rbd ls CEPH_BD-K8s-Prod
        rbd ls CEPH_BD-K8s-Test


    🔶 Check Image info 

        rbd info {pool-name}/{image-name}
        rbd info CEPH_BD-K8s-Prod/IMG-K8s-Prod
        rbd info CEPH_BD-K8s-Test/IMG-K8s-Test



    🔶 Resize Image  

        rbd resize --size 20480 IMG-K8s-Prod                     ➜ increase size 
        rbd resize --size 1024 IMG-K8s-Test --allow-shrink       ➜ decrease size  

            image only take space when use it. 
                if only create, no put data in. the real size is zero.

========================================

🔵 Client ✶ Mapping Image  ✅

    🔶 install Ceph Common

        U20-TempleOnly ~ sudo apt install ceph-common


    🔶 Copying Conf and Keyring from moniter node

        sudo scp root@10.12.12.70:/etc/ceph/ceph.conf /etc/ceph/ceph.conf
        sudo scp root@10.12.12.70:/etc/ceph/ceph.client.admin.keyring /etc/ceph/ceph.client.admin.keyring

            No Change filename on client side after copy
            when mapping. it use default name to find server info.
            or next mapping will wrong 


    🔶 mapping

        mapping is like put new disk to client server.
        if need use disk. still need format & mount.

        ◎ Check status    rbd showmapped 
        ◎ map             rbd map Pool_BD-K8s-Prod/IMG-K8s-Prod -p rbd





=====================

🔵 Client ✶ Mount disk    ✅

    atter Mapping successful.
        it is like a new hardware disk.
            format it to xfs &&  mount it; 
                then we can use it.


    🔶 check disk name 

        fdisk -l  
        Disk /dev/rbd0: 10 GiB, 10737418240 bytes, 20971520 sectors


    🔶 Format To XFS 

        mkfs.xfs /dev/rbd0 



    🔶 Make Mount Point 

        mkdir /mnt/ceph-img-k8s-prod


    🔶 Mount 

        mount /dev/rbd0 /mnt/ceph-img-k8s-prod/


    🔶 Verify 

        df -T 
        df -hl | grep rbd



🔵 Client ✶ Auto Mount 💯

    🔶 Create Script 

        sudo bash -c 'echo "
        #!/bin/bash
        rbd map Pool_BD-K8s_Prod/IMG-K8s-Prod -p rbd
        mount /dev/rbd0 /mnt/ceph-img-k8s-prod/
        " > /root/crontab-script-ceph-DB-Mount-NoDEL.sh'


        🔶 Give Perssmion ‼️

            chmod +x /root/crontab-script-ceph-DB-Mount-NoDEL.sh



    🔶 crontab desc 

        ⦿  crontab -l   check  task
        ⦿  crontab -e   edit/add task
        ⦿  crontab -r   remove  task 


    🔶 enter crontab edit 

        crontab -e


    🔶 add a line & save file 

        @reboot /root/crontab-script-ceph-DB-Mount-NoDEL.sh


    🔶 reboot tast 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  OB Demo  
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 
❌❌❌ Give Up.  can not enable fuck serverice..



‼️ Offical manual ..

https://docs.ceph.com/en/pacific/radosgw/pools/


radosgw  commond 
https://www.mankier.com/8/radosgw-admin


🔵 AWS S3 Desc  

    Amazon Simple Storage Service

    offer a web.  in web you can control all.
    every file have a url. 
    you can use it in your web. 
    good for website!!! 



🔵 Basic info 

    rgw-us-east:
    realm: replicated
    zonegroup: us
    zone: us-east
    rgw-us-west:
    realm: replicated
    zonegroup: us
    zone: us-west



    zone + zone ... >> Zone Group 
        zone group + zone group + ... >> realm 

    all zone under the same zone group  can sync between zone.
        this make backup between two physical place possible.
            like Zone-Group-Global
                zone_1 put in china 
                zone_2 put in Us
                ...



    ‼️ no commit no take effeci:             radosgw-admin period update --commit
    ‼️ no commit no take effeci:             radosgw-admin period update --commit
    ‼️ no commit no take effeci:             radosgw-admin period update --commit



 
 
🔶 Add placement target....
 
    $ radosgw-admin zonegroup placement add \
        --rgw-zonegroup OB-Group-Ark \
        --placement-id temporary


    $ radosgw-admin zone placement add \
        --rgw-zone OB-Zone-Ark \
        --placement-id temporary \
        --data-pool default.rgw.temporary.data \
        --index-pool default.rgw.temporary.index \
        --data-extra-pool default.rgw.temporary.non-ec



🔵 

🔶 Create realm:   

    radosgw-admin realm create --rgw-realm=OB-Realm-Ark --default 


🔶 Create zone group:  

    radosgw-admin zonegroup create --rgw-zonegroup=OB-Group-Ark --master --default


🔶 Create zone:  

    radosgw-admin zone create --rgw-zonegroup=OB-Group-Ark --rgw-zone=OB-Zone-Ark --master --default


🔶 update period

    radosgw-admin period update --rgw-realm=OB-Realm-Ark --commit 



🔶 Create RGW  

    orch apply rgw <realm_name> <zone_name> 
        ceph orch apply rgw OB-Realm-Ark OB-Zone-Ark --placement="1 CEPH-Node"


🔶 Check rgw 

    ceph orch ps --daemon-type rgw
    No daemons reported

    🐞 web:   No RGW service is running.

    



🔶 Create USER  ❌

    radosgw-admin user create --uid=rgw-user --display-name=rgw-user --system


    "user_id": "rgw-user",
    "display_name": "rgw-user",

            "user": "rgw-user",
            "access_key": "XD0QFJQCEJMSLEYQACGS",
            "secret_key": "WboesDxdzMezAMwQd85xpL26BAckiJfCvlZpt0Ci"
   


    记录access_key和secret_key的值保存为 access_key.txt 和secret_key.txt ，通过命令集成到dahsboard中。

    ceph dashboard set-rgw-api-access-key -i accesskey.txt 
    ceph dashboard set-rgw-api-secret-key -i secretkey.txt 



 
  


🔵 OB  client ..

    # 安装 AWS s3 API
    [root@ceph01 ~]# yum -y install s3cmd

    # 创建用户
    [root@ceph01 ~]# radosgw-admin user create --uid=s3 --display-name="objcet storage" --system

    # 获取用户 access_key 和 secret_key
    [root@ceph01 ~]# radosgw-admin user info --uid=s3 | grep -E "access_key|secret_key"
                "access_key": "RPRUFOWDK0x",
                "secret_key": "32efWJ7OKx"

    # 生成 S3 客户端配置(设置一下参数, 其余默认即可)
    [root@ceph01 ~]# s3cmd --configure
    Access Key: RPRUFOx
    Secret Key: 32efWJ7OeKJx
    S3 Endpoint [s3.amazonaws.com]: ceph01
    DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]: %(bucket).ceph01
    Use HTTPS protocol [Yes]: no
    Test access with supplied credentials? [Y/n] y
    Save settings? [y/N] y
    Configuration saved to '/root/.s3cfg'

    # 创建桶
    [root@ceph01 ~]# s3cmd mb s3://bucket
    Bucket 's3://bucket/' created

    # 查看当前所有桶
    [root@ceph01 ~]# s3cmd ls
    2020-06-28 03:02  s3://bucket
    -----------------------------------
    ©著作权归作者所有：来自51CTO博客作者红尘世间的原创作品，请联系作者获取转载授权，否则将追究法律责任
    使用cephadm快速搭建ceph集群
    https://blog.51cto.com/hongchen99/2507660






 











🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ceph auth / user permit 

🔵 WHY CephX 

    both ceph(server) and k8s(client) have a same key.
    use this can avoid hacker atteck. 

    if any client need use ceph.
    ceph have to create use first.
    then ceph will create a username & a key.

      [client.k8s]
          key = AQBTvaZi0HhkABAAgn 


🔵 CephX Auth 

    CephX manager whole ceph cluster login system.

    inside ceph:  
        not only user(clietn.admin .. ) need password. 
        mon/osd/mds need password to login ceph too.


    outside ceph: (like k8s cluster) 
        if k8s want connect/use ceph cluster must need four info.
            ◎ ceph cluster id:   fsid 
            ◎ ceph cluster monitor ip address. 
            ◎ username 
            ◎ password 


    we use mon node to build ceph cluster.
    so even you lost client.admin password. 
    mon node can reset password for admin.



🔵 permit analyze  

    CEPH-MGR.Root ~ ceph auth ls

    client.admin
        key: AQC08qBirX7TLhAAn
        caps: [mds] allow *
        caps: [mgr] allow *
        caps: [mon] allow *
        caps: [osd] allow *

    CEPH-MGR.Root ~ ceph auth ls
    client.k8s
        key: AQBTvaZi0HhkABAAgYs 
        caps: [mgr] profile rbd pool=Pool_BD-K8s_Test
        caps: [mon] profile rbd
        caps: [osd] profile rbd pool=Pool_BD-K8s_Test

    client.miranda
        key: AQC8nKFiQgA+A
        caps: [mds] allow r, allow rw path=/
        caps: [mon] allow r
        caps: [osd] allow rw pool=data


 
    🔶 permit 

        *  = rwx
        r:   allow read 
        w:   allow write 
        x:   allow run auth commonds (ceph auth list / ceph guth get)


    🔶 [mon] / [mgr] / [mds]  / [osd]

        ◎ caps:  capabilities 

        ceph cluster have a lot role.
        differnet role have differnet job/function.
        if you need give someone ceph`s permission.
        need add role permit first.  then set detail permit under that role. 

            caps: [osd] allow *               ➜ have all control to osd role.
            caps: [osd] allow rw pool=data    ➜ only read+wirte under data pool. 
            

    🔶 profile rbd pool=Pool_BD-K8s_Test




🔵 Set Permit Demo 

https://www.shuzhiduo.com/A/mo5k4alQdw/



🔵 Create/get User 

    ceph auth get-or-create client.k8s mon 'allow r' osd 'allow rw pool=Pool_BD-K8s_Test'


🔵 Change User Permit ✅

    ceph auth get client.k8s

    ceph auth caps client.k8s mon 'allow r' osd 'allow rw pool=Pool_BD-K8s_Test'    ➜ rw  one pool
    ceph auth caps client.k8s mon 'allow r' osd 'allow *'                           ➜ rw  all pool 




