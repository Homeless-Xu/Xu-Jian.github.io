---
sidebar_position: 1730
title: 🎪-7️⃣🌐🌐🌐 VXLAN ➜ NSXT - AIO
---

# vSphere ✶ NSXT




🔵 best website 

https://vdives.com/



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Basic info  

🔵 RequireMent       ✅

    🔶 Minumum Node 
        one physical esxi host is enough.  ram need 32G+ 
        a manager and a edge node T0.  you can run nsxt. 
            ▪ Physical:  Host    (esxi   )    x 1
            ▪ Virtual    Manager (in esxi)    x 1
            ▪ Virtual    T0      (in esxi)    x 1  


    🔶 Physical NIC

        if have vcenter/vds already. no addition physical nic needed.
        if no vcenter/vds   must have one empty nic. for nsx host node. 
            as for manager/t0/t1 there is nothing about physical nic.
                they all vm. under esxi. so only use portgroup in esxi.


    🔶 Physical Network Device  

        must support 1600 mtu setting.
        

    🔶 OVA Files ‼️ 

        ▪ nsxt-manager.ova   ➜ For Manager  
        ▪ nsxt-edge.ova      ➜ For Edge: T0 & T1.
            T0 and T1 both use: nsxt-edge.ova!   just have differet config. 

        if use vcenter.   only need manager.oav 
        if use esxi.      need both.  

            if have vceter, you can install edge via manager
            so you only need manager.ova 
            so you no need care their version. they must be same.

            if you use esxi.  you need downlaod both ova yourself. 
            do do make sure use same version.
                not use 3.1.2. manager . and use 3.0.0 edge.!!!  not work! 
                ‼️ Manager.ova and edge.ova   must same version. ‼️
                ‼️ Manager.ova and edge.ova   must same version. ‼️
                ‼️ Manager.ova and edge.ova   must same version. ‼️



    🔶 NSXT-Manager

        NSXT-Manager   one is must.  no need two for study only.
        only need manager during install and config.
        after total configed. you can even stop it. 


    🔶 T0/T1 node 

        T0 is must.  T1 is option.
        one T0 is ok. by default it need two.



    🔶 vCenter 

        NSX-T  can work with all other planform.  
        so vCenter is not a must needed.
        i don`t use it. take too much ram. 



 


🔵 License           ✅


    🔶 VMware NSX-T Data Center 3.1-only  ✅

        VM based keys.   3.1 ok.  3.2 No
        GC7N2-2NZDM-H88NY-GWQ7G-YF0E0
        AA1NK-2LG0L-48DQY-G5XNX-P6KVA
        CU3M2-8LZ47-485GZ-XQM7G-NY8G2

    offical have 60 day free try license.  if you use 3.2+ 






🔵 MTU Change ✅

    🔶 MTU Desc 

        MTU: ip package size in network.
        MTU Setting have two level setting:  Software & Hardware.
            by default. both software(win mac linux) and Hardware (switch router ) is 1500.
        
        software mtu like car running in road.
        hardware mtu like road width of the road.
            so hardware mtu can bigger than software mtu.
            but software mtu can`t bigger than hardware mtu.

        defautl mtu work well for most contidion.
        iscsi work much faster if have large mtu like 9000. but this is an option.
        as for nsxt. mtu 1600 is a must.

        a lot physical device involved with nsx.
            physical switch 
            physical router 
            physical vlan for nsxt in router.


 


    🔶 MikroTik Hardware Demo  

            ‼️ for simple  change whole hardware switch & router to mtu:1600 L2 MTU: 1700 ‼️
            ‼️ for simple  change whole hardware switch & router to mtu:1600 L2 MTU: 1700 ‼️
            ‼️ for simple  change whole hardware switch & router to mtu:1600 L2 MTU: 1700 ‼️

        usually all nic is under one bridge in ros.
        and all vlan is on that bridge.

        if we want change nsxt`s vlan`s mtu.  must change bridge`s mtu first.
        if want change birdge`s mtu.  must change all nic`s mtu under that bridge first.

        so usually set the whole switch & router to mtu:1600 or High...





🔵 Basic NSXT Knowledge 

    🔶 T0 T1 Desc        ✅

        like router(T0) and switch(T1) in physical network .
            T0 - Router:    router connect network device. (T0-T1es)
            T1 - Switch:    switch connect client device   (T1-VMs)


 
🔵 Traffic Type / Physical VLAN 

    Physical network:        use Normal vlan.
    Virtual  network(nsxt):  use overlay-based  vlan. 

    no matter how advanced nsxt`s overlay-based vlan is.
    it must need work at physical device like physical switch, physical nic. 

    normal vlan`s number is limited.   (0-4069)
    overlay-based vlan`s number is unlimited!  
    we must splite the two normal vlan and overlay vlan away.
    otherwise. you did something wrong in nsxt. it will take down the physical network too.

    so we create a physical vlan for nsxt only. 
    all overlay-based vlan. work under that physical vlan. 


    🔶 NSXT Overlay Physical VLAN 

        but in reality we need give two physical valn for nsxt`s overlay traffic.
        nsxt is still imporve it.  maybe they will fix it someday.
            Two Overlay Physical VLAN ‼️
                Physical Host node:  Need Physical vlan:  vlan-nsx_1044-overlay-host 
                Virtual  Edge node:  Need Physical vlan:  vlan-nsx_1040-overlay-edge 

        

    🔶 NSXT BGP Physical VLAN 

        the two overlay physical vlan. let all nsxt device connect each other.
        but still need a physical vlan to connect nsxt`s virtual world to physical world.
            how can nsxt visit internet.  
            how can nsxt`s overlay-vlan visit physica vlan.
            how can physical vlan   visit nsxt `s overlay valn

        all this need we connect physical router and our nsxt virtual router (Edge T0)
        so we need a physical vlan between physical router and T0 router.
        it is for virtual router T0`s wan connection 

        usually company have two physical router. in case one router dies.
        so we give two physical vlan for T0`s WAN connection too. 
            one vlan connect to physical router_A:  vlan-nsx_1041-T0-WAN-To-Router_A
            one vlan connect to physical router_B:  vlan-nsx_1042-T0-WAN-To-Router_B

        after T0`s wan is successful connected to physical router.
        you can setup bgp. to connect nsxt & physical network.
        after bpg setted. 
            any connect between nsxt network and physical network use BGP physical VLAN. 


    🔶 physical vlan summary 

        vlan-nsx_1044-overlay-host 
        vlan-nsx_1040-overlay-edge 

        vlan-nsx_1041-T0-WAN-To-Router_A
        vlan-nsx_1042-T0-WAN-To-Router_B

        two physical vlan for overlay is must. 
        one Physical vlan for T0`s WAN connection.  two is recommand.




 
