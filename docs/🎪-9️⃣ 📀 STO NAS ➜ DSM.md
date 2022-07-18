---
sidebar_position: 1910
title: 🎪-9️⃣📀 STO NAS ➜ DSM7
---


# NAS ✶ Local ➜ ESXI . Synology



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 HomeLAB ✶ NAS ▪ DSM7.1 ESXI 

🔵 Why 

    truenas difficult to set file permission.



🔵 How install DSM 

    1. download other`s custemed image.  (much easy ) ➜  i succeed 
    2. build your own image              (diffculty ) ➜  i failed

    so here is demo 
        install dsm7.1 to vm under esxi7
            i use esxi notnaml disk as data storage disk
            if your vm use whole physical disk (maybe works too)

    ‼️ if you want install dsm to physical machine this custom image no work ‼️
    ‼️ if you want install dsm to physical machine this custom image no work ‼️
    ‼️ if you want install dsm to physical machine this custom image no work ‼️


 

🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 DSM 7.1 ESXi Demo ✅ 

🔵 Must Know 

    ‼️ this for esxi only. no for physical machine. no waste your time. ‼️
    ‼️ this for esxi only. no for physical machine. no waste your time. ‼️

    ‼️ when begin install dsm. must cut wan internet but keep lan internet ‼️
    ‼️ when begin install dsm. must cut wan internet but keep lan internet ‼️
            you need lan  because you need login dsm web to install dsm system.
            you must cut internet. during install. after install you can connect back 


🔵 Workflow  

    download custom img:      ➜ get two vmdk. 
    upload two vmdk to esxi:  ➜ get one esxi disk (dsm boot disk)
    create new vm.            ➜ choose redhat ent.. 7+ 
    change bios mode to bios. (no efi)
    delete default iscsi dirver 
    add sata dirver control  
    add boot disk 
    add data disk
    set disk boot order (sata 0:0 / sata 0:1) 

    boot dsm 

    search ip use dysonolgy assistant tool / or check your router`s dhcp pool 
    login dsm install web.
    download xxx.pat from offical web. 
        pat should same version with dsm. 
        here we download DSM_DS3617xs_42661.pat

    ‼️ must no internet. if no you will enter ecovermode, if so re create vm ‼️
    ‼️ must no internet. if no you will enter ecovermode, if so re create vm ‼️

 k)



🔶 Download customed vmkd. 

    1. download zip 
    2. get two vmkd file. 
    3. upload both file to esxi 
        it auto become one esxi disk. 

🔵 Config VM 

    🔶 create vm 

        linux / redhat ent 7+
            we need sata control. 
            some linux version in esxi no sata control.
    

    🔶 Delete default vm config 

        delete default iscsi control - must
                    dsm no support iscsi dirver.

        Delte cd-rom 
            we no need it .

        delete default disk 
            we need delete it.  and add it back later. 
            it is about boot order.
            this is not must. if you know how to set boot order.


    🔶 Add sata control  

            boot disk have to use sata/ide control 
            data disk can sata mode. 


    🔶 Add disk 

        we need two disk:  
            boot disk:  the disk we download & upload to esxi. 
            Data disk:  we need install dsm system to disk! 
                data disk must 15G+ 



    🔶 config disk control 

        boot disk must use sata/ide control. 
            you can try sata first.  if sata no bootable. use ide.

        data disk.  usually sata works. 
            i just try add esxi disk create like normal vm.
            if you rdm esxi host sata control to vm. maybe much east.

        ‼️ if you can enter dsm web. but it say no disk found. 
            either the custom image no have driver you need. or you did something wrong.
            if you use esxi7 & add normal disk like create vm.
            you must should be good at this step. 


    🔶 set boot order: 

        Boot disk:  use sata: 0.0  means first boot disk 
        data disk:  use sata: 0.1  means second boot disk i. 


    🔶 nic 

        usually   vmxnet 3 works.
            if you boot into dsm. but no ip found.
                try change to other. 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 DSM 7.1 DIY Image ❌❌❌ 

❌❌❌  end up no disk found.  ❌❌❌


🔵 Tool Desc 

    dsm default img no support esxi.
        need add esxi nic driver.
        if you add/mount physical disk to vm. you need add disk dirver too.
            like nvme dirver.

    this tool help you install driver to dsm image.
    create a custom dsm img for you.
    then you can use thant img to create dsm vm.


🔵 Tool Workflow  

    download tool from github 

    add some driver (esxi nic / nvme driver)
    make a image
    make bootfile
    boot dsm 



🔵 Prepair 

    we need a vm to create image.
    need remote use screen.


