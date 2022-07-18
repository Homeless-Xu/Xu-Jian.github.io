---
sidebar_position: 1701
title: 🎪-7️⃣🌐-0️⃣ SSL ➜ Apply & Use
---


# SSL ✶ Apply & Use




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 SSL  Desc 

🔵 http vs https

    http:   no encryped         ➜  everyone know what you send to website
    https:  encryped with ssl   ➜  no one know what you send to website 

        if need use https. must have ssl first. 


🔵 SSL Desc 

    Client: chrome/firefox 
        >> Server: nginx/apache  <<=======>>  CA Center: WanCA/LanCA

    ssl can help you encrypted data. 
    but most import part is ssl help you verify server is true/fake 

    because a lot hack in internet.
    even you set https. it is still can be hacked.
    if you visit a https web. the lock/https color in chrome url is green. that website is ok 
    if you visit a https web. the lock/https color in chrome url is red.   that website no ok



🔵 WAN-CA Desc 

    how sll can verify a web is real(green) or fake(red)
    ssl need use CA to make decide.

    CA is theone who general SSL. 

        everyone can build CA!  it is just a file
            you can build ca for your homelab.
        
        when it come to internet. 
            some big company begian offer CA service to public.
            and chrome/firefox have set those company`s ca`s info to browser by default.

            so if website use those ca to generate ssl.
                when you visit their website. 
                    the https lock color will be green.


    wan-ca generate two file for website
        fullchain.pem  
        privatekey.pem

    website set that two file to nginx/apache
        ca can verity if the website`s pem is the right one.
            if right > client chrome https lock be green.
            if wrong > client chrome https lock be red.



🔵 WAN-SSL Workflow

    🔶 visit a https website:   https://vps.0214.icu
        check ca in chrome url
            *.0214.icu 
                issued by: R3  (big company)
                    in it have public key




    website use big company`s ca to generate ssl:  public + private key
    website config public+private key to nginx 
    client use chrome to visit nginx 
        nginx send back some info.
        chrome use info to verify it nginx is fake or true.



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL Demo ESXI & Desc
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL Demo ESXI & Desc
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL Demo ESXI & Desc


🔵 Why need Local SSL  

    esxi need https. or chrome deny it. 
    a lot more need ssl too.


🔵 SSL tooles 

    openssl / mkcert  a lot tool can general local ssl for you.
    mkcert is easiest.


🔵 MacOS install mkcert 

    brew install mkcert
    brew install nss # if you use Firefox

    🔶 Offical Manual 
        https://github.com/FiloSottile/mkcert



🔵 Create & install CA 

    iMAC Desktop mkcert -install
    The local CA is already installed in the system trust store! 👍
    The local CA is now installed in the Firefox trust store (requires browser restart)! 🦊

        ‼️ mkcert auto help you install lan-ca to macos & firefox. ‼️
        ‼️ mkcert auto help you install lan-ca to macos & firefox. ‼️
        ‼️ mkcert auto help you install lan-ca to macos & firefox. ‼️


    🔶 MacOS Check installed LAN-CA:  

        mac 
            >> keychain 
                >> system 
                    >> mkcert techark@TechArks-iMac.local (TechArk)
        


    🔶 Chrome  

        use system `s ca. 
            so if lan-ca install to macos.  chrome can use too. 

    
    🔶 Firefox 

        not use system`s ca.
            this is why if firefox need install nss



🔵 Local Domain prepair

    before use https.  
    you need config http first.  not visit esxi via ip! 
    in local router/dsn server. set static dns


    🔶 set Aname 

        g3.esxi.rv >> 10.219.219.13
        g5.exxi.rv >> 10.219.219.15

    🔶 ping check 

        iMAC Desktop ping g3.esxi.rv

    🔶 url check

        http://g3.esxi.rv/



🔵 Create SSL for *.esxi 

    mkcert "*.esxi.rv"


    Created a new certificate valid for the following names 📜
    - "*.esxi"
    Warning: many browsers don't support second-level wildcards like "*.esxi" ⚠️
    Reminder: X.509 wildcards only go one level deep, so this won't match a.b.esxi ℹ️
    The certificate is at "./_wildcard.esxi.pem" and the key at "./_wildcard.esxi-key.pem" ✅
    It will expire on 20 September 2024 🗓