🔵 NSXT Node 

    NSXT have two kinds of node.
        ▪ Host node: is physical manchine
        ▪ Edge node: is virtual machine 

    Host node offer physic nic to let nsxt use. / let physical computer support nsxt traffic. 

    Edge node is like Network device in physical network.
        nsxt as a virtual network system. of course need virtual network device
            ▪ Edge Node -T0 like physical router 
            ▪ Edge Node -T1 like physical switch 




🔵 Edge Node NIC  ‼️ 


    all edge is vm machine under esxi.
    so all nic in edge use esxi network`s portgroup.
    by default all edge have 4 nic.

    first one is for edge`s manager networl
    the rest 3 are for nsxt traffic.
    T0 and T1 is have different use of this 3 nic.
    because T0 need connect to physical router. T1 Not.


    🔶 T0 Nic Demo 

        nic0       ➜  esxi manager    ➜ PG-esxi-manager 
        fp-eth0    ➜  nsxt overlay    ➜ PG-TypeC-NSX_1040-TEP-Edge   
        fp-eth1    ➜  nsxt vlan-01    ➜ PG-TypeC-NSX_1041-WAN_RouterA 
        fp-eth2    ➜  nsxt vlan-02    ➜ PG-TypeC-NSX_1042-WAN_RouterB 


    🔶 T1 Nic Demo 

        nic0       ➜  esxi manager    ➜ PG-esxi-manager 
        fp-eth0    ➜  nsxt overlay    ➜ PG-TypeC-NSX_1040-TEP-Edge   
        fp-eth1    ➜  nsxt vlan-01    ➜ PG-TypeC-NSX_1040-TEP-Edge
        fp-eth2    ➜  nsxt vlan-02    ➜ PG-TypeC-NSX_1040-TEP-Edge

 


🔵 Segment  ✅

    two kinds:   
        overlay-based (under overlay zone )
        vlan-based    (under vlan zone)


    🔶 T0 Segment ✅

        ▪ VLAN-T0-WAN-RouterA
            zone-T0-Wan-RouterA 
            vlan: 0     ➜  we already set vlan in esxi portgroup.

        ▪ VLAN-T0-WAN-RouterB
            zone-T0-Wan-RouterB   
            vlan: 0     ➜  we already set vlan in esxi portgroup.





