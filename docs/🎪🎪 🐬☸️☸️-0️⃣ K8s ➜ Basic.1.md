---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️0️⃣ K8s ➜ Basic
---

# K8s ✶ Basic




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 yaml  ✅
🔵 why yaml 

    you can create pod use kubectl ➜  like create docker use cmd
    best choose is use yaml  file  ➜  like docker-compose.yaml 

    in linux everything is file 
    in k8s   everything is yaml

        even driver need use yaml too like ceph-csi storage driver. 

          create a xx.yaml for pod, 
            kubectl create -f xx.yaml
              to run that pod






🔵 yarm Demo 

    K3s ~ cat proxy-traefik.yaml 
    apiVersion: traefik.containo.us/v1alpha1
    kind: IngressRoute
    metadata:
      name: dashboard
    spec:
      entryPoints:
        - web
      routes:
        - match: Host() && (PathPrefix() || PathPrefix())
          kind: Rule
          services:
            - name: api@internal
              kind: TraefikService



🔵 yarm desc 

    yarm have two parts:
        yarm.top:     controler config ➜  k8s config. 
        yarm.other:   controled config ➜  pod config. 


    apiVersion: xxxx     # k8s api version ➜ kubectl api-versions 
    kind: xxxx           # most deployment. 
    metadata:
      name: xxx
      namespace: xxxx   # choose namespace 
    spec:
      replicas: xx       # how many copys. 
      selector:

      template           # pod config blow 
        metadata:
        spec:
          containers





🔵 export yaml file  ✅

    we no need to create yaml all by ourself. 
    just need export a yaml and change some config

      ◎ kubectl create  ➜ export yaml from no installed  pod
      ◎ kubectl get     ➜ export yaml from installed     pod


      all kubectl cmd can export yaml, just add -o yaml ‼️ 


        kubectl create deployment web --image=nginx --dry-run -o yaml 
        kubectl create deployment web --image=nginx --dry-run -o yaml > web.yaml

        kubectl get deploy csi-rbdplugin-provisioner -o=yaml
        kubectl get deploy csi-rbdplugin-provisioner -o=yaml --export > demo.yaml

        kubectl expose deployment web --port=80 type=NodePort --target-port=80 --name=webxx  -o yaml
        kubectl expose deployment web --port=80 type=NodePort --target-port=80 --name=webxx  -o yaml > xx.yaml

---
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 POD  ✅

🔵 why pod 

    k8s is for manager app/pod
    but some app need many container work together.
    and those container under one app must need in same host/node.
    if k8s need move one container in that app. k8s must move all container of that app.

    one pod is like one vm: virtual machine.
    all container under one vm can connect each other.


🔵 Pod Desc 

    POD = Connect Multi namespace & cgroup.
    means Connect Milti Container Together 

    Containeres under same POD. can connect each other ! 
    Containeres under same POD. like under same host/machine ! 

    if app need more than one Containeres to work.
    so best way is make those Containeres use same POD. 
    give every app a different pod.


    🔶 pod simple 

      k8s manager app(pod)
          one app = one or more container 
            all container under same pod use same network & storage 


🔵 pod config 

    image   control 
    restart control   
    source  control   ➜  set/limit   cpu & ram

    health  check     ➜  docker runs not means service runs. 
                          ➜  health check not check pod status, 
                              ➜ check your real service status inside pod.
                      


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Node Choose ✅

    https://www.youtube.com/watch?v=PQHjwA2qZdQ&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=28&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


🔵 how k8s choose node 

  manual choose node:   node name 
  Auto   Choose Node:   resource  /  nodeSelector  /  nodeAffinity 




🔵 node name 

	apiVersion: v1
	kind: Pod
	metadata:
		name: nginx
	spec:
		nodeName: foo-node # use this node 



🔵 node selector 

    one k8s cluster have lots nodes.
        some nodes for prod env.  
        some nodes for test env.
    k8s allow you choose node for pod. 

    first give role to your k8s node.  then let pod choose a role. 


  🔶 get node 

      kubectl get node
      NAME      STATUS   ROLES                  AGE     VERSION
      k3s.vps   Ready    <none>                 4m23s   v1.24.2+k3s2
      k3s.mgr   Ready    control-plane,master   2m30s   v1.24.2+k3s2
      k3s.dkt   Ready    <none>                 10m     v1.24.2+k3s2


  🔶 set env_role 

      kubectl label node k3s.mgr env_role=lan-main
      kubectl label node k3s.dkt env_role=lan-test
      kubectl label node k3s.vps env_role=wan-proxy


  🔶 check env_role 
      kubectl get nodes --show-labels


  🔶 use role 

    spec:
      nodeSelector:
        env_role: lan-main






