---
sidebar_position: 1720
title: 🎪-7️⃣🌐🌐 Proxy ➜ Teaefik
---



# Net ✶ Proxy


🔵 Demo  

    WAN-VPS: 172.93.42.232 ➜  VPN.Srv: 10.214.214.254    ➜  traefik_vpn
    LAN-DKP: 172.16.1.140  ➜  VPN.CLI: 10.214.214.140    ➜  traefik_local  (port 8888) 
    LAN-DVM: 10.1.1.89     ➜  LAN                        ➜  dashy          (port 8099) 

        traefik.0214.icu >>>  172.93.42.232 (traefik forward to lan) >> 172.16.1.140:8888
        dashy.0214.icu   >>>  172.93.42.232 (traefik forward to lan) >> 10.1.1.89:8099


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 custom traefik

🔵 How add rotue use docker-compose file

    🔶 use entrypoint.sh 
        most docker have a default script:   entrypoint.sh
        this shell run when docker start.
        so just need add route cmd to entrypoint.sh file.
            1. copy entrypoint.sh out from docker.
            2. add route cmd into entrypoint.sh 
            3. mount changed entrypoint.sh back to docker. 
                    change entrypoint.sh file permit.  so docker can mount it .
                        chmod 777 entrypoint.sh


                            #!/bin/sh
                            set -e
                            route add -net 10.214.214.0 netmask 255.255.255.0 gw netmaker
                            route add -net 172.16.1.0 netmask 255.255.255.0 gw netmaker
                            route add -net 10.1.1.0 netmask 255.255.255.0 gw netmaker

                            # first arg is `-f` or `--some-option`

        ‼️ by default container no have prtmit add route. need give permit ‼️
        ‼️ by default container no have prtmit add route. need give permit ‼️
        ‼️ by default container no have prtmit add route. need give permit ‼️

            in docker-compose file 
            add  this under traefik

            cap_add:
            - NET_ADMIN




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  traefik http forward demo ✅

🔵 How 

    - create all.yaml 
    - mount xxx.yaml to docker traefik
            - /root/all.yaml:/all.yaml
    - docker traefik use command load custom.yaml.
            - "--providers.file.filename=/all.yaml"


    🔶 add endpoints: web 

      - "--entrypoints.web.address=:80"
      i not config ssl yet.  so need use http. 
      if have ssl. no need this. 


🔵 vi Traefik-Forward-Config.yaml


http:
  routers:
    RH-Dashy-DVM:
      service: SH-Dashy-DVM
      entrypoints: web
      rule: "Host(`dashy.0214.icu`)"
    RH-Traefik-DPK:
      service: SH-Traefik-DKP
      entrypoints: web
      rule: "Host(`traefik.0214.icu`)"

  services:
    SH-Dashy-DVM:
      loadBalancer:
        servers:
        - url: http://10.1.1.89:8099
    SH-Traefik-DKP:
      loadBalancer:
        servers:
        - url: http://172.16.1.140:8888



  🔶 visit  

      must  use safari.  or chrome incognito mode (in case have cash!!!! )
      must  use safari.  or chrome incognito mode (in case have cash!!!! )
      must  use safari.  or chrome incognito mode (in case have cash!!!! )


    http://traefik.0214.icu 
    http://dashy.0214.icu



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Manyal SSL Config Demo 💯


🔵 insecureskipverify ???

    - "--serverstransport.insecureskipverify=true"

    when traefik visit website inside homelab.
    it can ignore tls error. 
    allow you visit with tls error.



🔵 How (manual apply ssl)

  - use certbot manyal apply ssl 
  - copy ssl out & mount ssl into traefik 
  - config traefik yaml & mount
  - let traefik load yaml 



🔵 apply SSL 

  certbot certonly  -d "*.0214.icu" --rsa-key-size 4096 --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
    ... 
    follow it.  go dns website.  add some record to your dns  and comeback.
    ... 

    # make sure that private key are 4096 bit long. 2048 bit will NOT work ‼️ 
        # openssl rsa -text -in privkey.pem | grep bit
        # writing RSA key
        # RSA Private-Key: (2048 bit, 2 primes)  ➜ this no work. change one ...


🔵 copy ssl out 

    certFile: /root/ssl-nodel/fullchain.pem
    keyFile: /root/ssl-nodel/privkey.pem



🔵 Config docker compose file 

  🔶 Mount 

    - /root/ssl-nodel:/certs
    - /root/all.yaml:/all.yaml


  🔶 let traefik load all.yaml 

      command:
        - "--providers.file.filename=/all.yaml"


  🔶  delete netmaker default ssl config line 

          - "--entrypoints.websecure.http.tls.certResolver=http"     # delete this 



🔵 config  demo all.yaml  ✅


http:
  routers:
    RH-Dashy-DVM:
      service: SH-Dashy-DVM
      entrypoints: websecure
      rule: "Host(`dashy.0214.icu`)"
    RH-Traefik-DPK:
      service: SH-Traefik-DKP
      entrypoints: websecure
      rule: "Host(`traefik.0214.icu`)"

  services:
    SH-Dashy-DVM:
      loadBalancer:
        servers:
        - url: http://10.1.1.89:8099
    SH-Traefik-DKP:
      loadBalancer:
        servers:
        - url: http://172.16.1.140:8888



