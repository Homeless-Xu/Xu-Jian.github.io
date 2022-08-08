---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️0️⃣ K8s ➜ Basic
---

# K8s ✶ Basic



---
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 POD  ✅


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










🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Secret 



🔵 create secret 

    create secret-xxx.yaml
    put use name passwd in it. 
    apply secret-xxx.yaml
    then your username & password is saved to k8s. 
    kubectl get secret to check.


🔵 use secret 

	- use variable 
	- mount volume 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ConfigMAP & Secret 

	https://www.youtube.com/watch?v=MymwImXFUz4&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=39&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1




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


