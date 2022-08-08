---
sidebar_position: 3930
title: 🎪🎪🎪-1️⃣🛢 DB ➜ Redis
---

# Redis


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 
🔵 Redis Desc 

    use ram as default storage.
    can can save data to disk storage too.
    depend how you config it. 



🔵 Bitnami redis image  
    Bitnami improve a lot on docker.   like safety . etc. 
    so we use bitnami `s docker image .
    bitnami/redis


🔵 Redis GUI tool 
    redis-insight is best 


🔵 redis cli tool 

    redis-cli

        sudo apt-get install redis-tools -y







🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Redis - Docker  ✅

 
     DBredis:
        container_name: DB-Redis
        image: 'bitnami/redis:latest'
        ports:
            - "6379:6379"
        environment:
            - ALLOW_EMPTY_PASSWORD=yes
        volumes:
            - /mnt/dpnvme/DMGS-DKP/DB-Redis:/bitnami


    🐞 

    ‼️ ⭐️  docker not run as root.  most tun as user 1001. 
    ‼️ ⭐️  docker not run as root.  most tun as user 1001. 
    ‼️ ⭐️  docker not run as root.  most tun as user 1001. 

        /bit/nami/redis  permission denied

            The idea behind all this is that the user with uid 1001 inside the container should be able to write to /bitnami/redis/data.


                sudo chown 1001 host folder path
                sudo chown 1001 /mnt/dpnvme/DMGS-DKP/DB-Redis


