---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️☸️ Demo ➜ ❌
---


# K8s Real Demo ➜




🔵 todo 
    move all docker-compose to k3s/k8s 


	fa9928b172e1   jumpserver/jms_all:latest          "./entrypoint.sh"        2 days ago      Up 2 hours                                                                            APP-Jumpserver
	ca4884bacf92   ghcr.io/requarks/wiki:2            "docker-entrypoint.s…"   2 days ago      Up 10 seconds                                                                         NoteWikijs
	3aefa5a3e0be   minio/minio:latest                 "/usr/bin/docker-ent…"   2 days ago      Up 2 hours                                                                            STO-S3-MinIO
	6befd0f41f87   postgres:latest                    "docker-entrypoint.s…"   2 days ago      Up 2 hours                     0.0.0.0:5432->5432/tcp, :::5432->5432/tcp              DB-Postgres-DKP
	bb833dc88257   gravitl/netclient:v0.14.3          "/bin/bash ./netclie…"   2 days ago      Restarting (0) 7 seconds ago                                                          VPN-NetmakerCLI-DKP
	236ed4f2807a   mysql:latest                       "docker-entrypoint.s…"   2 days ago      Up 2 hours                     0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   DB-MySQL-DKP
	60f41c6b1851   linuxserver/heimdall               "/init"                  2 days ago      Up 2 hours                                                                            DashBoard-Heimdall
	a8cf52dcd68f   travelping/nettools:latest         "tail -F anything"       2 days ago      Up 2 hours                                                                            Net-Debug-ToolBOX
	409a1fd3a37c   gitea/gitea:latest                 "/usr/bin/entrypoint…"   2 days ago      Up 10 seconds                                                                         APP-Gitea-dkp
	077cdb28bdb2   traefik:latest                     "/entrypoint.sh --lo…"   2 days ago      Up 2 hours



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 k3s prepair 

🔵 namespace 

    ns-app
    ns-db
    ns-tool
    ns-proxy



🔵 network 



🔵 storage 

    default:     local-path 
    ceph pvc 




sto-sc-cephcsi-rbd-K3s


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 mysql - heml  ❌ 

	https://artifacthub.io/packages/helm/bitnami/mysql


	helm repo add bitnami https://charts.bitnami.com/bitnami

	helm install my-release bitnami/mysql
	helm install mysql-test bitnami/mysql --namespace ns-db --set global.storageClass=local-path









🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 nettools 

🔵 docker compose demo 

    netdebug:
        container_name: Net-Debug-ToolBOX
        image: travelping/nettools:latest
        restart: always
        command: tail -F anything
            # keep docker running.   
            # some docker no have task. will auto shutdown.


🔵 K8s - CMD 

    kubectl run netdebug --image=travelping/nettools -- sleep infinity  ✅



🔵 k8s - yml 

🔶 create namespace:      ns-tool-net
🔶 create pod  yaml:      toolNetNettoolsPod.yaml

	apiVersion: v3
	kind: Pod
	metadata:
	name: toolNetNettoolsPod
	namespace: ns-tool-net
	spec:
	containers:
	- name: toolNetNettoolsPod
		image: travelping/nettools:latest
		command:
		- sleep
		- "infinity"
		imagePullPolicy: IfNotPresent
	restartPolicy: Never



🔶 apply      ➜        kubectl apply -f /mnt/tool-net-nettools-pod.yaml

	pod/tool-net-nettools-pod created


	Failed to create pod sandbox: rpc error: code = Unknown desc = failed to start sandbox container for pod "tool-net-nettools-pod": E
	rror response from daemon: 
	failed to update store for object type *libnetwork.endpointCnt: Key not found in store



🔶 1. import to configmap .

	kubectl run -it --image=jrecord/nettools nettools --restart=Never --namespace=default




	kubectl apply -f nettools.yaml





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 K8s ✶ Nginx  ✅

    🔶 Deploy               kubectl create deployment nginx --image=nginx
    🔶 Export server Port   kubectl expose deployment nginx --port=80 --type=NodePort
    🔶 Check local Port     kubectl get svc
    🔶 Visit                http://172.16.0.80:32461/


==
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Docker ✶ NetBox ✅ 

    🔵 Docker Netbox ✅

        ⭐️⭐️⭐️⭐️⭐️
        https://computingforgeeks.com/how-to-run-netbox-ipam-tool-in-docker-containers/


        ⭐️⭐️⭐️ - offical manual.
        https://github.com/netbox-community/netbox-docker

        docker-compose up -d
        web:    8000
        admin / admin  


==
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 K8s ✶ NetBox ❌ 
==
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 K8s_Local ✶ Net-Tool  ✅

    🔵 Docker Demo 

        🔶 Desc 

            a docker with net tooles.
            good for test network/debug 

        🔶 Run 

            docker run -dt travelping/nettools


    🔵 Minikube Demo 

        🔶 run 

            kubectl create deployment netdebug --image=travelping/nettools      ❌
            kubectl run netdebug --image=travelping/nettools -- sleep infinity  ✅

                ‼️ docker must have a task that will never finish. or docker will auto stop it ‼️
                ‼️ docker must have a task that will never finish. or docker will auto stop it ‼️
                ‼️ docker must have a task that will never finish. or docker will auto stop it ‼️


        🔶 enter docker 

            kubectl exec <podname> -it -- bash
            kubectl exec netdebug -it -- bash



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Misc 

	kubectl run my-app --image=gcr.io/some-repo/my-app:v1 --port=3000

	$ kubectl expose deployment my-app --type=LoadBalancer --port=8080 --target-port=3000
	service "my-app" exposed