🔵 ESXI Config SSL

    🔶 delete default ssl file 

        rm -f /etc/vmware/ssl/rui.crt
        rm -f /etc/vmware/ssl/rui.key

    🔶 upload & rename

        cd /etc/vmware/ssl

        _wildcard.esxi.pem       ➜ rename to rui.crt 
        _wildcard.esxi-key.pem   ➜ rename to rui.key

    🔶 reboot esxi 

    🔶 test:    https://g3.esxi.rv




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL ▪ Fortigate GUI ✅
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL ▪ Fortigate GUI ✅
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Lan-SSL ▪ Fortigate GUI ✅

🔵 desc 

    use https to login frotigate gui web.


🔵 How 

    fotigate >> system >> setting >> https server certicate 
        >> create 
            >> we use mkcert generate ssl.  so choose import 
                >> type:  certificate
                    cert file:  choose  xx.pem
                    key file :  choose  xx-key.pem 
                        >> create 

    fotigate >> system >> setting >> https server certicate 
        >> choose new created. 
            >> apply 


    set dns record:   fg.rv.ark >> 10.219.219.1

    https visit:      https://fg.rv.ark









🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 HomeLAB ✶ SSL.Demo ➜ Nginx-Docker 💯

🔶 Simplest Demo

    docker run -d -p 80:80 --rm --name nginx nginx
    web visit http://ip  

    if we need really use it. 
    we need setup more,


🔶 DNS Config

    one host/vps can run alot web.
    every web can have a domain.

        vps.0214.icu     to vps ip 
        nginx.0214.icu   to vps ip 



🔶 mkdir folder

    mkdir -p /root/DockerConfNoDEL/Nginx/log
    mkdir -p /root/DockerConfNoDEL/Nginx/conf
    mkdir -p /root/DockerConfNoDEL/Nginx/conf.d
    mkdir -p /root/DockerConfNoDEL/Nginx/static
    mkdir -p /root/DockerConfNoDEL/Nginx/ssl



🔵 Copy Default files 

🔶 Copy nginx.conf

    we not create new config.  
    we often copy a conf. and change some in it.

    docker cp nginx:/etc/nginx/nginx.conf /root/DockerConfNoDEL/Nginx/conf/nginx.conf
                |--> this is docker nage.  we run a nginx first. then copy config from it. 



