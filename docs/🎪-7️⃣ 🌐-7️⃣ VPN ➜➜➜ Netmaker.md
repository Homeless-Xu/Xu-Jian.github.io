---
sidebar_position: 1719
title: 🎪-7️⃣🌐-7️⃣ VPN ➜➜➜ Netmaker
---


# VPN ✶ Netmaker



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Netmaker  demo

🔵 netmaker Server require 

    Ubuntu 20.  best .
    public ip. 
    domain.


🔵 Domain prepair 

    🔶 Set A record for:  dashboard & api & broker 

            dashboard.0214.icu 
            api.0214.icu     
            broker.0214.icu  

                no change name. must use  dashboard/api/broker + domain.


    🔶 Check DNS 

        VPS ~ nslookup dashboard.0214.icu 8.8.8.8 
        Server:         8.8.8.8
        Address:        8.8.8.8#53

        Non-authoritative answer:
        Name:   dashboard.0214.icu
        Address: 172.93.42.232




🔵 SRV  Install Depndenices

    sudo apt-get install -y docker.io docker-compose wireguard
        
        🔶 option
            sudo apt install containerd
                systemctl start docker
                systemctl enable docker



🔵 check port & firewall 

    make sure 443 not used

    sudo ufw allow proto tcp from any to any port 443 && sudo ufw allow 51821:51830/udp
    iptables --policy FORWARD ACCEPT



🔵 config docker-compose file 

    wget -O docker-compose.yml https://raw.githubusercontent.com/gravitl/netmaker/master/compose/docker-compose.traefik.yml
    sed -i 's/NETMAKER_BASE_DOMAIN/<your base domain>/g' docker-compose.yml
    sed -i 's/SERVER_PUBLIC_IP/<your server ip>/g' docker-compose.yml
    sed -i 's/YOUR_EMAIL/<your email>/g' docker-compose.yml


    sed -i 's/NETMAKER_BASE_DOMAIN/0214.icu/g' docker-compose.yml
    sed -i 's/SERVER_PUBLIC_IP/172.93.42.232/g' docker-compose.yml
    sed -i 's/YOUR_EMAIL/xx2610@gmail.com/g' docker-compose.yml


     🔶 get NETMAKER_BASE_DOMAIN
            
            in compose file 
            BACKEND_URL: "https://api.NETMAKER_BASE_DOMAIN"
                so NETMAKER_BASE_DOMAIN should be your domain.  no head.
                    0214.icu

        🔶 get public ip

            ip route get 1 | sed -n 's/^.*src \([0-9.]*\) .*$/\1/p'




🔶 generate key 

    VPS ~ tr -dc A-Za-z0-9 </dev/urandom | head -c 30 ; echo ''
    XJy7GUhBQwJNVphiVl2gqiGio3x3f4

    sed -i 's/REPLACE_MASTER_KEY/KcZNVf46rEJvqecy1Vqpb5BZFaUlxU/g' docker-compose.yml



🔶 Prepare MQ

    No need any config. jsut run 

    wget -O /root/mosquitto.conf https://raw.githubusercontent.com/gravitl/netmaker/master/docker/mosquitto.conf



🔶 start netmaker 

    sudo docker-compose up -d

    ◎ check log:          docker logs netmaker


🔶 login 

    https://dashboard.0214.icu/login




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 USE Netmaker 

🔵 Srv:   Create network 

    ip:  10.214.214.0/24 
        is local network  ➜ no .  (for big company. use lan only.)
        udp hole punching ➜ if allow wireguard use udp vpn.



🔵 Client:  install netclient 

    ‼️ offical Manyal   https://docs.netmaker.org/netclient.html  ‼️
    ‼️ offical Manyal   https://docs.netmaker.org/netclient.html  ‼️
    ‼️ offical Manyal   https://docs.netmaker.org/netclient.html  ‼️


    🔶 Node - Ubuntu 

        curl -sL 'https://apt.netmaker.org/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/netclient.asc
        curl -sL 'https://apt.netmaker.org/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/netclient.list
        sudo apt update
        sudo apt install netclient -y



    🔶 Node - mac 

        brew install netclient