🔵 TEP: Desc         ✅

    ip is for physical network.
    in nsxt`s virtual network. they use tep instead of ip.  they are same thing. 
    all overlay-based traffice supported device in nsxt need tep.
    so physical host node.  and virtual edge vm. all need tep ip.

    tep`s package size is bigger than ip. 
    so it can not work on normal ip.
    so they create tep.



🔵 TEP: Host & Edge  💯

    ‼️ for simple edge tep and host tep use two physical vlan ‼️
    ‼️ for simple edge tep and host tep use two physical vlan ‼️
    ‼️ for simple edge tep and host tep use two physical vlan ‼️

 
        🔶 offical reason 

            https://kb.vmware.com/s/article/83743

            Edge TEP and ESXi host TEP can be configured on the same VLAN in the following configurations:

            - Edge VM TEP interface connected to a portgroup on an ESXi host not prepared for NSX
            - Edge VM TEP interface connected to a portgroup on a switch not used by NSX, on an ESXi host prepared for NSX
            - Edge VM TEP interface connected to a logical switch on a vDS7 with NSX-T 3.1.0 or above
            - Edge VM TEP interface connected to a logical switch on a NVDS with NSX-T 3.1.0 or above


            Edge TEP and ESXi host TEP must be on separate VLANs in the following configurations:

            - Edge VM TEP interface connected to a vDS portgroup where that vDS is used by NSX-T
            - Edge VM TEP interface connected to a logical switch on vDS/NVDS prior to NSX-T 3.1.0



            🔶 good file 02

                https://www.virten.net/2020/11/nsx-t-3-1-enhancement-shared-esxi-and-edge-transport-vlan-with-a-single-uplink/





🔵 time out set      ✅

    🔶 manager & edge 

        NSXT-G3> set cli-timeout 0
        NSXT-G3>
        NSXT-G3> get cli-timeout
        Wed Jun 01 2022 UTC 23:10:13.593
        Timeout disabled








🔵 CLI  Commands  

    🔵 all nsxt cli 

        https://vdc-download.vmware.com/vmwb-repository/dcr-public/cc42e3c1-eb34-4567-a916-147e79798957/8264605c-a5e1-49a8-b603-cc78621eeeab/cli.html


    🔵 Default Password  💯

        admin / default 
        default password is : default    not password or passwd ...


    🔵 Edge IP & Hostname ✅

        ssh to vm use admin account.

        NSXTEdge> get interfaces
        NSXTEdge> set interface eth0 ip 10.219.219.103/24 gateway 10.219.219.11 plane mgmt

        NSXTEdge> get hostname
        NSXTEdge> set hostname NSXT-Edge-T0-G3.RV




