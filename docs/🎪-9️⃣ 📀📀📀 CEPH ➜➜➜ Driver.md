---
sidebar_position: 1930
title: 🎪-9️⃣📀📀📀 CEPH ➜➜➜ Driver
---

# K8s ✶ Install Ceph-CSI Driver




⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️
https://daimajiaoliu.com/daima/8c4744d11e93007


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 CEPH-CSI Integrating K8s 

  🔵 WHY 
      after you have build your own ceph cluster.
      next is let app: docler/k8s/esxi/vm  use ceph disk.
      ceph-rbd is most useful.

      you have two option make app use ceph-rbd.
      1. manual mount ceph-rdb disk to app (like docker) 
          use just like local disk. 

      2. no manyal any ceph-rdb disk to app (like K8s)
          install a ceph-csi  driver to app.
              let app can control ceph cluster directly.


🔵 POD ✶ ConfigMap Desc   ✅

    configmap allow you sperate config file and image file.
    it make conatin much moveable.

    like your database`d password/token.
    never write it into container 
    so create a configmap. put password in configmap.
    
    



🔵 Ceph Why & How   ✅


    🔶 WHY  

        Container `s data  is tempelory.
        k8s can create as many contain as you like.
        but when delete contain. the data in contain is lost too.


        CEPH as storage . we need use it.
        not only simple mount ceph disk(rbd/image) to vm/esxi 
        we can go deeper. let k8s cluster use ceph 
            combine ceph-rbd & StorageClass 
                let ceph give storage to pod. 


    🔶 How

        there is lots ways to use ceph.
        for now maybe rbd-provisioner is best 


🔵 K8s Storage flow 

    Pod >> PVC  >> PV  >> Storage 

    Physical Storage: NFS/ISCSI/CEPH-RBD.....
        >> PV: (Provide Storage): 
            >> PVC (Use Storage): 
                >> Namaspace 
                    >> Pod (Volume)

        Docker User:      Create PVC 
        IT Admin:         Create PV.



🔵 Clean Old file - option 

    if you tried install ceph-csi before. 
    ‼️ if possibel  reset k8s cluster.  ‼️
    ‼️ if possibel  reset k8s cluster.  ‼️
    ‼️ if possibel  reset k8s cluster.  ‼️


  🔶 configmap

      kubectl get configmap
      kubectl delete configmap xxx

  🔶 services

      kubectl get services
      kubectl delete services xxxx

  🔶 deployments.apps

      kubectl get deployments.apps
      kubectl delete deployments.apps csi-rbdplugin-provisioner







🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ceph-csi 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 CEPH-CSI 

    https://docs.ceph.com/en/latest/rbd/rbd-kubernetes/?highlight=CSI

    https://github.com/ceph/ceph-csi


🔶 CEPH-CSI offical Config Manual 

    ‼️  https://docs.ceph.com/en/latest/rbd/rbd-kubernetes/?highlight=CSI
    ‼️  https://docs.ceph.com/en/latest/rbd/rbd-kubernetes/?highlight=CSI
    ‼️  https://docs.ceph.com/en/latest/rbd/rbd-kubernetes/?highlight=CSI

==
🔵 CEPH Prepair ➜ Pool & User ✅

🔶 Create Pool / image 

  CEPH-MGR.Root ~ ceph osd lspools
    Pool_BD-K8s_Test
  CEPH-MGR.Root ~ rbd ls Pool_BD-K8s_Test
    IMG-K8s-Test


🔶 Disable some img function 

    rbd feature disable POOL_NAME/IMAGE_NAME FEATURE_NAME

      rbd feature disable Pool_BD-K8s_Test/IMG-K8s-Test deep-flatten
      rbd feature disable Pool_BD-K8s_Test/IMG-K8s-Test fast-diff
      rbd feature disable Pool_BD-K8s_Test/IMG-K8s-Test object-map
      rbd feature disable Pool_BD-K8s_Test/IMG-K8s-Test exclusive-lock

 
