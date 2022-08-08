---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️3️⃣ STO ➜ Basic
---


# Storage ✶ PV PVC CSI




 
🔵 StorageClass / Driver / Plugin

  🔶 WHY 

    PV & PVC make k8s storage config much easy.
        but config pv & pvc is still not that easy. 
          it is two people`s job! 
            if you want config pvc. must let it.admin config pv first.

    we have somthing better: Provisioner/StorageClass
        Provisioner/StorageClass can auotmatic create pv when docker user create pvc.
    
    Provisioner/StorageClass is just like driver for storage cluster.
        different storage cluster need different dirver 



  🔶 StorageClass / Driver _ CEPH-CSI

      StorageClass is not buildin.
      we need config StorageClass First! 
        we use ceph-rbd 
        so we need install ceph-csi plugin first.

        with this plugin. k8s can control ceph cluster.
        so k8s can create/delete ceph-rbd disk on ceph.



🔵 CEPH-CSI Desc

    CSI:   Container Storage Interface
      a dirver/plugin for container;   allow k8s control ceph cluster. 
        like NIC: make one pc connect other pc.
        here CSI: make K8s Storage Connect Ceph Storage. 








🔵 How 

  i have a detail demo under sto.rbd.ceph-csi install 
  this is simple demo 


  install cephadm to all k8s node 
    so k8s can support ceph storage. 


  install ceph-csi driver to all k8s node
    so k8s can support ceph much smart 
      like auto create pv for you .


  prepair pool & user in ceph cluster. 

  test pvc in k8s 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 CEPH-CSI prepair 


🔵 install cephadm tool  all node

    sudo apt install -y cephadm 

      test ceph port 
        nmap -p 6789 10.12.12.70



🔵 config ceph-csi plugin/driver in all node


    git clone --depth 1 --branch devel https://github.com/ceph/ceph-csi.git

    cd /root/ceph-csi/deploy/rbd/kubernetes


🔶 1. edit csi-config-map.yaml

  ‼️ only change id&ip ‼️
  ‼️ only change id&ip ‼️
  ‼️ only change id&ip ‼️


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



  kubectl apply -f csi-config-map.yaml
  kubectl get configmap



🔶 2.  csi-rbd-secret.yaml        ➜  set Ceph Username & password  ✅

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
      userKey: AQC08qBirxxxxx=     # ‼️ ChangeMe-02
    EOF



    kubectl apply -f csi-rbd-secret.yaml
    kubectl get secret



🔶 3. csi-kms-config-map.yaml    ➜  set kms / just copy&paste     ✅

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



    kubectl apply -f csi-kms-config-map.yaml








🔶 4. ceph-csi-config.yaml       ➜  unknow  / just copy&paste 

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







🔶 5. Edit provisioner.yaml      - Option 

    by default it create 3 same pod for backup!
    we k3s for test. only need 1. 


      🔻 Check  replicas 
        cat csi-rbdplugin-provisioner.yaml | grep replicas
          replicas: 3

      ◎ use sed change to 1 
        sed -i 's/replicas: 3/replicas: 1/g' csi-rbdplugin-provisioner.yaml

      ◎ check result 
        cat csi-rbdplugin-provisioner.yaml | grep replicas
          replicas: 1



🔶 apply other. 

    kubectl create -f csi-provisioner-rbac.yaml
    kubectl create -f csi-nodeplugin-rbac.yaml

    kubectl create -f csi-rbdplugin-provisioner.yaml
    kubectl create -f csi-rbdplugin.yaml




🔶 check status 
    now. ceph-csi is done. you can test.it.
      this need take some time to finisth.

    kubectl get pods
    NAME                                         READY   STATUS    RESTARTS   AGE
    csi-rbdplugin-xp84x                          3/3     Running   0          2m47s
    csi-rbdplugin-7bj26                          3/3     Running   0          2m47s
    csi-rbdplugin-provisioner-5d969665c5-2gvbq   7/7     Running   0          2m51s




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ceph  prepair 

🔵 ceph  prepair 

  🔶 Create Ceph K3s Pool:    CEPH_BD-K3s

      ceph osd lspools
      ceph osd pool create CEPH_BD-K3s 128 128 
      ceph osd pool set CEPH_BD-K3s size 1
      ceph osd pool set CEPH_BD-K3s target_size_bytes 1000G

      sudo rbd pool init CEPH_BD-K3s
      rbd ls CEPH_BD-K3s



  🔶 Create Ceph K3s User : k3s 

    ceph auth get-or-create client.k3s mon 'allow r' osd 'allow rw pool=CEPH_BD-K3s'

        [client.k3s]
                key = AQAJcctixrxxxx


  🔶 get admin key          ➜     ceph auth get client.admin

        key = AQC08qBixxxx09AX7TtstKNAA==


  🔶 get cluster id / fsid  ➜     ceph mon dump
        fsid b57b2062-e75d-11ec-8f3e-45be4942c0cb




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 k8s pvc demo 

🔵 Config StorageClass ✅




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


    cat <<EOF > sto-sc-cephcsi-rbd-k3s.yaml
    ---
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: sto-sc-cephcsi-rbd-k3s                          # ‼️ Change Me 03
    provisioner: rbd.csi.ceph.com
    parameters:
      clusterID: b57b2062-e75d-11ec-8f3e-45be4942c0cb      # ‼️  Change Me 01
      pool: CEPH_BD-K3s                               # ‼️  Change Me 02
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



    kubectl apply -f sto-sc-cephcsi-rbd-k3s.yaml





🔵 pvc create demo

    cat <<EOF > pvc-k3s-db-mysql.yaml
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: pvc-k3s-db-mysql                         # ‼️ Must         set pvc name
    spec:
      accessModes:
        - ReadWriteOnce                         # ‼️ ceph available Option: ReadWriteOnce/ReadOnlyMany
      volumeMode: Block                         # ‼️ NoChange:    this is block type. 
      resources:
        requests:
          storage: 500Gi                         # ‼️ Option       Change Size
      storageClassName: sto-sc-cephcsi-rbd-k3s   # ‼️ Must         Chaneg To your sc name 
    EOF




    kubectl apply -f pvc-k3s-db-mysql.yaml




🔵 PVC debug 

  🔶 check sc pv pvc status 

    kubectl get sc
    kubectl get pv
    kubectl get pvc    ➜  if ok. it should bond to sc!   


    K3s ~ kubectl get pvc
    NAME          STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS             AGE
    sto-pvc-k3s   Pending ❌                                     sto-sc-cephcsi-rbd-k3s   37m


  🔶 kubectl describe pvc





🔵 pvc use demo


    cat <<EOF > pod-netdebug.yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-netdebug                       # ‼️ set pod name 
    spec:
      containers:
        - name: nettools                        # ‼️ any name
          image: travelping/nettools            # ‼️ image url must right
          command:                              # ‼️ must run something. ro docker die.
            - sleep
            - "infinity"
          volumeDevices:
            - name: ceph-k3s-pod-nettools        # ‼️ any name... same with blow: volumes.name
              devicePath: /tmp                   # ‼️ container inside  path
      volumes:
        - name: ceph-k3s-pod-nettools            # ‼️ any name... same wuith up:  volumeDevices.name
          persistentVolumeClaim:
            claimName: sto-pvc-k3s               # ‼️ use your pvc name
    EOF


    kubectl apply -f pod-netdebug.yaml





🔵 check ceph. 

    CEPH-MGR.Root ~ rbd -p Pool_BD-K8s_Prod ls
    IMG-K8s-Prod
    IMG-K8s-Prod-NoCSI
    csi-vol-59b173b3-ee1b-11ec-8899-0e9db053906f


    ‼️ deleted pvc delete disk in ceph;   delete pod. disk still keep ‼️
    ‼️ deleted pvc delete disk in ceph;   delete pod. disk still keep ‼️
    ‼️ deleted pvc delete disk in ceph;   delete pod. disk still keep ‼️

    pvc is like disk.  you can mount all pod folder into one pvc. 
    but never delete pvc.

    but. how to create folder. in pvc? 
    how my k8s node can visit pvc folder. 