🔵 Misc   ❌


    🔶 Sync time ✅

        ssh use root !!!  
        sudo timedatectl set-timezone America/Los_Angeles



    🔶 Node 

        nsxt manager:   Host node   ➜ physical host like esxi.
        nsxt manager:   edge node   ➜ VM; T0 T1 use same vm. just differet config.     
    
            host node must be physical machine.
            not trt install three vm esxi in a physical esxi. 
            a lot error will happen. just not waste time.



        ‼️ esxi 虚拟机 是安装不了的. 就是 一个esxi实体机里面 安装esxi虚拟机. 里面的那个不行的 ‼️ 
        ‼️ esxi 虚拟机 是安装不了的. 就是 一个esxi实体机里面 安装esxi虚拟机. 里面的那个不行的 ‼️ 


        nsxt 支持跨平台. 不仅仅支持esxi. 还有 linux / kmv / 等等
        要让 物理机器 支持 nsxt. 那么肯定是内核层面的了.
        不同的系统 安装对应的nsxt 内核模块就行.

        kvm 的物理主机有kvm 的 nsxt 插件.
        debian/linux 的物理主机 有 debian/linux 的nsxt 插件.
        esxi 物理主机有 esxi 的 nsxt 插件.

        安装也分 手动和自动.
        esxi 主机的话.  nsx-t 上可以自动安装. 

        node >> host transport node >>
            managed by vCenter:   nsxt你加了vcenter . 所有esxi 都自己加在这下面. 不需再手动加host.
            managed by stand host:  没加vcenter. 手动一个个加物理机器的 选这里.

        一步步下去 就自动给你安装 esxi 需要的 nsxt 内核.   还创建了 nvds 分布式交换机.


        到这里 自动创建了 几个 vmm10 / 100  网卡.
        但是没有 ted ip.   dhcp 没设置过啊.


        
        🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
        现在 物理机器已经支持 nsxt 了.   然后也分了ip.
        相当于一个 虚拟发 分布式交换机已经建好.


        物理交换机 >> 物理实体机: 创建 虚拟分布式交换机 >>  
        分布式交换机创建好 你就可以当运营商了.  接下去 就是  主路由器t0  和 次路由器t1
        最好t1 下面接虚拟机.  网络就完成了.

        
        
        
        🔶 t1  也分两种.

            t1-DR  每台物理机 安装 nsx 后. 就支持nsxt 网络了.
            t1-SR:  但是要把多个物理主机  的网络连到一个 t1里面! ???




        🔵 网络路径 


        🔶 虚拟化 物理网卡 

            物理交换机  >>  esxi 主机物理网卡 ➜  配置nsx后 
                通过安装插件, 把物理网卡 虚拟成 nsxt 可用的网卡. 
                配置 uplink. 哪些网卡分给nsx 就在这里设.
                设好后就有了 tep ip.

            相当于家庭网络 连好了进来的宽带.
            下面配总路由.T0.


        🔶 T0 - SR & DR 

            T0 虽然是总路由.  应该一个就够了.
            但是虚拟化. 细分了总路由器的功能. 
            T0-SR 提供服务: dhcp / nat / nds ... ...
            T0-DR 提供 分布式连接: ( 连所有 T1. 次路由 )

            T0 是用 nsxt-edge 镜像虚拟出来的虚拟机.
            一个nsx网络 只要两个 T0 虚拟机:  一个 T0-SR  一个T0-DR 


        🔶 T1- SR & DR 

            T1  可以看成 分布式交换机.
            也细分成两个.

            T1-DR:  把 所有的 nsxt 网卡连起来.
            T1-SR:  让物理网卡 支持 nsxt 功能

            T1-SR 不是虚拟机. 不用部署. 就是一个插件 配nsx网络的时候会自动安装. 
            每个主机都要配t1-sr.  然后选择uplink . 也就是哪些网卡开 nsxt 功能.
            这里第一步就已经弄过了. 

            我们现在要搭建的是 T1-SR 
            也是通过 nsxt edge 镜像来搭建虚拟机. 然后配置成 T1-SR 角色. 

        
 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   Real Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   Real Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   Real Demo 


🔵 Physical VLAN Prepair 

    🔶 Hardware network device 

        Mikrotik: Router ➜  RB4011    10.219.219.11 
        Mikrotik: Switch ➜  CRS328    10.219.219.22 
            crs is L3 switch. is can be router too. here as a backup router. 
    

    🔶 Hardware VLAN 

        RB4011  VLAN-NSX_1044-Overlay-Host          10.44.44.11/24  ‼️ this need enable dhcp ‼️
        RB4011  VLAN-NSX_1040-Overlay-Edge          10.40.40.11/24
        RB4011  VLAN-NSX_1041-VLAN-T0-WAN_RB4011    10.41.41.11/24
        CRS328  VLAN-NSX_1042-VLAN-T0-WAN_CRS       10.42.42.22/24


🔵 Physical ESXI NIC & PortGroup 

    🔶 esxi 

        ESXI_G3             10.219.219.13
        ESXI_G5             10.219.219.15

    🔶 esxi_g3 physical nic       1x ETH  1x TypeC  1x usb_01  1x usb_02
    🔶 esxi_g5 physical nic               1x TypeC  1x usb_01  1x usb_02

        esxi not use any usb nic.   all usb nic is for nsxt only. 
        eth & typeC is for esxi. default switch0. 

    
    🔶 ESXI-G3 & G5 PortGroup 

        PG-ESX_1219-Manager                VLAN: 1219
        PG-NSX_1044-Overlay-Host           VLAN: 1044
        PG-NSX_1040-Overlay-Edge           VLAN: 1040
        PG-NSX_1041-VLAN-T0-WAN_RB4011     VLAN: 1041
        PG-NSX_1042-VLAN-T0-WAN_CRS328     VLAN: 1042