🔵 Srv:  get token & set client nodes.

    server will general client cmd for you .
    just copy and past to client node.
    (client need install netclient first)


    docker run -d --network host  --privileged -e TOKEN=eyJhcGljb25uc3RyaW5nIjoiYXBpLjAyMTQuaWN1OjQ0MyIsIm5ldHdvcmsiOiIwMjE0Iiwia2V5IjoiNDUwY2Q2MjQzMmVlNzllOSIsImxvY2FscmFuZ2UiOiIifQ== -v /etc/netclient:/etc/netclient --name netclient gravitl/netclient:v0.14.3

    ‼️ if errot           sudo service docker restart ‼️










🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Traefik add router  ✅

    vps-docker-netmaker:  can ping vpn(10.214.214.0/24) & lan inside homelab(172.16.1.0/24)
    vps-docker-traefik:   No  ping vpn(10.214.214.0/24) & lan inside homelab(172.16.1.0/24)

        but vps-docker-traefik  can connect vps-docker-netmaker. (use same docker`s network )
            add staric route to vps-docker-traefik. so vps-docker-traefik can visit vpn & lan in homelab.
                route add -net 10.214.214.0 netmask 255.255.255.0 gw netmaker
                route add -net 172.16.1.0 netmask 255.255.255.0 gw netmaker
                route add -net 10.1.1.0 netmask 255.255.255.0 gw netmaker
                    ◎ netmaker is netmaker`s container name.
                    ◎ cotainer in same docekr netwrok. can ping each other use contain name.








🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   vps host network config 

🔵 goal 

    if use docker to build netmaker in vps. 
    vps it self can not visit valn in homelab. or vpn network.

    to fix this.
        netmaker use vps`s eth0 by default. 
        add another ip to vps`s eth0 card.

        then set vps. as egress node. 
        let vlan in homelab visit new ip in host. 
    
            ‼️ no create new virtual nic in vps. no work. tried ‼️
                netmaker only use one nic. 
                    you can enter netmaker docker. 
                        use route to check which nic it use. 



🔵 add another ip to nic 

    vi /etc/network/interfaces

    iface eth0 inet static
    address 172.16.254.254/32

    sudo systemctl restart networking



🔵 cofig netmaker 

    set vps . host as  exgress gateway. 

    egress gateway range:  172.16.254.254/32
    egress nic :     eth0


🔵 ether vps docker.netmaker 

    bash-5.1# route
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    default         vps.local       0.0.0.0         UG    0      0        0 eth0
    10.1.1.0        *               255.255.255.0   U     0      0        0 nm-0214
    10.214.214.0    *               255.255.255.0   U     0      0        0 nm-0214
    172.16.1.0      *               255.255.255.0   U     0      0        0 nm-0214
    172.16.254.254  *               255.255.255.255 UH    0      0        0 nm-0214
    172.19.0.0      *               255.255.0.0     U     0      0        0 eth0

    ‼️ fuck no work. .. ..

    add static router to docker.netmaker.  fuck ..



            route add -net 10.214.214.0 netmask 255.255.255.0 gw netmaker
                route add -net 172.16.1.0 netmask 255.255.255.0 gw netmaker

    route add -net 172.16.254.254 netmask 255.255.255.255 gw netmaker
    route add -net 172.16.254.254 netmask 255.255.255.255 device 

    /sbin/route add -net <IP>/<MASK> <GW> dev <ethX>



    The configuration file for this is /etc/network/routes



.. 

create network for docekr.
egress this network ????



..
自己创建 overlay 网络 .
不用swarm 也能用 啊.  fuck.  除了 nsx 还有啥overlay ???


overlay 主机.  必须额外一个网卡啊?


use k8s  -.-

how add vps to k8s.

vps. run cmd.  use vlan...  doen ????





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 

🔵 



🔵  test on other vpn node. 




    1 packets transmitted, 1 packets received, 0% packet loss
    round-trip min/avg/max = 1.191/1.191/1.191 ms
    bash-5.1# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
        valid_lft forever preferred_lft forever
    3: nm-0214: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1280 qdisc noqueue state UNKNOWN group default qlen 1000
        link/none 
        inet 10.214.214.254/24 scope global nm-0214
        valid_lft forever preferred_lft forever
        inet 10.214.214.254/32 scope global nm-0214
        valid_lft forever preferred_lft forever
    26: eth0@if27: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
        link/ether 02:42:ac:13:00:05 brd ff:ff:ff:ff:ff:ff link-netnsid 0
        inet 172.19.0.5/16 brd 172.19.255.255 scope global eth0
        valid_lft forever preferred_lft forever
        inet6 fe80::42:acff:fe13:5/64 scope link 
        valid_lft forever preferred_lft forever




    bash-5.1# route
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    default         vps.local       0.0.0.0         UG    0      0        0 eth0
    10.1.1.0        *               255.255.255.0   U     0      0        0 nm-0214
    10.214.214.0    *               255.255.255.0   U     0      0        0 nm-0214
    172.16.1.0      *               255.255.255.0   U     0      0        0 nm-0214
    172.16.254.254  *               255.255.255.255 UH    0      0        0 nm-0214
    172.19.0.0      *               255.255.0.0     U     0      0        0 eth0




🔵 give  etho another ip!!! 



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  install netmaker in host .❌❌❌❌
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  install netmaker in host .❌❌❌❌
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  install netmaker in host .❌❌❌❌

    https://netmaker.readthedocs.io/en/master/server-installation.html#nodocker



🔵 why no docker 

    docker have lots cons. 
    docker can not allow host itself have vpn! 
    so that host can do nothing but run netmaker server.

    no docker is the best choose. 
    but.. very difficulty.. fuck.... 



🔵 dns prepair 

    same as docker install.


🔵 install netmaker 

    🔶 get download link .

        go here
        https://github.com/gravitl/netmaker/releases
        get latest link.
        https://github.com/gravitl/netmaker/releases/download/v0.14.4/netmaker


    🔶 download install sh 

        wget https://raw.githubusercontent.com/gravitl/netmaker/master/scripts/netmaker-server.sh

        vi 

        change download link to new link 
        wget -O /etc/netmaker/netmaker https://github.com/gravitl/netmaker/releases/download/latest/netmaker
        wget -O /etc/netmaker/netmaker https://github.com/gravitl/netmaker/releases/download/v0.14.4/netmaker


    🔶 install 



🔵 change masterkey  

    🔶 generate key 

        tr -dc A-Za-z0-9 </dev/urandom | head -c 30 ; echo ''
        XJy7GUhBQwJNVphiVl2gqiGio3x3f4

            just a random key. any machine can do.


    🔶 config 

        cd /etc/netmaker/config/environments/
        masterkey: "secretkey"  ➜  masterkey: "xxxxxx"


    🔶 restart 

        sudo systemctl restart netmaker

    🔶 check status 
        sudo journalctl -u netmaker

    now. netmaker server is done.
    next is config dashboard/ui for it.



🔵 netmaker-ui 

    install nginx 
    put netmaker-ui file into nginx.
    config nginx 

    use proxy like traefik to visit netmaker-ui from internet



🔶 install nginx  

    apt install nginx
    apt install unzip 


🔶 move default nginx config ???

    sudo cp /usr/share/nginx/html/nginx.conf /etc/nginx/conf.d/default.conf


🔶 get netmaker-ui download link 

    https://github.com/gravitl/netmaker-ui/releases/tag/v0.14.4
    https://github.com/gravitl/netmaker-ui/releases/download/v0.14.4/Netmaker-Ui-v0.14.4.zip


🔶 change new link 

    sudo wget -O /usr/share/nginx/html/netmaker-ui.zip https://github.com/gravitl/netmaker-ui/releases/download/v0.14.4/Netmaker-Ui-v0.14.4.zip

🔶 unzip 

    sudo unzip /usr/share/nginx/html/netmaker-ui.zip -d /usr/share/nginx/html

🔶 jsut run 

    sudo sed -i 's/root \/var\/www\/html/root \/usr\/share\/nginx\/html/g' /etc/nginx/sites-available/default


🔵 create config.js 

🔶 download config generate js 

    cd /usr/share/nginx/html
    wget https://raw.githubusercontent.com/gravitl/netmaker-ui/master/generate_config_js.sh
    chmod 655 generate_config_js.sh


🔶 us js create config 

    sudo sh -c 'BACKEND_URL=https://api.0214.icu /usr/share/nginx/html/generate_config_js.sh >/usr/share/nginx/html/config.js'

    ‼️ change me.  use your own     BACKEND_URL: "https://api.0214.icu"
    sudo sh -c 'BACKEND_URL=http://<YOUR BACKEND API URL>:PORT /usr/share/nginx/html/generate_config_js.sh >/usr/share/nginx/html/config.js'


🔶 check config. 

    cat config.js 
    window.REACT_APP_BACKEND='https://api.0214.icu';

🔶 restart nginx 

    sudo systemctl start nginx



🔵 what this do 

    this do nothing? 
    no web available now...


    -.-  why fuck do it. 
    tool should make life easy .







