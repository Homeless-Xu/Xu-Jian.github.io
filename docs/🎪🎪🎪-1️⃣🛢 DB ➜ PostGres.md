---
sidebar_position: 3930
title: 🎪🎪🎪-1️⃣🛢 DB ➜ Postgres
---

# Postgres




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 DB - Postgress

 

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