🔵 VM IP Prepair ✅

    ◎ Host             Node:   physical manchie.           ➜ ESXI_G3             10.219.219.13
    ◎ Host             Node:   physical manchie.           ➜ ESXI_G5             10.219.219.15

    ◎ NSXT Manager     Node:   Virtual Machine in ESXI_G3  ➜ VM_NSXT-Manager-G3  10.219.219.3

    ◎ NSXT Edge T0-G3  Node:   Virtual Machine in ESXI_G5  ➜ VM_NSXT-T0-G3       10.219.219.103  
    ◎ NSXT Edge T0-G5  Node:   Virtual Machine in ESXI_G3  ➜ VM_NSXT-T0-G5       10.219.219.105 
    ◎ NSXT Edge T1-G3  Node:   Virtual Machine in ESXI_G3  ➜ VM_NSXT-T1-G3       10.219.219.113 
    ◎ NSXT Edge T1-G5  Node:   Virtual Machine in ESXI_G5  ➜ VM_NSXT-T1-G5       10.219.219.115


🔵 Manager Node NIC Mapping 

    🔶 Manager nic  mapping 

        all nsxt host node and edge node connect manager use esxi manager vlan .
        only need one nic for manager. 


🔵 T0 Node Nic Mapping

    🔶 VM_NSXT-Edge-T0-G3    4 vNIC.  

        vm-nic01: eth0       PG-ESX_1219-Manager             
        vm-nci02: fp-eth0    PG-NSX_1040-Overlay-TEP-Edge         
        vm-nci03: fp-eth1    PG-NSX_1041-VLAN-T0-WAN_RB4011         
        vm-nci04: fp-eth2    PG-NSX_1042-VLAN-T0-WAN_CRS328       


    🔶 VM_NSXT-Edge-T0-G5    4 vNIC.  

        vm-nic01: eth0       PG-ESX_1219-Manager             
        vm-nci02: fp-eth0    PG-NSX_1040-Overlay-TEP-Edge         
        vm-nci03: fp-eth1    PG-NSX_1041-VLAN-T0-WAN_RB4011         
        vm-nci04: fp-eth2    PG-NSX_1042-VLAN-T0-WAN_CRS328      

  
🔵 T1 Node Nic Mapping 

    🔶 VM_NSXT-Edge-T1    4 vNIC.  
        vm-nic01: eth0       PG-ESX_1219-Manager             
        vm-nci02: fp-eth0    PG-NSX_1040-Overlay-TEP-Edge         
        vm-nci03: fp-eth1    PG-NSX_1040-Overlay-TEP-Edge     
        vm-nci04: fp-eth2    PG-NSX_1040-Overlay-TEP-Edge    

  




🔵 Add Host To nsxt

    🔶 Host Node Desc 

        nsxt manager must have control of physical nic on all host node. 
        add all esxi/kvm host to manager, so manager can control the physical nic on all host. 


    🔶 Add ALL host Node 

        https://10.219.219.53/nsx/#/app/system/home/nodes
            >> Host transport node 
                >> add esxi-g3 & esxi-g5 

 

🔵 Add Edge To nsxt  💯

    🔶 Edge Node

        ◎ login use admin                ➜   ssh admin@10.219.219.203
        ◎ Check edge ip                  ➜   NSXTEdge>get interface eth0
        ◎ check manager connect status.  ➜   NSXTEdge>get managers               
            No managers configured


    🔶 Manager Node  

        ◎ ssh login use admin            ➜   MAC ~ ssh admin@10.219.219.5
        ◎ get manager thumbprint         ➜   NSXT> get certificate api thumbprint

    🔶 edge join manager 💯

        join management-plane <NSX Manager FQDN> username admin thumbprint <thumbprint>
        join management-plane 10.219.219.3 username admin thumbprint 79ae01acae330fbf6fa409fa2b6159f7ed591a0beaa71394f7db9e9d170579f4








🔵 Transport Zone Prepair 

    ◎ Zone-Overlay-Host    type overlay   
    ◎ Zone-Overlay-Edge    type overlay 
    ◎ Zone-VLAN-T0-RB4011  type vlan     
    ◎ Zone-VLAN-T0-CRS328  type vlan     



🔵 Uplink Profile  

    ‼️ only profile for host need vlan.  all other use esxi portgroup(have vlan set alreay!! )  ‼️ 
    ‼️ only profile for host need vlan.  all other use esxi portgroup(have vlan set alreay!! )  ‼️ 
    ‼️ only profile for host need vlan.  all other use esxi portgroup(have vlan set alreay!! )  ‼️ 

    ▪ For Host     Profile-Host-Overlay_1044  ➜ 2 nic  loadbalance   & Transport VLAN: 1044 
    ▪ For T0       Profile-T0-Overlay         ➜ 1 nic  loadbalance   & Transport VLAN: 0
    ▪ For T0       Profile-T0-VLAN-RB4011     ➜ 1 nic  loadbalance   & Transport VLAN: 0
    ▪ For T0       Profile-T0-VLAN-CRS328     ➜ 1 nic  loadbalance   & Transport VLAN: 0
    ▪ For T1       Profile-T1-Overlay         ➜ 3 nic  loadbalance   & Transport VLAN: 0