🔶 Create User & Key:

    ceph auth get-or-create client.k8s mon 'allow r' osd 'allow rw pool=Pool_BD-K8s_Test'

      [client.k8s]
          key = AQBTvaZi0HhkABAAgYa1I5 
            ◎ ➜  ‼️name:  k8s 
            ◎ ➜  ‼️key:   AQBTvaZi0HhkABA 


🔶 Check User permit

    CEPH-MGR.Root ~ ceph auth get client.k8s
    exported keyring for client.k8s
    [client.k8s]
      key = AQBTvaZi0HhkABAAgYa1 
      caps mon = "allow r"
      caps osd = "allow rw pool=Pool_BD-K8s_Test"


🔶 User Option (use admin)

    CEPH-MGR.Root ~ ceph auth get client.admin
    exported keyring for client.admin
    [client.admin]
      key = AQC08qBirX7T 
    


🔶 Check cluster id & Monitor IP

    CEPH-MGR.Root ~ ceph mon dump
    fsid b57b2062-e75d-11ec-8f3e-45be4942c0cb       ➜ ‼️  need this 
    0: [v2:10.12.12.70:3300/0,v1:10.12.12.70:6789/0] mon.CEPH-Mgr  ‼️ ➜ need v1 ip & port 
        two version:  v1 v2.
        ceph-csi only support v1. for now. 



==
🔵 K8s  network Prepair ✅

    ALL K8s node.


  🔶 Install Package 

      sudo apt install -y cephadm
          cephadm package include ceph-common. 
            you can try only install ceph-common -.-
          use ubuntu_20 will save you lot time.


  🔶 Network Config  ✅

    ‼️ all k8s need connect to ceph manager 6789 port)‼️
      give same portgroup nic to k8s.
        
        ◎ Test 6789 port connection 
        nmap -p 6789 10.12.12.70



