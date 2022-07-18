---
sidebar_position: 2930
title: 🎪🎪🐬 Docker ➜➜➜ Swarm 
---

# Docker Swarn 


🦚 Goal 

    wan:  vps  VPN.S_1214.214  ➜ swarm worker2 
    lan:  dkp  VPN.C_1214.140  ➜ swarm master
    lan:  dkt  VPN.C_1214.144  ➜ swarm worker1


    -.- use wireguard build vpn first. 





🔵 swam & network 

    🔶 manager node:   Make docker node as manager

        docker swarm init --advertise-addr 10.214.214.140


    🔶 work node  (join to swarm)

        docker swarm join --token SWMTKN-1-11a4qe1waxtyntm9g-7wr1sfz3nyn2w91tjhtt78fmg 172.16.1.140:2377


    🔶 Manager node: (check all nodes)

        docker node ls


    🔶 All Node (check swarm network)

        docker network ls
        all node should have a ingress overlay network
            7jyu8yhpb142   ingress                           overlay   swarm


    🔶 Manager node: (create overlay network)

        docker network create -d overlay --attachable DNET-Overlay-Traefik

            ‼️ if many docker machine need use same docker network. need use overlay. ‼️
            ‼️ even use overlay. if need use between diff hosts. need use --attachable ‼️ 
            it will auto sync to all swarm node.



    🔶 Work Node (check if network auto created )

        docker network ls

            ‼️ node only create network when need it. ‼️
            if need need use it.  it will auto created.
            🔥 if node no need it/no docker require that network.  it will not auto created! ...  fuck
            🔥 if node no need it/no docker require that network.  it will not auto created! ...  fuck
            🔥 if node no need it/no docker require that network.  it will not auto created! ...  fuck


    🔶 test network 

        manager node:   docker run -dit --name alpine1 --network DNET-Overlay-Traefik alpine
        work ndoe:      docker run -dit --name alpine2 --network DNET-Overlay-Traefik alpine

            then work ndoe can see the synced network
            Docker-Test.Root ~ docker network list
            NETWORK ID     NAME                              DRIVER    SCOPE
            y20sdrp0g3j2   DNET-Overlay-Traefik              overlay   swarm


                enter alpine2 & ping alpine1
                Docker-Test.Root ~ docker exec -it 405e8f3a6c50 ash




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵



🔵 log 

	You need to ensure that the requisite network ports are open between the swarm nodes.

	TCP port 2377 for cluster management communications
	TCP and UDP port 7946 for communication among nodes
	UDP port 4789 for overlay network traffic




	docker info"




🔵 

	To deploy your application across the swarm, use `docker stack deploy`.

	docker compose swarm




🔵 how use swarm

	all cmd must run from manager node..
	after worker node joined swarm.  no need login worker any more..



	fuck learn swarm . why not learm k8s.
	swarm is between docker compose and k8s. 
	not good .
