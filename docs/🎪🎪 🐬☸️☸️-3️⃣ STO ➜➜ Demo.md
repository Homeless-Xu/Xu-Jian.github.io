---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️3️⃣ STO ➜ Demo
---


# Storage ✶ Demo


🔵 Must Know 

  🔶 rbd vs nfs 

    most cluster storage use rbd / nfs
        - rbd is for one machine/pod 
        - nfs if for lot machine/pod 


  🔶 pvc VS volume-folder 

    we use rbd 
      ➜ means one pod need one pvc!
      ➜ need create a lot pvc! 

      ➜ means every folder you mount to docker/docker-compose
        you need create a pvc in k8s.

      pvc is like volume-folder.
          you delete pod. pvc still there.
          but you delete pvc. all data gone.


  🔶 PV / StorageCass

      pv just like a hardware disk.
      you just need decide which disk to use.
        there are lots kinds of disk -.- 

          - prod disk
          - test disk

          - fast disk 
          - slow disk 

          - backup-yes disk 
          - backup-no  disk  


  🔶 PV & PVC 

      PV:   like formated disk. 
      PVC:  like folder 

            pv & pvc is same to you .
            you delete pvc. it maybe auto delete pv for you.
                - depends is storage support this function 


  🔶 StorageCass & real storage cluster 




🔵 How PVC Work 

  you create pvc.
      🔥 k8s auto create pv. & bond to your pvc.


  you crteate pod & use    pvc 

  you delete  pod & delete pvc   
      you no need keep data, so you  delete your pvc.
      if you need keep data, never   delete your pvc. 
          🔥 k8s maybe auto delete pv for you !!!!!
                  - depends is storage support this function 



🔵 StorageClass config 

    StorageClass decide what kind of disk/pv  user can use. 

    StorageClass is bond to storage cluster pool. 
      diffeent pool have different config. 
        how config storageclass depends on your pool conig.

    you no need much kind of disk/storageclass.

        SC-Test.0 ➜ Test pool    ➜  Normal disk with  0 copy  backup 
        SC-Prod.2 ➜ Prod pool    ➜  Normal disk with  2 copy  backup
        SC-DB     ➜ DB pool      ➜  high performace disk 





🔵 PVC  vs  Volume_folder 

  in docker ➜  volume is a folder  on host.
  in k8s    ➜  pvc    is a rbd disk on pod (not on host)


      in docker you can creat folder/file for volume very easy.
          it is local
      
      in k8s. you can only visit pvc folder in pod!
          it is rbd disk.
            rbd means one disk only allow one pod to use.
              some pods can not start without some config file.
                  you have to mount pvc to other pod in order to put file in pvc.
                    -.-







🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 misc 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵



🔵 PV Access Modes ✅

  ◎ RWO  ReadWriteOnce 
  ◎ ROX  ReadOnlyMany 
  ◎ RWX  ReadWriteMany  

  Access Modes is how disk can mount to node.

  most common is RWO:  
    disk mount to one node only. that node can read and write data.
  
  many storage like ceph support ROX: 
    Disk can mount to many node, but only one node can write.

  less storage support RWX: 
    disk can mount to many node. all node can read and write! 

  not all storage support three above mode.
  ‼️ ceph-csi only support RWO ROX.  No RWX  ‼️
  ‼️ ceph-csi only support RWO ROX.  No RWX  ‼️
  ‼️ ceph-csi only support RWO ROX.  No RWX  ‼️


==
🔵 RBD-Pod Volume set

  pod 
    >> spec 
      >> container. 
        >> VolumeDevice      # ‼️  this means usd rbd type storage          
          - name: KeepWeSame # ‼️set volume to use. name is just blow 3 line. must keep same
          - devicepaths      #  Path into Docker

  pod 
    >> spec 
      >> volumes:
        - name: KeepWeSame






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 one pvc . mount many folder. ✅ 

  apiVersion: v1
  kind: Pod
  metadata:
    name: test
  spec:
    containers:
      - name: test
        image: nginx
        volumeMounts:
          # 网站数据挂载
          - name: config
            mountPath: /usr/share/nginx/html
            subPath: html
          # Nginx 配置挂载
          - name: config
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
    volumes:
      - name: config
        persistentVolumeClaim:
          claimName: test-nfs-claim