---
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Controller Kind


🔵 why need controller. 

    controller. can control app even after app deployed.
        change   copy numbers / replicas 
        update   pod  image
        rollback old  version 
        ....





🔵 controller & pod 

    controller  use label to control pod. 
    so put same label in both control and pod 


    Deployment is one of the controller use most. 


		apiVersion: apps/v1
		kind: Deployment
		metadata:
		spec:
		replicas: 1
		selector:
			matchLabels:
			app: web    ➜ 📍
		strategy: {}
		template:
			metadata:
			creationTimestamp: null
			labels:
				app: web  ➜  📍




🔵 Controller.kind ➜ deployment 


🔵 Controller.kind ➜ daemonset 

    let every node run some pod. 
    like log agent. need install to every node.



🔵 Controller.kind ➜ job & cronjob 

    job     ➜ one time job
    cronjob ➜ repeat job many times. 


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Service  

🔵 Why Service  

    service is like firewall. all lan/wan traffic need setup firewall/service  first.
    service is like firewall. all lan/wan traffic need setup firewall/service  first.
    service is like firewall. all lan/wan traffic need setup firewall/service  first.
        any lan traffic ( inside  cluster )
        any wan traffic ( outside cluster )
        

🔵 Service Desc 

    after you deploy your app to k8s, next is visit our app! 

    k8s give every pod a virtual ip. 
    so pods inside k8s can visit each other.

    but you can`t. you are not pods, you are not a part of k8s cluster.
    if your web broswer need visit app. 
    you need setup service first.
    service can help you visit pods inside k8s cluster.
    
    Service put all pods together.
    you visit service. you visit all pods under service.
    it is a bridge between you (out cluster) to pods(in cluster)




🔵 Stateful VS Stateless ✅

    one app:  many copy.
    it is something about copy under one app. 

    state less ➜ all copy under one app. is     same. 
    state ful  ➜ all cpoy under one app. is not same. 


    app-mysql:   mysql-pod-master + mysql-pod-slave.
        this two pod are not same! 
        must run master first. then run slave.

    app-nginx 
        nginx is for load balance.  no master. all pod is equel. 
        so this is state less 



    Stateful:   every copy have a different pod name 
    Stateless:  every copy have a same      pod name 




    Stateful  ➜ use statefulSet to deploy pod
    stateless ➜ use deployment  to deploy pod ➜ nginx usual use deployment.





🔵 Service Mode 

    ClusterIP:              LAN Traffic    ➜ inside  cluster <> inside  cluster 
    NodePort:               WAN Traffic    ➜ inside  cluster <> outsied cluster 
    NodePort.LoadBalance:                  ➜ use proxy like nginx/traefik.


    NodePort:      clusterIp: yes + internal port + host port
    ClusterIP:     clusterIp: yes + internal port
    Headless       clusterIp: no                              ➜ allow you custom config ip. 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Secret 

🔵 why 

	a lot app need set password.
	create mysql need set mysql password.

	in docker-compose we just write passwrod in config. not safe..
	k8s use secret to keep your username & password safe.



	https://www.youtube.com/watch?v=qxaKQY1qiNc&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=38&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


🔵 create secret 

    create secret-xxx.yaml
    put use name passwd in it. 
    apply secret-xxx.yaml
    then your username & password is saved to k8s. 
    kubectl get secret to check.


🔵 use secret 

	- use variable 
	- mount volume 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ConfigMAP 

	https://www.youtube.com/watch?v=MymwImXFUz4&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=39&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


🔵 why 

    docker-compose use xx.yaml to save all docker cmd.
    k8s use configmap to save like hostname/port

    configmap:  save normal   info 
    secret   :  save password info



🔵 create configmap 

	redis.host=redis
	redis.port=1111





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Ingress (router)

	https://www.youtube.com/watch?v=ntLJxQbN3O8&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=44&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


🔵 Why Ingress  

	node port.  only avail on one node! 
	node port. use ip.  in reality we use url.

	ingress is based on nodeport. make it more better.
	ingress is like router.



🔵 ingress workflow 

    ◎ config nodeport first

    ◎ choose  route (choose ingress gateway)   ➜   nginx/traefik 
    ◎ install route (deploy ingress gateway)   ➜   nginx/traefik 

    ◎ config router(ingress rule   )   ➜   config nginx/traefik


