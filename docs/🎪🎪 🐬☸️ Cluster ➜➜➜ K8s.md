---
sidebar_position: 2930
title: 🎪🎪🐬☸️ Cluster ➜➜➜ K8s
---

# K8s Build





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Basic info 

🔵 Why k8s 

    docker 
        >> docker compose    -- for simple use
            >> swarm         -- no recommand
                >> k8s       -- final 


    docker is everywhere.
    when you have lots docker. you need manager. 
    k8s /k3s can help you.


🔵 K8s / K3s 

    K3s is light weight.  ➜ less ram   (0.5G ram +)
    K8s is heavy weight.  ➜ much ram   

    k8s have cluster build tool:   kubeadm.

    ◎ K8s Version 
        https://kubernetes.io/releases/
            1.24+ 


🔵 minikube

    for test only.    we need one.
    learn on minikube. then try on k8s.


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  K8s  Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  K8s  Demo 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  K8s  Demo 


🔵 require 

    🔶 ubuntu 20

    ‼️ 🔥 NO Docker installed! Docker has buildin xxx. but we need CRI-O ‼️
    ‼️ 🔥 NO Docker installed! Docker has buildin xxx. but we need CRI-O ‼️
    ‼️ 🔥 NO Docker installed! Docker has buildin xxx. but we need CRI-O ‼️


        🔶 uninstall old docker 

            $ sudo apt-get purge -y docker-engine docker docker.io docker-ce  
            $ sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  


    ‼️ 🔥 at least two node.  or  k8s dashboard no work. ‼️


    ram must 2G + .  most vps no....