🔵 Host Node Config ✅

    ESXI-G3 & ESXI-G5  

        Transport Zone:     Zone-overlay-Host 
        Uplink Profile:     Profile-Host-Overlay_1044
        IP:                 DHCP.       (from rb4011 vlan_1044)
        nic:                vusb0,vusb1 (get name form esxi >> netwrok >> physical nic >> )


🔵 Edge Node Ip Pool Prepair 

    host node can use dhcp from physical router.
    edge node can`t.   so create a ip pool in nsxt.(something like dhcp too, it is nsxt network`s dhcp. )

    manaegr >> networking >> ip address pools >> add
        Pool-Overlay-Edge 
            subnets: ip range 
                CIDR:  10.40.40.0/24                     ➜  use physical overlay-edge`s vlan info.
                IP Ranges:  10.40.40.100-10.40.40.200    ➜  ... 
                Gateway ip:  10.40.40.11                 ➜ ‼️ 

    🔶 Gateway desc 

        we have two overlay vlan .
        we sepreate it because vmware still not support one vlan/ or recommand use two vlan. 
        but the two overlay vlan still need connected!
        so both your overlay-edge and overlay-host  need set a gateway!!
        in gateway. allow them visit each other.
            if gateway is firewall.  by default. visit between two vlan is denyed.
            if gateway is Router.  by default. visit between two vlan is allowed.






🔵 T0-G3 T0-G5 Node Config 

    ‼️ have a default switch.  we use it for overlay  traffic;
      still need create two switch. for T0`WAN. need connect to two physical router.

    ‼️ only overlay zone need tep ip.    vlan zone no need ip! ‼️ 


    nsxHostSwitch (default switch)
            Transport Zone:     Zone-Overlay-Edge 
            Uplink Profile:     Profile-T0-Overlay
            IP:                 IP Pool >> Pool-Overlay-Edge
            nic:                fp-eth0 

    Switch-To-RB4011 (Create )
            Transport Zone:     Zone-T0-RB4011
            Uplink Profile:     Profile-T0-VLAN-RB4011
            nic:                fp-eth1

    Switch-To-CRS328 (Create )
            Transport Zone:     Zone-T0-CRS328
            Uplink Profile:     Profile-T0-VLAN-CRS328
            nic:                fp-eth2




🔵 T1-G3 T1-G5 Node Config 

    nsxHostSwitch (default switch)
            Transport Zone:     Zone-Overlay-Edge 
            Uplink Profile:     Profile-T1-Overlay, 
            IP:                 IP Pool >> Pool-Overlay-Edge
            nic:                fp-eth0, fp-eth1, fp-eth2







