---
sidebar_position: 1703
title: 🎪-7️⃣🌐-3️⃣ DNS ➜ AdGuard
---


# DNS ✶ AdGuard



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 NET DNS:  AdGuard Home 


🔵 Desc 

    adguard  = pihole(remove ad) + smartnds(choose fatest dns)

    https://icloudnative.io/posts/adguard-home/


🔵 Adgurad vs pihole 

    https://github.com/AdguardTeam/AdGuardHome


🔵 Docker 

    https://hub.docker.com/r/adguard/adguardhome

    🔶 use host mode if possible.


    docker run --name adguardhome\
        --restart unless-stopped\
        -v /my/own/workdir:/opt/adguardhome/work\
        -v /my/own/confdir:/opt/adguardhome/conf\
        -d adguard/adguardhome



🔵 add  upstream dns server.

    https://p3terx.com/archives/use-adguard-home-to-build-dns-to-prevent-pollution-and-remove-ads-2.html



🔵 dns filter.

    anti-AD      https://anti-ad.net/easylist.txt
    
    halflife      https://cdn.jsdelivr.net/gh/o0HalfLife0o/list@master/ad.txt


    https://anti-ad.net/easylist.txt





🔶 free dns 

    Google	8.8.8.8	8.8.4.4
    Cloudflare	1.1.1.1	1.0.0.1
    Quad9	9.9.9.9	149.112.112.112
    OpenDNS Home	208.67.222.222	208.67.220.220



    server 8.8.8.8
    server 8.8.4.4
    server 1.1.1.1
    server 1.0.0.1
    server 9.9.9.9
    server 149.112.112.112
    server 208.67.222.222
    server 208.67.220.220

    server-tcp 8.8.8.8
    server-tcp 8.8.4.4
    server-tcp 1.1.1.1
    server-tcp 1.0.0.1
    server-tcp 9.9.9.9
    server-tcp 149.112.112.112
    server-tcp 208.67.222.222
    server-tcp 208.67.220.220

    server-tls 8.8.8.8
    server-tls 8.8.4.4
    server-tls 1.1.1.1
    server-tls 1.0.0.1
    server-tls 9.9.9.9
    server-tls 149.112.112.112
    server-tls 208.67.222.222
    server-tls 208.67.220.220






🔵 router dhcp set

    main dns:  172.16.8.2
    sec        172.16.8.4
    3          1.1.1.1
    4.         8.8.8.8




🔵 result test .

    dig can show how long dns request takes.

    compare. yourself

    dig @1.1.1.1 xclient.info
    dig @172.16.8.2 xclient.info

    


 🔶 server test 

  nslookup -q=a 0214.icu 172.16.8.4 ➜ use 172.16.8.4 as dns server 
  nslookup -q=a 0214.icu 8.8.8.8    ➜ use 8.8.8.8    as dns server 

    U.2204 ~ nslookup -q=a 0214.icu 172.16.8.4
    Server:		172.16.8.4
    Address:	172.16.8.4#53

    Non-authoritative answer:
    Name:	0214.icu
    Address: 172.93.42.232


 🔵 Ubuntu buildin systemd-resolved Setup ✅

🔶 Check 53 status 

  sudo lsof -i:53
    COMMAND   PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    systemd-r 858 systemd-resolve   13u  IPv4  22529      0t0  UDP localhost:domain
    systemd-r 858 systemd-resolve   14u  IPv4  22530      0t0  TCP localhost:domain (LISTEN)

      53 is used by default in ubunbtu.
      if you need use smartdns/ this need use 53 too.
      so have to free the 53 first.


🔶 Change resolved.conf

   vi /etc/systemd/resolved.conf
     edit line #DNSStubListener=yes
         to be DNSStubListener=no


    this will free the 53 port.
    this is system buildin function .
    ‼️ do not disable it. just let it not use 53.‼️
    ‼️ do not disable it. just let it not use 53.‼️
    ‼️ do not disable it. just let it not use 53.‼️
    otherwise network will go wrong.
     
 

 🔵 Desc 

  smartdns:  get the fastest dns for you .
  pi-hole :  remove ads.

  smartdns IP :  172.16.8.4 
  pi-hole  IP :   172.16.8.2

  pc       dns server:  172.16.8.2
  pi-hole  dns server:  172.16.8.4
  smartdns dns server:  1.1.1.1 ... 



🔵 Demo 

  pc (pihole ip )
    >>  pihole (smartdns ip )
      >> internet 
  
 