🔵 VM Prepair


    🔶 set Hostname & static IP   ???

        K8s-Manager       172.16.0.80
        K8s-WorkerG3      172.16.0.83


            hostnamectl set-hostname your-new-hostname
            hostnamectl set-hostname K8s-Worker-VPS

            client only need add manager`s hostname?
            what if some nodes not in same vlan.





🔵 Disable swap - All Node 

    https://graspingtech.com/disable-swap-ubuntu/


    🔶 Check 

        sudo swapon --show
        if nothing it is disabled.


    🔶 forever turn off swap 

       sudo vi /etc/fstab
            remove /swap line 
                /swap.img       none    swap    sw      0       0


🔵 Config firewall 

    🔶 like this is ok.

        [root@localhost ~]# lsmod |grep br_netfilter
        br_netfilter           22209  0
        bridge                136173  1 br_netfilter


 


🔵 Install CRI-O - ALL Node  ✅

        ‼️ if you have docker/contained install already. something will wrong. ‼️
            use a new system. No install docker/contained! 


    🔶 Set OS & CRI-o version value

        K8s-Mgr.Root ~ OS=xUbuntu_20.04
        K8s-Mgr.Root ~ VERSION=1.24
            🔥 must run this.   set system version
            ◎ find your system
                https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/

            ◎ check lsteat cri-o version 
                https://github.com/cri-o/cri-o
                    should be same version with k8s.


    🔶 Copy & Paste 
			
		# Add Kubic Repo
		echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" | \
		sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list

		# Import Public Key
		curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | \
		sudo apt-key add -

		# Add CRI Repo
		echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" | \
		sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

		# Import Public Key
		curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | \
		sudo apt-key add -


    🔶 Install 

        sudo apt update
        sudo apt install cri-o cri-o-runc cri-tools -y


    🔶 enable

        sudo systemctl enable crio.service


    🔶 start & check 

        sudo systemctl start crio.service
        sudo crictl info
            ‼️ make sure runtime is ready.  network ignore it. ‼️


 
🔵 Kubernetes tooles install - ALl Node ✅

    🔶 Tooles Desc 

        ◎ kubeadm:    Deploy k8s cluster
        ◎ kubelet:    manager pod/network 
        ◎ kubectl:    k8s cli tool.


    🔶  add key 

        curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add


    🔶 add apt repository 

        echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" >> ~/kubernetes.list
        sudo mv ~/kubernetes.list /etc/apt/sources.list.d


    🔶 Install  tooles 

        sudo apt-get update

        sudo apt-get install -y kubelet kubeadm kubectl

        sudo apt-mark hold kubelet kubeadm kubectl  
            apt-mark means no update these package when you update your system!
            update package often cause problems.
            

 



🔵 Create Cluster (Mgr node) ✅

    🔶 Create 

        sudo kubeadm init --pod-network-cidr=10.244.0.0/16

            kubeadm join 172.16.0.80:6443 --token qrtig7v35 \
                --discovery-token-ca-cert-hash sha256:d6

🔵 check join cmd 

    if forget join cmd. 
    check 

kubeadm token create --print-join-command

kubeadm join 172.16.0.80:6443 --token oi0nrh.ns09no --discovery-token-ca-cert-hash sha256:7






🔵 Worker join cluster.

    kubeadm join 172.16.0.80:6443 --token qrti3b.3tg7v35 \
                    --discovery-token-ca-cert-hash sha256:f83007a


    🔶 ? 

🔵 Reset K8s Cluster - option 

  https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-reset/
  https://www.techrunnr.com/how-to-reset-kubernetes-cluster/

  kubeadm reset -f
    ‼️ All node need reset. ‼️




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Manager config dashboard 
🔶

	mkdir -p $HOME/.kube
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	sudo chown $(id -u):$(id -g) $HOME/.kube/config


🔶 firewall 

    sudo ufw allow 6443
    sudo ufw allow 6443/tcp


🔵 flannel plugin 

    🔶 function 

        manage docker network 
        like subnet. 


    🔶 Install

        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml


    🔶 check status

        kubectl get pods --all-namespaces
        kubectl get componentstatus
        kubectl get cs



 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 K8s Dashboard 

🔵 K8s dashboard 


    https://github.com/kubernetes/dashboard


    https://github.com/kubernetes/dashboard/releases


🔶 install 

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml


    ◎ delete/remove - option
        kubectl delete -f xxxx


🔶 Check dashboard stauts

    kubectl get pods --all-namespaces
        make sure  kubernetes-dashboard is running .  not pendding ..

            if not running check name spcae log
                kubectl get events -n <namespace> 
                kubectl get events -n kubernetes-dashboard
 



🔶 Dashboard Login Desc 

    by default. 
    only allow local machine (k8s manager node) to login.

    we need login dashboard from remote machine.



🔶 Dashboard ▪ Change  type 

    kubectl -n kubernetes-dashboard edit service kubernetes-dashboard

        k8s-app: kubernetes-dashboard
        sessionAffinity: None
        type: NodePort   ➜ change  from clusterip to nodeport 


🔶 Dashboard ▪ get web port (443:30484)

    K8s-Manager ~ kubectl get svc -n kubernetes-dashboard
    NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
    dashboard-metrics-scraper   ClusterIP   10.102.194.58   <none>        8000/TCP        4m41s
    kubernetes-dashboard        NodePort    10.97.178.197   <none>        443:xxxxx/TCP   4m41s



🔶Dashboard ▪ Log in 

    https://172.16.0.80:xxxxx/
    https://172.16.0.80:30775

    need token.
    we need create user first.  
    then create token for that user
    then can back.



🔵 Create Dashboard user 

    create two yaml file:  xxx.yaml
    use kubectl apply -f xxx.yaml to apply.


🔶 File_01:   miranda.yaml


	sudo bash -c 'echo "
	apiVersion: v1
	kind: ServiceAccount
	metadata:
	name: miranda
	namespace: kubernetes-dashboard
	" > /root/miranda.yaml'



🔶 File_02:  ClusterRoleBinding.yaml

	sudo bash -c 'echo "
	apiVersion: rbac.authorization.k8s.io/v1
	kind: ClusterRoleBinding
	metadata:
	name: miranda
	roleRef:
	apiGroup: rbac.authorization.k8s.io
	kind: ClusterRole
	name: cluster-admin
	subjects:
	- kind: ServiceAccount
	name: miranda
	namespace: kubernetes-dashboard
	" > /root/ClusterRoleBinding.yaml'







🔶 apply yaml

    kubectl apply -f miranda.yaml
    kubectl apply -f ClusterRoleBinding.yaml

        ◎ delete yaml (option )
            kubectl delete -f  miranda.yaml


🔶 get user token & login 

    kubectl -n kubernetes-dashboard create token miranda

    cpoy token to website. 
    so we can login. now.

 