==
🔵 Workflow ‼️

  🔶 Tips 

      1. Not create new namespace for ceph-csi  
          use default: default 
          if changed namespace. need replace at a lot place in other yaml.

      2. No change any ceph-csi yaml filename we download next move.
          file name is configed in other yaml! 



  0. download github ceph-csi.
      ◎ include all yaml we need, just edit some yaml,  apply.  and done! 

  1. set Ceph Cluster info 
      ◎ k8s need konw ceph cluster ip to connect.

  2. set Ceph Username & password
      ◎ k8s need ceph`s permission (username & token) to control ceph-rbd disk.

  3. copy&paste  no change
  4. copy&paste  no change
  5. copy&paste  no change
  6. check result


🔵 0. Download github Ceph-CSI      ✅

  🔶 download 

    git clone --depth 1 --branch devel https://github.com/ceph/ceph-csi.git

  🔶 enter 

    K8s-Manager kubernetes pwd
    /root/ceph-csi/deploy/rbd/kubernetes
    K8s-Manager kubernetes ll
    total 40K
    -rw-r--r-- 1 root root  309 Jun 15 19:30 csi-config-map.yaml
    -rw-r--r-- 1 root root  435 Jun 15 19:30 csidriver.yaml
    -rw-r--r-- 1 root root 1.8K Jun 15 19:30 csi-nodeplugin-psp.yaml
    -rw-r--r-- 1 root root 1.1K Jun 15 19:30 csi-nodeplugin-rbac.yaml
    -rw-r--r-- 1 root root 1.2K Jun 15 19:30 csi-provisioner-psp.yaml
    -rw-r--r-- 1 root root 3.2K Jun 15 19:30 csi-provisioner-rbac.yaml
    -rw-r--r-- 1 root root 8.0K Jun 15 19:30 csi-rbdplugin-provisioner.yaml
    -rw-r--r-- 1 root root 7.1K Jun 15 19:30 csi-rbdplugin.yaml

    

🔵 1. csi-config-map.yaml        ➜  set Ceph Cluster info         ✅

  ‼️ enter /root/ceph-csi/deploy/rbd/kubernetes;  edit file only. no need create file! ‼️
  ‼️ enter /root/ceph-csi/deploy/rbd/kubernetes;  edit file only. no need create file! ‼️


    cat <<EOF > csi-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      config.json: |-
        [
          {
            "clusterID": "b57b2062-e75d-11ec-8f3e-45be4942c0cb",   # ‼️ ChanegMe-01 
            "monitors": [
              "10.12.12.70:6789"                                   # ‼️ ChangeMe-02
            ]
          }
        ]
    metadata:
      name: ceph-csi-config
    EOF


    🔶 apply yaml       ➜    kubectl apply -f csi-config-map.yaml
    🔶 check configmap  ➜    kubectl get configmap

    kubectl delete configmap ceph-csi-config




==
🔵 2. csi-rbd-secret.yaml        ➜  set Ceph Username & password  ✅

  ‼️ try use ceph admin if possible .  other user have unknow problem 
  ‼️ try use ceph admin if possible .  other user have unknow problem 
  ‼️ try use ceph admin if possible .  other user have unknow problem 


      cat <<EOF > csi-rbd-secret.yaml
      ---
      apiVersion: v1
      kind: Secret
      metadata:
        name: csi-rbd-secret
        namespace: default
      stringData:
        userID: admin                                         # ‼️ ChangeMe-01
        userKey: AQC08qBirX7TLhAAn    # ‼️ ChangeMe-02
      EOF



  🔶 applu 

      kubectl apply -f csi-rbd-secret.yaml


  🔶 check 

      kubectl get secret
      NAME             TYPE     DATA   AGE
      csi-rbd-secret   Opaque   2      37s


==
🔵 3. csi-kms-config-map.yaml    ➜  set kms / just copy&paste     ✅

    cat <<EOF > csi-kms-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      config.json: |-
        {}
    metadata:
      name: ceph-csi-encryption-kms-config
    EOF


  🔶 apply 

    kubectl apply -f csi-kms-config-map.yaml


🔵 4. ceph-csi-config.yaml       ➜  unknow  / just copy&paste 

    cat <<EOF > ceph-config-map.yaml
    ---
    apiVersion: v1
    kind: ConfigMap
    data:
      ceph.conf: |
        [global]
        auth_cluster_required = cephx
        auth_service_required = cephx
        auth_client_required = cephx
      # keyring is a required key and its value should be empty
      keyring: |
    metadata:
      name: ceph-config
    EOF


    kubectl apply -f ceph-config-map.yaml



🔵    Edit provisioner.yaml      - Option 

  🔶 WHY 

    K8s-Mgr.Root ~ kubectl get pods
    NAME                                         READY   STATUS    RESTARTS   AGE
    csi-rbdplugin-jh4j2                          3/3     Running   0          12h
    csi-rbdplugin-provisioner-86984584f5-f84bw   7/7     Running   0          12h
    csi-rbdplugin-provisioner-86984584f5-qllx4   0/7     Pending   0          12h  
    csi-rbdplugin-provisioner-86984584f5-skbx7   0/7     Pending   0          12h  

    by default it create 3 csi-rbdplugin-provisioner:  
      one run. two pending.
        i just keep one run.  no want two pending. 
          jsut need change replicas from 3 to 1 


        🔶 Check  replicas 

          cat csi-rbdplugin-provisioner.yaml | grep replicas
            replicas: 3

        ◎ use sed change to 1 
          sed -i 's/replicas: 3/replicas: 1/g' csi-rbdplugin-provisioner.yaml

        ◎ check result 
          cat csi-rbdplugin-provisioner.yaml | grep replicas
            replicas: 1

 


    🔶 debug 

      kubectl get namespace
      kubectl get pods --all-namespaces
      kubectl get events -n <namespace> 
      kubectl get events -n 
      



🔵 5. install driver             (all k8s node ? )

    kubectl create -f csi-provisioner-rbac.yaml
    kubectl create -f csi-nodeplugin-rbac.yaml

    kubectl create -f csi-rbdplugin-provisioner.yaml
    kubectl create -f csi-rbdplugin.yaml


🔵 6. Check Status . ✅

    kubectl get pods
      NAME                                         READY   STATUS    RESTARTS   AGE
      csi-rbdplugin-provisioner-86984584f5-2vd5r   7/7     Running   0          21s
      csi-rbdplugin-provisioner-86984584f5-b96hj   0/7     Pending   0          21s
      csi-rbdplugin-provisioner-86984584f5-k8jlx   0/7     Pending   0          21s
      csi-rbdplugin-r7t5b                          3/3     Running   0          16s


==
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Ceph-CSI PVC demo

🔵 Demo Desc 

    it.admin config storage class first.

    docker user create pvc. 
    storageclass auto create pv.

    save it.admin time


🔵 Config Storage Class 
      
    🔶 create yaml 

    cat <<EOF > csi-rbd-sc.yaml
    ---
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: csi-rbd-sc
    provisioner: rbd.csi.ceph.com
    parameters:
      clusterID: b57b2062-e75d-11ec-8f3e-45be4942c0cb      # ‼️  Change Me 01
      pool: Pool_BD-K8s_Test                               # ‼️  Change Me 02
      imageFeatures: layering
      csi.storage.k8s.io/provisioner-secret-name: csi-rbd-secret
      csi.storage.k8s.io/provisioner-secret-namespace: default
      csi.storage.k8s.io/controller-expand-secret-name: csi-rbd-secret
      csi.storage.k8s.io/controller-expand-secret-namespace: default
      csi.storage.k8s.io/node-stage-secret-name: csi-rbd-secret
      csi.storage.k8s.io/node-stage-secret-namespace: default
    reclaimPolicy: Delete
    allowVolumeExpansion: true
    mountOptions:
      - discard
    EOF


  🔶 apply yaml 

    kubectl apply -f csi-rbd-sc.yaml


  🔶 Check SC

    K8s-Mgr.Root kubernetes kubectl get storageclass
    NAME         PROVISIONER        RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
    csi-rbd-sc   rbd.csi.ceph.com   Delete          Immediate           true                   9h




🔵 Create block-based PVC

    i only use ceph-bd /rbd /image function.
    so i only need create block-based pvc.

    if you use ceph-fs you need create file-system-based pvc
      use offical document 
        https://docs.ceph.com/en/latest/rbd/rbd-kubernetes/?highlight=CSI


  🔶 create pvc yaml 

    cat <<EOF > raw-block-pvc.yaml
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: raw-block-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      volumeMode: Block             # ‼️ this is block type.  no need change
      resources:
        requests:
          storage: 1Gi              # ‼️ Only need change Size
      storageClassName: csi-rbd-sc
    EOF



  🔶 apply pvc

      kubectl apply -f raw-block-pvc.yaml


  🔶 check pvc status 

    kubectl get pvc
    NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    raw-block-pvc   Bound‼️   pvc-d55e2cee-c122-4364-a2d8-d164ecadd012   1Gi        RWO            csi-rbd-sc     9h



🔵 Bind PVC to pod   ➜ copy & paste

    cat <<EOF > raw-block-pod.yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-with-raw-block-volume
    spec:
      containers:
        - name: fc-container
          image: fedora:26
          command: ["/bin/sh", "-c"]
          args: ["tail -f /dev/null"]
          volumeDevices:
            - name: data
              devicePath: /dev/xvda
      volumes:
        - name: data
          persistentVolumeClaim:
            claimName: raw-block-pvc
    EOF


    kubectl apply -f raw-block-pod.yaml



  🔶 Check pod

    kubectl get pod
    pod-with-raw-block-volume                    0/1     ImagePullBackOff   0          10m



🔵 Ceph Check Image 

    now k8s can use pool in ceph.
    no need to create image manual! 

    🔶 Check Ceph Pool

      CEPH-MGR.Root ~ rbd -p Pool_BD-K8s_Test ls
      IMG-K8s-Test
      csi-vol-c8000098-edff-11ec-8899-0e9db053906f       ➜ this is k8s create for us.