🔵 Host Node TEP IP Connection test ✅ ‼️‼️ 

    🔶 why 

        we have two physical vlan for overlay/tep 
        make sure host node can ping other host node 
        make sure edge node can ping other edge node 


    🔶 Host TEP ping Test 

        root ssh to esxi host. 
        get tep(vxlan) ip      >>   esxcfg-vmknic -l 
        tep(vxlan) ping test   >>   ping ++netstack=vxlan 10.44.44.59 -s 1572 -d -I vmk10
        3 packets transmitted, 0 packets received, 100% packet loss ❌❌❌❌❌❌

        ‼️ ping ++netstack=vxlan 10.44.44.59 -s 1572 -d -I vmk10 ‼️
        ‼️ ping ++netstack=vxlan 10.44.44.59 -s 1572 -d -I vmk10 ‼️
        ‼️ ping ++netstack=vxlan 10.44.44.59 -s 1572 -d -I vmk10 ‼️
                rember the ping test size is 1572  for  1600 mtu.
                rember the ping test size is 8972  for  9000 mtu.
                    -s means payload size.
                    not the real package size. 
                    real package size = payload + 28 for icmp and ip. 


        🐞 traceroute  ✅

            [root@ESXI-HW-G3:~] traceroute ++netstack=vxlan 10.44.44.59
                traceroute to 10.44.44.59 (10.44.44.59), 64 hops max
                1   10.44.44.59  0.403ms  0.317ms  0.305ms


        🐞 Ping -s  ❌

            [root@ESXI-HW-G3:~] ping ++netstack=vxlan 10.44.44.59 -s 1500 -d -I vmk10
            PING 10.44.44.59 (10.44.44.59): 1500 data bytes
            1508 bytes from 10.44.44.59: icmp_seq=0 ttl=64 time=0.596 ms
            1508 bytes from 10.44.44.59: icmp_seq=1 ttl=64 time=0.590 ms

            [root@ESXI-HW-G3:~] ping ++netstack=vxlan 10.44.44.59 -s 1600 -d -I vmk10
            PING 10.44.44.59 (10.44.44.59): 1600 data bytes
            sendto() failed (Message too long)
            sendto() failed (Message too long)


            🐞 Ping -s -I desc 
                -s  ➜  set ping package size .
                -I  ➜  set use which nic to ping out 

                use ping --help check what -s means.
                    in simple set how big the package sent.
                        normal network`s mtu is 1500.  
                        means the biggest package size should under 1500. 
                        other wise the package will be droped by a lot network device.
                        like pc`s nic.  switch. router ...

                    nsxt`s overlay network must use mtu >= 1600. 
                    so we have to test nsxt`s network use a large mtu than normal.
                    and we need choose what nic to send traffic too.  use -d nicname.

                    we ping use mtu 1600 failed.  we found the problem.
                    the problem is mtu size. 
                    it is nothing about nsxt. 
                    it is about your physical network not support large mtu.
                    or our physical network support large mtu. just need some config.

                    for esxi usb nic. it will not work at mtu 9000.  
                    but mtu 4000 should be ok.  of course nsxt only need mtu 1600.
                    so try fix it. 

                    
        🐞 MTU - Debug_01:  Mikrotik CRS328 Physical Switch
        
            host node tep use vlan 1044.
            create a vlan in switch. and give that vlan a ip:  10.44.44.22 
            and ping all host node`s tep ip 

                [Miranda@MikroTik] > ping 10.44.44.66 size=1600
                SEQ HOST                                     SIZE TTL TIME       STATUS        
                    0 10.44.44.66                                                  timeout       
                    1 10.44.44.66                                                  timeout       
                    2 10.44.44.66                                                  timeout       
                    sent=3 received=0 packet-loss=100% 

                [Miranda@MikroTik] > ping 10.44.44.66 size=1500
                SEQ HOST                                     SIZE TTL TIME       STATUS        
                    0 10.44.44.66                              1500  64 557us     
                    1 10.44.44.66                              1500  64 541us     
                    2 10.44.44.66                              1500  64 498us     


            🔶 how fix 

                set vlan_1044 `s mtu size in switch. 
                vlan_1044 is created in bridge.  that bridge have all ports.
                if we change mtu in that bridge. all other device will efficted.

                make another bridge:  Bridge-NSX-MTU_1600 
                put all nsxt host node`s nic to that bridge.
                    we can not change vlan`s mtu size directry.
                        we need change all the nic`s mtu under Bridge-NSX-MTU_1600  to 1600 
                            then the vlan`s mtu size on Bridge-NSX-MTU_1600 will automatic chaneg to 1600.

            🔶 Mikrotik  MTU vs L2MTU vs Actual MTU

                there is two MTU:  MTU  and L2 MTU. 

                    L2 MTU is for vlan   package 
                    MTU    is for normal package   

                        default MTU    is 1500
                        default L2 MTU is 1592  
                            so normal vlan should work.
                            but nsxt`s overlay need 1600.

                                so we change 
                                    mtu    to 1600.
                                    l2 mtu to 1700.  it should work 

                    then the tep vlan under Bridge-NSX-MTU_1600
                        L2 mtu automatic changed to 1696
                        mtu still 1500.
                            manual change it to 1600.


            🔶 Router config 

                i have a router above switch. 
                i don`t want change whole network to large mtu. 
                a lot problem will happen.   so i only chaneg mtu for nsxt host node`s nic on switch.
                nsxt `s tep need dhcp. 
                    so move everything nsxt related from router to switch. 
                        like dhcp for tep.  all nsxt vlans. 
                    
            
            🔶 ping again. ✅ 

                [root@ESXI-HW-G3:~] ping ++netstack=vxlan 10.44.44.22 -s 1572 -d -I vmk10 -c 10000
                PING 10.44.44.22 (10.44.44.22): 1572 data bytes
                1580 bytes from 10.44.44.22: icmp_seq=0 ttl=64 time=0.480 ms

                --- 10.44.44.22 ping statistics ---
                1 packets transmitted, 1 packets received, 0% packet loss


            🔶 ping host node. 

                [root@ESXI-HW-G3:~] ping ++netstack=vxlan 10.44.44.197 -s 1572 -d -I vmk10 -c 10000
                done. 





  