tls:
  stores:
    default:
      defaultCertificate:
        certFile: /certs/fullchain.pem
        keyFile: /certs/privkey.pem





🔵 visit  

  🔶 visit  

      must  use safari.  or chrome incognito mode (in case have cash!!!! )
      must  use safari.  or chrome incognito mode (in case have cash!!!! )
      must  use safari.  or chrome incognito mode (in case have cash!!!! )



  https://traefik.0214.icu
    fuck. this must allow cookie to work... -.- 

  https://dashy.0214.icu









🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 netmaker  traefik demo 


traefik:
    image: traefik:latest
    container_name: traefik
    cap_add:
      - NET_ADMIN
    command:
      - "--api.insecure=true"
      - "--providers.file.filename=/custom.yaml"

      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
      - "--entrypoints.websecure.http.tls=true"

      - "--log.level=INFO"
      - "--providers.docker=true"
      - "--providers.docker.exposedByDefault=false"
      - "--serverstransport.insecureskipverify=true"
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro

      - /root/entrypoint.sh:/entrypoint.sh
      - /root/Traefik-Forward-Config.yaml:/custom.yaml
      - /root/ssl-nodel:/certs


    ports:
      - "443:443"
      - "80:80"
      - "8089:8080"




























🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Traefik
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Traefik
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Traefik

🔵 WHY  Reverse Proxy ✅

    one host only one 80 443
    docker allow us run a lot website in one host.
    you can give different website differnet port.
    but Reverse Proxy allow all website use 80/443 at same time.
    just use different url.

    all 80/443 request go to Reverse Proxy docker first.
    let Reverse Proxy docker forward to different website depends on different url.


    Reverse Proxy can do much more function




🔵 traefik Desc ✅  

    traefik like nginx`s Reverse Proxy function. but do better at Reverse Proxy
    traefik like nginx`s Reverse Proxy function. but do better at Reverse Proxy
    traefik like nginx`s Reverse Proxy function. but do better at Reverse Proxy
  

    router  connect data between wan & lan
    fraefik connect url  between Domain & ip

        router deal with data;  fraefik deal with url.

        point all domain dns to fraefik server.
            let fraefik forward request to your real internel server.


    🔶 nginx ingress  vs traefik ingress

        https://www.modb.pro/db/197174



🔵 How Use traefik ✅

    traefik need use docker.sock;  means have the same permit as docker.
    so traefik can auto config almost everything for you.
    traefik is enabled on all docker by default.
        means. if you login traefik dashboard.
        traefik almost auto config all for you.
        you only need change a little! 

    but auto config is good for beginner.
    at end we usually disable auto config.



🔵 traefik config

  two kinds config

  static config  ➜ config not change often
  donynic config ➜ config chnage     often 

      all kinds config can use 
          config file:   mount file: traefik.yml
          cli:  set when run docker/docker-compose


              for easy understand.  
              we set all in docker-compose file.
              not use any traefik.yml file.



      🔶 providers. ✅

          where traefik runs:       docker/k8s ..
          even just install to host.

          different planform have different api.
          if traefik need auto config for you. 
              must set the right providers. 



 

🔵 Network ✅

    by default  all docker is quarented.
    one docker is not able to connect other docker.

    if want app use traefik auto discover mode.
    we must make sure app can visit traefik first.

    so create a docker network: dk-net-traefik
    put all app who need use traefik under same docker network:  dk-net-traefik


    🔶 note 

        one machine one docker-compose file
            all docker under docker-compose-file use same network by default.

        one machine two+ docker-compose file
            you need create network. 
            and join to same network.

        many machine many docker-compose file 
            create cluster first:  like  k8s/swarm
            in cluster. create a docker network.
            join app & traefik to same docker network.




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  config 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  config 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  config 

  

1. provider   ✅ 

    different physical router have differnet cmd.
    traefik(software) can run on all hardware.  
    you just need set which hardware/provider you run traefik on .
    . file/ docker / k8s /swarm 


2. config: 

    static / dynamic config 


3. entrypoint & services 

    entrypoint:  frontend   ➜ data in traefik   ➜ bind host ip & port.
    services:    backend    ➜ data out traefik  ➜ send data to which server.


🔶 router & rule

    hardware router:  what ip  send to what enterface.
    traefik  router:  what url send to what server.

        traefik  router       ➜ like ip
        traefik  router.rule  ➜ like url ➜ give ip a name. easy to remember.

            ‼️ traefik router(ip),  traefik rule(url) ‼️
            ‼️ traefik router(ip),  traefik rule(url) ‼️
            ‼️ traefik router(ip),  traefik rule(url) ‼️







 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

demo file  good

https://github.com/htpcBeginner/docker-traefik/blob/master/docker-compose-t2.yml

 
 