🔶 Check nginx.conf

    ‼️ every docker have differnet config.  better check it ‼️

    cat /root/DockerConfNoDEL/Nginx/conf/nginx.conf
        ....
        include /etc/nginx/conf.d/*.conf;
        ....
    
    we found default.conf  include another folder:  conf.d/*.conf;
        because one nginx can run a lot web like website.
                1.0214.icu / 2.0214.icu / 3.0214.icu
            you can give every url a  different conf.
                and put then under  conf.d/*.conf;

    we enter docker found there is a file in conf.d too.
        so we need copy it out to host folder too.
            root@e32f3bb799fe:/etc/nginx/conf.d# ls
            default.conf
            root@e32f3bb799fe:/etc/nginx/conf.d# 



🔶 Copy default.conf from conf.d

    docker cp nginx:/etc/nginx/conf.d/default.conf /root/DockerConfNoDEL/Nginx/conf.d/default.conf


🔶 Copy index.html 

    docker cp nginx:/usr/share/nginx/html/index.html /root/DockerConfNoDEL/Nginx/static/index.html


🔶 out folder demo 

    VPS Nginx tree
    .
    ├── conf
    │   └── nginx.conf
    ├── conf.d
    │   └── default.conf
    ├── log
    ├── ssl
    └── static
        └── index.html


🔵 Run nginx 

🔶 check host file

    docker`s name can not same.
    so we stop & delete old nginx. 
    start a new one & mount folder/file to nginx.
    ‼️ some are file some are folder ‼️

    VPS Nginx pwd
    /root/DockerConfNoDEL/Nginx
    VPS Nginx tree
    .
    ├── conf
    │   └── nginx.conf
    ├── conf.d
    │   └── default.conf
    ├── log
    ├── ssl
    └── static
        └── index.html

    5 directories, 3 files



🔶 run 

docker run -p 443:443 -p 80:80 --name nginx \
	-v /root/DockerConfNoDEL/Nginx/static:/usr/share/nginx/html  \
	-v /root/DockerConfNoDEL/Nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
	-v /root/DockerConfNoDEL/Nginx/log:/var/log/nginx  \
	-v /root/DockerConfNoDEL/Nginx/conf.d:/etc/nginx/conf.d \
    -v /root/DockerConfNoDEL/Nginx/ssl:/ssl \
    -d nginx



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 SSL APPLY 

🔵 WHY

    now we copy all needed file from docker to local.
    when create new nginx docker.  just need mount local folder to nginx docker.
    if you delete nginx docker. the data is still on local.

    next is config nginx SSL.
    nginx default suppprt http.
    for real use at least config https and setup ssl.
    let`s apply ssl first. then setup ssl to nginx.



🔵 SSL Verify 

    https://eff-certbot.readthedocs.io/en/stable/using.html#manual


    if you wnt appley 0214.icu a ssl.
    you must prove 0214.icu is your!!!! 

    you have two option to prove you can control the domain:  
        http challge: ask you put some file to your web server
        dns challge : need you login domain.and put some txt nds record to dmain.



🔶 DNS 

    vps.0214.icu >> 172.93.42.232


🔶 Apply tool

    apply ssl need install a tool: certbot

        if you use nginx  or apache ....   they make apply much easy.
            sudo certbot --nginx
            sudo certbot --apache

    we install nginx in docker... 
        so we apply ssl manyal! 
        https://certbot.eff.org/instructions?ws=other&os=ubuntufocal



🔶 Instal certbot (ubuntu 20)

    sudo snap install core; sudo snap refresh core
    sudo snap install --classic certbot
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    sudo snap set certbot trust-plugin-with-root=ok




🔵 SSL Apply: manual & dns-challenge

🔶 apply 

    certbot certonly  -d "*.0214.icu" --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory 

    ...
    Please deploy a DNS TXT record under the name:
    _acme-challenge.0214.icu.
    with the following value:
    I-sb3kevItDpJOUgoLRBQiJtErDDb1bZoqLqLHfsRrw
    ...


    certbot certonly  -d "*.0214.icu" --rsa-key-size 4096 --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory 

e. g. certbot --rsa-key-size 4096 -(other-arguments)`






🔶 verify domain 

    login web where you buy domain.
    add above record and come back to apply 

        cloudflare >> dns >> add record

            type:     txt 
            name:     _acme-challenge.0214.icu.
            content:  I-sb3kevItDpJOUgoLRBQiJtErDDb1bZoqLqLHfsRrw


🔶 contine apply 

    Successfully received certificate.
    Certificate is saved at: /etc/letsencrypt/live/0214.icu/fullchain.pem
    Key is saved at:         /etc/letsencrypt/live/0214.icu/privkey.pem
    This certificate expires on 2022-09-19.
    These files will be updated when the certificate renews.

    NEXT STEPS:
    - This certificate will not be renewed automatically. Autorenewal of --manual certificates requires the use of an authentication hook script (--manual-auth-hook) but one was not provided. To renew this certificate, repeat this same certbot command before the certificate's expiry date.


 


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Nginx Config SSL 

🔵 Copy SSL 

    cp /etc/letsencrypt/live/0214.icu/fullchain.pem /root/DockerConfNoDEL/Nginx/ssl
    cp /etc/letsencrypt/live/0214.icu/privkey.pem /root/DockerConfNoDEL/Nginx/ssl


    -v /root/DockerConfNoDEL/Nginx/ssl:/ssl \


🔵 edit conf.d/default.conf

    ‼️ not conf/nginx.conf  leave that no change.   ‼️  

    
    cat <<EOF >> /root/DockerConfNoDEL/Nginx/conf.d/default.conf
    server {
      listen 80;
      server_name vps.0214.icu 0214.icu;
    
      # Redirect all traffic to SSL
      rewrite ^ https://$server_name$request_uri? permanent;
    }
    
    server {
      listen 443 ssl default_server;
    
    # ‼️ must 1.2+  or firefox no support.
      ssl_protocols TLSv1.2 TLSv1.3; 
    
      # disables all weak ciphers
      ssl_ciphers ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM;
    
      server_name vps.0214.icu 0214.icu;
    
      ## Access and error logs.
      access_log /var/log/nginx/access.log;
      error_log  /var/log/nginx/error.log info;
    
      ## Keep alive timeout set to a greater value for SSL/TLS.
      keepalive_timeout 75 75;
    
      ## See the keepalive_timeout directive in nginx.conf.
      ## Server certificate and key.
      ssl_certificate /ssl/fullchain.pem;
      ssl_certificate_key /ssl/privkey.pem;
      ssl_session_timeout  5m;
    
      ## Strict Transport Security header for enhanced security. See
      ## http://www.chromium.org/sts. I've set it to 2 hours; set it to
      ## whichever age you want.
      add_header Strict-Transport-Security "max-age=7200";
      
      root /usr/share/nginx/html;
      index index.html;
    }
    EOF