🔵 Edge Node TEP IP Connection test
 
🔵 Cluster 

🔵 add t0 t1 gateway 




🔵 T0 Segment Prepair 

    segment-vlan-T0-RB4011  Zone-T0-RB4011 VLAN: 0
    segment-vlan-T0-CRS328  Zone-T0-CRS328 VLAN: 0
        we have set vlan in esxi portgroup already. so here 0



🔵 T0 WAN interface config 

    as a virtual gateway . it need wan ip to connect physical router.
    add wan interface and set T0`s wan`s ip first.
    
    nsxt >> T0 >> edit >> interfaces >>add 
    because we have two T0. VM. 
    every T0 have two interace (one for RB4011 one for CRS328)
    so we need add totally four interface here. 
    if you only one T0. and one nic for T0`s vlan traffic. only need add one.
    the most important is. connect physical router and this t0.
    if you can ping successful use vlan_1041 or vlan_1042 you can go next: bgp.

    this is T0`s wan ip. so we need give an ip address for every wan interface. 
        all type: extenal 

    T0-G3-RB4011:  type: extenal   10.41.41.33/24    segment-vlan-t0-rb4011    Node: T0-G3  
    T0-G3-CRS328:  type: extenal   10.42.42.33/24    segment-vlan-t0-crs328    Node: T0-G3
    T0-G5-RB4011:  type: extenal   10.41.41.55/24    segment-vlan-t0-rb4011    Node: T0-G5
    T0-G5-CRS328:  type: extenal   10.42.42.55/24    segment-vlan-t0-crs328    Node: T0-G5

    then ping form physical router. make sure works.



🔵 T0 BGP config 

    🔶 Set Loca AS 

        set T0     local AS:   65400
        set RB4011 local AS:   65411
        Set CRS328 local AS:   65422


    🔶 T0 BGP Config 

        we need connect two physical router. so we need add two bgp
        very simple only need. pgysical gateway`s ip and AS number.
        if you have two T0 VM. you need config Source address.  if no.  leave it default.

            IP:   10.41.41.11
            Remote AS:  65411
            Source address: 10.41.41.33  10.41.41.55 

            IP:   10.42.42.22
            Remote AS:  65422
            Source address: 10.42.42.33  10.42.42.55 



    🔶 RB4011(ros v7) BGP Config 

        winbox >> routing >> bgp >> connection >> add 
        BGP-RB4011-ToNodeT0G3
            AS:             65411               ➜ set local AS  
            Remote Address: 10.41.41.33/32      ➜ T0`s wan`s G3 node`s IP. 
            Remote AS:      65400               ➜ set remote AS
            Local Rols:     ebgp 

        BGP-RB4011-ToNodeT0G5
            AS:             65411               ➜ set local AS  
            Remote Address: 10.41.41.55/32      ➜ T0`s wan`s G5 node`s IP. 
            Remote AS:      65400               ➜ set remote AS
            Local Rols:     ebgp 



    🔶 CRS328 (ros v7) BGP Config 

        winbox >> routing >> bgp >> connection >> add 
        BGP-CRS328-ToNodeT0G3
            AS:             65422               ➜ set local AS  
            Remote Address: 10.42.42.33/32      ➜ T0`s wan`s G3 node`s IP. 
            Remote AS:      65400               ➜ set remote AS
            Local Rols:     ebgp 

        BGP-CRS328-ToNodeT0G5
            AS:             65422               ➜ set local AS  
            Remote Address: 10.42.42.55/32      ➜ T0`s wan`s G5 node`s IP. 
            Remote AS:      65400               ➜ set remote AS
            Local Rols:     ebgp 





🔵 BGP Route Export 

    after bgp connected.
    we need choose  what vlan(segment) in nsxt should giveout.
    usually segment is connect to T1. 
    so you need setup T1, allow T1 to share segment to T0. 
    then T0 can share segment to Physical router.


    create some segment. 
    create t1.
    connect segment to T1. 
    connect T1 to T0 


🔶 T1 BGP share Setting 

    T1>> edit >> Route advertisment >> 
        enable :  all connected segmanets & service ports 



🔶 T0 BGP share setting 

        T0 >> edit >> Route re-distitbution >> add 
            route re-distribution >> 

        enable connected interace & segments.
            on both T0 subnets and T1 subnets 
        
        then go rb4011  ip >> routing 
            should have some new . begian with Db. 
            then you can ping nsxt vlan. form physical router. 


🔵 configurations

    you did it.
        it took me weeks. -.-  
        