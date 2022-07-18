---
sidebar_position: 3930
title: 🎪🎪🎪-5️⃣💠💠💠 ➜ Jump Server
---

# Jump Server



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Jump Server.
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Jump Server.
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Jump Server.

pc (internet)  <====>   vps (internet)
    vps (wireguard.Srv)   <======>   homelab-JumpServer: wireguard.Cli
        homelab-JumpServer <======> Homelab-other machine 


🔵 WHY 

    visit homelab`s internel server  via internet is a must.
    you can install vpn to any client in  homelab
    but best way is use jumpserver.

    any want connect into your homelab. 
    need login to jumpser first. then type in usernam/password.
    



🔵 Jump Server Function
    log everything you do! 



🔵 OpenSource Jump Server
    
    jumpserver/ teleport
    https://www.jumpserver.org/


    🔶jms_all
        https://hub.docker.com/r/jumpserver/jms_all

    need mysql


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Prepair  - mysql 

🔵 Login 

    mycli -h DockerProd.ark -P 3306 -u root
    SHOW DATABASES;

🔵 Check Database 

    ◎ simple info ➜   SHOW DATABASES;
    ◎ detail info ➜   select * from information_schema.schemata;


🔵 create DB: DBjumpserver ✔️

    create database DBNAME CHARACTER SET utf8 COLLATE utf8_bin;
        create database DBjumpserver CHARACTER SET utf8 COLLATE utf8_bin;
            ‼️ Database coding must requirements  uft8 ‼️
            ‼️ Database coding must requirements  uft8 ‼️


🔵 Create user: ✅

    CREATE USER 'USERjumpserver'@'%' IDENTIFIED BY 'password';

        % only means. can remote to mysql use server`s lan ip.
        or bydefault. not allow user remote login.


🔵 Give Permit ✅

    GRANT ALL PRIVILEGES ON DBjumpserver.* TO 'USERjumpserver'@'%';
    FLUSH PRIVILEGES;

        only give USERjumpserver access DBjumpserver. 
        no access other db on mysql.


🔵 mysql remote test 

    mycli -h 172.16.1.140 -P 3306 -u USERjumpserver
    show databases;





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 create key

🔶 WHY 

    the data in jumpserver is encrypted! 
    you must create a key when create jumpserver.
    if need update docker some day.
    use the same key to unlock the old encrypted data ??

    you can create key any linux machine.  (linux need have /dev/urandom )
    just need keep the key! 



🔶 Create SECRET_KEY

    if [ "$SECRET_KEY" = "" ]; then SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`; echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc; echo $SECRET_KEY; else echo $SECRET_KEY; fi


🔶 Create  BOOTSTRAP_TOKEN

    if [ "$BOOTSTRAP_TOKEN" = "" ]; then BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`; echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc; echo $BOOTSTRAP_TOKEN; else echo $BOOTSTRAP_TOKEN; fi




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Run Jumpserver

🔵 First time 

    need tell jumpserver docker how connect db.
    after connected.  jumpserver docker will create some mysql file.
    then copy that data out.

    next time. run a new jumpserver docker 
    no need type in mysql info.
    just need mount the mysql folder.


🔶 first run

    docker run --name DPjumpserver -d \
      -v /mnt/ceph-img-Docker-prod/DPdata/jumpserver/data:/opt/jumpserver/data \
      -p 80:80 \
      -p 2222:2222 \
      -e SECRET_KEY=vvLHJn9gLneiZsXtPbr019zvFsUE4AmgyNM7psyMbMkWuu7tMf \
      -e BOOTSTRAP_TOKEN=f48gD26NeNgl2QfD \
      -e DB_HOST=dockerprod.ark \
      -e DB_PORT=3306 \
      -e DB_USER=USERjumpserver \
      -e DB_PASSWORD=xxxxxxxx \
      -e DB_NAME=DBjumpserver \
      jumpserver/jms_all:latest





🔵 web login 

    http://172.16.1.140/ui/#/
    http://172.16.1.140/core/auth/login/

    ‼️ fuck chrome no web. safari works. ‼️ 
    ‼️ fuck chrome no web. safari works. ‼️ 
    ‼️ fuck chrome no web. safari works. ‼️ 


need long time to start. 