---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️0️⃣ K8s ➜ Misc.9
---


# K8s Misc





🔵 POD Config

  🔶 spec
    spec-pod        (podname;                      volumes:disk mount to container; )
    spec-containers ( Container name;  image url;  export port; mount out path)



  🔶 metadata

      name        ➜ pod name
      namespace   ➜ set namespace.    ‼️ default namespace is: default ‼️
      labels      ➜ set label
      annotaton   ➜ set note 


  🔶 name space

    not use k8s default namespace if possible.
    namespace is how you category all your dockeres.

      NS-Prod
      NS-Test
      NS-AIO


  🔶 Labels

    label-version
    label-prod/test 
    label-web/sql/app
    label-tool





🔵 Ingress 

  🔶 Needs 

    Ingre allow outside user. visit service inside cluster. 
      NodePort need use host node port, the port on host is limited!
      so need something better: Ingress 


  🔶 Desc 

    service use tcp/udp.
    ingress use http/https 

    if need use ingress.  have to install ingress controller first.
    like nginx ingress controller.








🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  K8s  basic 


	https://www.qikqiak.com/k8s-book/
	https://www.qikqiak.com/k8s-book/
	https://www.qikqiak.com/k8s-book/

	

🔵 deployment / backup 

    you can deploy pod use kubectl create -f pod.yaml
    but you use k8s.  the pod must be important.
    you need at least run two pod at same time.
    in case one pod down. k8s will switch to use another one 
    just like back up. 


        apiVersion: apps/v1
        kind: Deployment
        spec:
        replicas: 2


🔵 RC:  Replication Controller

    auto increase/decrease pod number for you.
    when pod load is too heavy. 
    it auto deploy another for you. 




🔵 static pod 

    by default k8s auto assign a node for pod.
    but sometimes you want some pod run on a fixed node.
    than use this. 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ConfigMap 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ConfigMap 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ConfigMap 






🔵 configmap vs config 

    you can create configmap from config file.
        but configmap is not config file 
        but configmap is not config file 
        but configmap is not config file 

    during import/create it will add some line to head & end

        kubectl describe cm custom-dashy.yaml
        Name:         custom-dashy.yaml
        Namespace:    default
        Labels:       <none>
        Annotations:  <none>



🔵 How use configmap in pod.yaml

	spec.env      ➜ can give a diff name for pair.
	spec.envFrom  ➜ no change name 
	spec.volumes  ➜ 


    configMapKeyRef  ➜ must choose configmap & choose key.
    configMapRef     ➜ only choose configmap . no need choose key 





🔵 configmap ➜ evn demo ✅

	[root@localhost ~]# vim demo.yaml
	apiVersion: v1  
	kind: ConfigMap               # 🔥 
	metadata:
	name: cm-podxx              # 🔥 ConfigMap name.   pod need this name to use this caonfigmap
	data:                         # 🔥 all list under data is what we set.
	hostname: "k8s"             # Key: hostname   value: k8s
	password: "123"             # Key: password   value: 123
	---
	apiVersion: v1
	kind: Pod
	metadata:
	name: zhangsan
	spec:
	containers:
	- name: zhangsan
		image: busybox

		env:                         # 🔥 use env type
		- name: env-podxx-HostName   # 🔥 set a env name. any name 
		valueFrom:
			configMapKeyRef:         # use configmap.
			name: cm-podxx            # 🔥 choose ConfigMap name;  must same with the name in configmap.
			key: hostname             # 🔥 choose what key to use; must same with the name in configmap.
										# 🔥 no need set value -.-.  

		- name: env-podxx-Password    # 🔥 set a env name
		valueFrom:
			configMapKeyRef:
			name: cm-podxx
			key: password






🔵 configmap ➜ Mount demo ✅


🔶 Create configmap (import conf)

    kubectl create configmap xx                --from-file=yyyy
    kubectl create configmap cm-demo           --from-file=/root/redis.conf
    kubectl create configmap custom-dashy.yaml --from-file=/root/dashy.conf
    kubectl create configmap dashy-config --from-file=/root/dashy.conf



🔶 check configmap:         ➜ kubectl get cm
🔶 check  detail            ➜ kubectl describe cm xxxx




	apiVersion: v1
	kind: Pod
	metadata:
	name: nginx
	spec:
	containers:
	- name: nginx
		image: nginx
		ports:
		- containerPort: 80
		volumeMounts:
		- name: html                        # 🔥 use volume: must same as volumes.name
		mountPath: /var/www/html          # 🔥 set  intermal path
	volumes:
	- name: html                    # 🔥 create volume name:   must same as volumeMounts.name
		configMap:                    # 
		name: nginx-html            # 🔥 must same as the configmap file name in k8s. 







    🔶 deployment 

      deploy ment can deal with serverless docker very good.
      but not stateful docker.
      stateful docker is most difficult part in k8s.
      there is a lot to concern .







🔵 Storage 

        ◎ some pod can     move beacuse they use nfs/smb 
        ◎ some pod can not move beacuse they use rbd/iscsi 

    rbd driver ➜  can mount one  pod only  ➜  ➜ can not move to other node 
    nfs driver ➜  can mount many pod       ➜  ➜ can     move to other node 


        no need high disk performace ➜ can use nfs/smb
        do need high disk performace ➜ must rbd/iscis 

