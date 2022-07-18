---
sidebar_position: 3930
title: 🎪🎪🎪-5️⃣💠💠💠 ➜ Gittea
---

# Gittea


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 HomeLAB-Gittea 
🔵 Offical Manual 
    https://docs.gitea.io/en-us/install-with-docker/


🔵 Prepair

🔶 Install Docker-compose 


🔶 Create Custom volume

    docker volume ls
    docker volume create volumegitea
    docker volume create volumegiteadb


🔶 Create Custom network 

    docker network create network-gitea
    docker network ls


🔶 Create DockerFile  💯


    cat <<EOF >> docker-compose.gitea.yml
    version: "3"

    networks:                         # ‼️ any network use in container. must add here first
      network-gitea:                  # ‼️ changeme 
        external: false

    volumes:                          # ‼️ any volume use in container. must add here first
        volumegitea:                  # ‼️ changeme 
          external: true
        volumegiteadb:                # ‼️ changeme 
          external: true

    services:
      server:
        image: gitea/gitea:latest
        container_name: gitea
        environment:
          - USER_UID=1000
          - USER_GID=1000
          - DB_TYPE=postgres
          - DB_HOST=db:5432
          - DB_NAME=gitea
          - DB_USER=gitea
          - DB_PASSWD=gitea
        restart: always
        networks:
          - network-gitea                 # ‼️ changeme 
        volumes:
          - volumegitea:/data             # ‼️ changeme 
          - /etc/timezone:/etc/timezone:ro
          - /etc/localtime:/etc/localtime:ro
        ports:
          - "3000:3000"                    # changeme - option
          - "222:22"                       # changeme - option
        depends_on:
          - db

      db:
        image: postgres:latest
        restart: always
        environment:
          - POSTGRES_USER=gitea
          - POSTGRES_PASSWORD=gitea
          - POSTGRES_DB=gitea
        networks:
          - network-gitea                 # ‼️ changeme 
        volumes:
          - volumegiteadb:/var/lib/mysql  # ‼️ changeme 
    EOF




🔶 run 

    docker-compose up -d
    docker-compose -f filepath up -d
    docker-compose -f docker-compose.gitea.yml up -d


🔵 web login 
    ip:3000

    ◎ chalge all localhost to your serverip.
    ◎ set admin password
    ◎ install 






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 custom DB version 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  LAB ▪ Base ▪ gitea

🔵 Git Desc

    can auto backup everything you change.  good for file/blog
    you delete by mistake.  git allow visit histroy version any time.

    create a folder.
    enable folder with git
    set auto backup time.
    write blog in it.


🔵 WHY

    all important file 
        - docker config file.
        - notes/blogs 


🔵 Git Tooles 

    compare:     https://docs.gitea.io/zh-cn/comparison/

    public: github.
    private: gitea          ➜ recommond
    synology: git server 



🔵 Gitea desc 

    need database..  postgress..

 

🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 DB - Postgress

🔵 Docker Postgress.

    docker run -d \
    --restart always \
    --name DKP-postgres \
    -p 5432:5432 \
    -e POSTGRES_PASSWORD=xxxx \
    -e PGDATA=/var/lib/postgresql/data/pgdata \
    -v /mnt/dpnvme/DMGS-DKP/postgres/data:/var/lib/postgresql/data \
    postgres


 

🔶 test login 

    db: postgress 
    user: postgress 
    pwd :  your ..


🔵  Postgress GUI / CLI toole 
    🔶 GUI 
    https://postgresapp.com/documentation/gui-tools.html


    🔶 install  psql 

        sudo apt-get install postgresql-client -y
        brew install libpq

    🔶 CLI 
      
      psql -U username -h localhost -p 5432 dbname
      psql -U postgres -h localhost -p 5432 postgres
      psql -U usergitea -h 172.16.1.140 -p 5432 dbgitea


🔵 Config User & DB 

    🔶 create user

        CREATE ROLE gitea WITH LOGIN PASSWORD 'gitea';
        CREATE ROLE USERgitea WITH LOGIN PASSWORD 'Yshskl0@19';


    🔶 create DB

        CREATE DATABASE DBgitea WITH OWNER USERgitea TEMPLATE template0 ENCODING UTF8 LC_COLLATE 'en_US.UTF-8' LC_CTYPE 'en_US.UTF-8';


    🔶 remote test 

        psql -U usergitea -h 172.16.1.140 -p 5432 dbgitea


    🔶 final 

        dbname:      dbgitea
        username:    usergitea




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 run gitea 

    docker run -d \
    -p 2299:22 \
    -p 3000:3000 \
    --name DKPgitea \
    --restart always \
    -v /mnt/dpnvme/DMGS-DKP/gitea/data:/data \
    gitea/gitea:latest


    ‼️ no need config db when run docker. config in web ‼️



🔵 Login 

    http://172.16.1.140:3000/



🔵 first Config 

    rooturl chang to ip.
    or web url willbe ..
    http://localhost:3000/ 
    http://172.16.1.140:3000/user/login


    /mnt/dpnvme/DMGS-DKP/gitea/data/gitea/conf/app.ini 


🔵 create account 

🔵 login