🔶 JQ  tool - must 

    apt install jq -y



 


🔵 Downlaod tool 

    wget https://github.com/tossp/redpill-tool-chain/archive/refs/heads/master.zip
    
    unzip master.zip 
    
    mv redpill-tool-chain-master dsm7 & cd dsm7 



🔵 config default user_config ‼️ 

    🔶 1. rename  - must 

        mv sample_user_config.json ds918p_user_config.json
        mv sample_user_config.json ds3622xsp_user_config.json
            rename to the dsm version you want install.

    🔶 2. edit

        if you use usb boot mode. you need set vid pid 
            google: vid pid usb drive
        
        ‼️ if no usb node. (sata mode )   no edit. ‼️



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Driver  

🔵 Github DSM Driver Center 

    https://github.com/pocopico/rp-ext


🔵 esxi nic driver - Must ✅

    esxi vm have three kinds nic
        e1000、e1000e、vmxnet3
            we add all. 


    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/main/e1000/rpext-index.json

    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/main/e1000e/rpext-index.json

    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/main/vmxnet3/rpext-index.json


🔵 usb nic driver - option 

    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/ax88179_178a/rpext-index.json



🔵 Add other Driver 

    no too much driver. too big it will fail.



    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/vmw_pvscsi/rpext-index.json


    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/mvsas/rpext-index.json



    sata sas  

    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/aic94xx/rpext-index.json



🔵 nvme   sm981/pm981/pm983




🔵 common driver ?

    安装 thethorgroup.virtio : 
    ./redpill_tool_chain.sh add https://github.com/jumkey/redpill-load/raw/develop/redpill-virtio/rpext-index.json

    安装 thethorgroup.boot-wait : 
    ./redpill_tool_chain.sh add https://github.com/jumkey/redpill-load/raw/develop/redpill-boot-wait/rpext-index.json

    安装 pocopico.mpt3sas : 
    ./redpill_tool_chain.sh add https://raw.githubusercontent.com/pocopico/rp-ext/master/mpt3sas/rpext-index.json

    安装 jumkey.dtb : 
    ./redpill_tool_chain.sh add https://github.com/jumkey/redpill-load/raw/develop/redpill-dtb/rpext-index.json





🔵 Build image & bootfile

    🔶 Check usable version 

        ./redpill_tool_chain.sh 
            ds3622xsp-7.1.0-42661
            ds918p-7.1.0-42661


    🔶 Build image (xxx.img)

        ./redpill_tool_chain.sh build ds3622xsp-7.1.0-42661
        ./redpill_tool_chain.sh build ds918p-7.1.0-42661

            here tool help us put nic driver to image.

            🐞 if xxx libnetwork.endpointCnt: Key not found in store
                ‼️ sudo service docker restart & retry it ‼️ 
                ‼️ sudo service docker restart & retry it ‼️ 


    🔶 Build bootfile  (xxx.pat)

        ./redpill_tool_chain.sh auto ds3622xsp-7.1.0-42661
        ./redpill_tool_chain.sh auto ds918p-7.1.0-42661

            [#] Generating GRUB config... [OK]
            [#] Creating loader image at /opt/redpill-load/images/redpill-DS3622xs+_7.1.0-42661_b1655606748.img... [OK]
    

    🔶 Download bootable PAT image 

        Docker-Test.Root images pwd
        /root/dsm7/images
        Docker-Test.Root images ls
        redpill-DS3622xs+_7.1.0-42661_b1655606748.img



🔵 Generate SN/MAC ?

    ./redpill_tool_chain.sh sn ds918p
    ./redpill_tool_chain.sh sn dva3221




🔵 Change IMG to VMDK(esxi)

    🔶 Desc 

        this img can use on physical machine.
        if we need use in esxi. need change img to vmdk! 
        a lot way to change img. we choose qumu-img 


    ‼️ vmdk have two kinds. workstation and esxi.‼️
    we use  StarWind V2V Image Converter  change to esxi type .

    output two file.
    upload both two file to esxi. it become one xxx.vmdk!!!! 





🔵 esxi vm 

    Create new vm. 

    System: must        linux >> redhat >> ent... x 64 .. v7 
        or  maybe no sata control

    
    boot mode:   bios   no efi.

    Delete default disk & iscsi driver.
    add sata control. 
    
    change boot disk to sata control.

    add new disk (vm virtual disk )
        set to sata control 

    
    boot. 
        then use dysonolgy search tool 
        or go router check what ip new vm use.

    🔶 visit dsm web    

        10.1.1.50:5000 


    🐞 No Disk found 
    means no driver found. 
    need add scsi driver ??
 