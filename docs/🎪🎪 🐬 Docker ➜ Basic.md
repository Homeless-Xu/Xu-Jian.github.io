---
sidebar_position: 2930
title: 🎪🎪🐬 Docker ➜ Basic
---

# Docker Basic




🔵 Container Network Desc  ✅

    🔶 WHY 

        container often need connect other container.
        How make this possible?
        use docker`s network function.

        create a network under docker.
        put docker under same network name.
            the docker under same network can visit other.


    🔶 Container Network 

        ◎ sandbox:        a virtual network for container. 
        ◎ endpoint:       nic inside container. like eth0 eth1..
        ◎ network name:   connect other container.




🔵 Container Network Mode

    🔶 ls network

        Docker-Prod ~ docker network ls
        NETWORK ID     NAME                    DRIVER    SCOPE
        37233e92a107   bridge                  bridge    local
        97f4842d1630   host                    host      local
        1049f50cd70b   netbox-docker_default   bridge    local
        cb7d8f68331e   none                    null      local


    🔶 Mode:

        Bridge    Mode ➜ Default Mode 
        Host      Mode ➜ same as host!  only without ip. 
        None      Mode ➜ container no need network. turn off network.
        Container Mode ➜ for Connect other container 
        Custom    Mode ➜ 


 
    🔶 Container Mode Demo

        docker run -it --name foobar alpine sh
        docker run -it --name test --net=container:foobar alpine sh

            run a container first.
                then add another container to ..container.
                 so foobar & test are in same network 
        

            ◎ check docker network:    docker inspect 9582dbec7981



    🔶 Custom Mode Demo.

        # docker network create test
        # docker network ls
        # docker run -it --name foo --net=test alpine
        # docker run -it --name bar --net=test alpine






🔵 Docker  volume ✅

    🔶 Check 

        docker volume ls
        docker volume create volume-gitea
        docker volume inspect volume-gitea


    🔶 Volume Location 

        /var/lib/docker/volumes

        we move whole docker to nas/iscsi 
            so volume is in nas too.


    🔶 Check Volume 

        docker volume inspect volume-gitea
            "Mountpoint": "/mnt/ceph-img-Docker-prod/docker-prod-root/volumes/volume-gitea/_data",
            "Name": "volume-gitea",





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 Why 

	defaylt all docker/image save in /var/xxxxx 
	it use vm`s system disk.  very small.
	have to use a large one. like ceph disk. 

	of if we want move docker from one vm to another vm.
	if we have ceph. just mount ceph disk to another vm.


🔵 How - Ubuntu_20 ✅


🔶 Stop Docker 

	sudo systemctl stop docker.socket
	sudo systemctl stop docker.service



🔶 Dir prepair 

	mkdir /mnt/ceph-img-Docker-prod/docker-prod-root
	mkdir /mnt/ceph-img-Docker-prod/docker-prod-data

	mkdir /mnt/ceph-img-Docker-test/docker-test-root
	mkdir /mnt/ceph-img-Docker-test/docker-test-data



🔶 Edit config 

	sudo vi /lib/systemd/system/docker.service
		add -g /new/path/docker before -H

	ExecStart=/usr/bin/dockerd -H fd:// xxxx
	ExecStart=/usr/bin/dockerd -g /new/path/docker -H fd:// xxxxx

	ExecStart=/usr/bin/dockerd -g /mnt/ceph-img-Docker-prod/docker-prod-root -H fd:// xxxxx
	ExecStart=/usr/bin/dockerd -g /mnt/ceph-img-Docker-test/docker-test-root -H fd:// xxxxx


🔶 Move Older data

	sudo rsync -aqxP /var/lib/docker/ /new/path/docker
	sudo rsync -aqxP /var/lib/docker/ /mnt/ceph-img-Docker-prod/docker-prod-root
	sudo rsync -aqxP /var/lib/docker/ /mnt/ceph-img-Docker-test/docker-test-root


🔶 Start server 

	sudo systemctl daemon-reload
	sudo systemctl start docker


🔶 Check 

	ps aux | grep -i docker | grep -v grep

	






🔵 Namespace & cgroup ✅

    Docker Container = Namespace + cgroup 


  🔶 Namespace 

    ‼️ Namespace Function:   quarente ‼️
  
    K8s Default Namespace:   Default / Kube-system / Kube-public  
    
    like python version.
    some app need python2. other need python3.
    if you only have one vm.  need both run python2 & python3. 
    you may need namespace.   
    create one namespace install python2 
    create one namespace install python3 
      let different app run at different namespace.
      so both app can run good! 


  🔶 cgroup 
  
    in linux one task can use all host resource: all cpu/ram/disk 
    but this will make other task slow/die 
    so they create cgroup:  limit the resource task can use.  
    cgroup make docker contain use how much cpu/ram/disk possible. 
    every container have a different cgroup. 
    and you can set how much resource that cgroup/container can use .

