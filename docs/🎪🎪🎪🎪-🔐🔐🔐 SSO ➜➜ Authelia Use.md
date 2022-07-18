---
sidebar_position: 4930
title: 🎪🎪🎪🎪-🔐🔐🔐 SSO ➜➜ Authelia USE
---

# SSO Authelia USE




🔵 goal

  authelia + openldap ✅

  authelia + treafik 





🔵 
1. 

0214.icu >>  vps traefik (ssl)
    vpn: openldap + authelia 

    if need traefik  combind authelia.. need in same docker network.????
    we can use public port..


or add a local traefik .




0214.icu >>  vps traefik (ssl)   <===>  VPN. traefik local. 
    lan:  traefik +  openldap + authelia 


. use swarn.  or...k8s.  or ...




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 How
use traefik middle ware.











🔵 traefik + authelia 


https://github.com/authelia/authelia/blob/master/examples/compose/lite/docker-compose.yml




https://wbsu2003.4everland.app/2022/03/09/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E6%9C%8D%E5%8A%A1Authelia%EF%BC%88%E4%B8%8B%E7%AF%87%EF%BC%89/





Outline使用Authelia实现本地认证


https://wbsu2003.4everland.app/2022/03/11/Outline%E4%BD%BF%E7%94%A8Authelia%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E8%AE%A4%E8%AF%81/









https://www.youtube.com/watch?v=yHICIVhm_Lo&t=621s&ab_channel=Sagit





















    services:
      authelia:
        image: authelia/authelia
        container_name: authelia
        volumes:
          - ./authelia:/config
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.authelia.rule=Host(`authelia.example.com`)'
          - 'traefik.http.routers.authelia.entrypoints=https'
          - 'traefik.http.routers.authelia.tls=true'
          - 'traefik.http.routers.authelia.tls.certresolver=letsencrypt'
          - 'traefik.http.middlewares.authelia.forwardauth.address=http://authelia:9091/api/verify?rd=https://authelia.example.com'  # yamllint disable-line rule:line-length
          - 'traefik.http.middlewares.authelia.forwardauth.trustForwardHeader=true'
          - 'traefik.http.middlewares.authelia.forwardauth.authResponseHeaders=Remote-User,Remote-Groups,Remote-Name,Remote-Email'  # yamllint disable-line rule:line-length
        expose:
          - 9091
        restart: unless-stopped
        healthcheck:
          disable: true
        environment:
          - TZ=Australia/Melbourne

      redis:
        image: redis:alpine
        container_name: redis
        volumes:
          - ./redis:/data
        networks:
          - net
        expose:
          - 6379
        restart: unless-stopped
        environment:
          - TZ=Australia/Melbourne

      traefik:
        image: traefik:v2.8
        container_name: traefik
        volumes:
          - ./traefik:/etc/traefik
          - /var/run/docker.sock:/var/run/docker.sock
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.api.rule=Host(`traefik.example.com`)'
          - 'traefik.http.routers.api.entrypoints=https'
          - 'traefik.http.routers.api.service=api@internal'
          - 'traefik.http.routers.api.tls=true'
          - 'traefik.http.routers.api.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.api.middlewares=authelia@docker'
        ports:
          - 80:80
          - 443:443
        command:
          - '--api'
          - '--providers.docker=true'
          - '--providers.docker.exposedByDefault=false'
          - '--entrypoints.http=true'
          - '--entrypoints.http.address=:80'
          - '--entrypoints.http.http.redirections.entrypoint.to=https'
          - '--entrypoints.http.http.redirections.entrypoint.scheme=https'
          - '--entrypoints.https=true'
          - '--entrypoints.https.address=:443'
          - '--certificatesResolvers.letsencrypt.acme.email=your-email@your-domain.com'
          - '--certificatesResolvers.letsencrypt.acme.storage=/etc/traefik/acme.json'
          - '--certificatesResolvers.letsencrypt.acme.httpChallenge.entryPoint=http'
          - '--log=true'
          - '--log.level=DEBUG'

      secure:
        image: traefik/whoami
        container_name: secure
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.secure.rule=Host(`secure.example.com`)'
          - 'traefik.http.routers.secure.entrypoints=https'
          - 'traefik.http.routers.secure.tls=true'
          - 'traefik.http.routers.secure.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.secure.middlewares=authelia@docker'
        expose:
          - 80
        restart: unless-stopped

      public:
        image: traefik/whoami
        container_name: public
        networks:
          - net
        labels:
          - 'traefik.enable=true'
          - 'traefik.http.routers.public.rule=Host(`public.example.com`)'
          - 'traefik.http.routers.public.entrypoints=https'
          - 'traefik.http.routers.public.tls=true'
          - 'traefik.http.routers.public.tls.certresolver=letsencrypt'
          - 'traefik.http.routers.public.middlewares=authelia@docker'
        expose:
          - 80
        restart: unless-stop




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 config swarn first'

vps +  local doker. 




🔵 traefik + authelia 


https://github.com/authelia/authelia/blob/master/examples/compose/lite/docker-compose.yml




https://wbsu2003.4everland.app/2022/03/09/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95%E6%9C%8D%E5%8A%A1Authelia%EF%BC%88%E4%B8%8B%E7%AF%87%EF%BC%89/



有了 Authelia，再配合 Fail2ban 防止暴力破解，公网访问的安全性问题会得到很大的保障。

虽然官方强调 OpenID Connect 仍处于预览阶段，但实际上Authelia 已经支持 OIDC 认证，不过限于篇幅，还是留到下回吧。

下期预告👉『Outline使用Authelia实现本地认证』，文章将讨论如何实现 Outline 通过 Authelia 的 OIDC 完成本地认证，而不再需要借助基于公网的第三方认证



Outline使用Authelia实现本地认证


https://wbsu2003.4everland.app/2022/03/11/Outline%E4%BD%BF%E7%94%A8Authelia%E5%AE%9E%E7%8E%B0%E6%9C%AC%E5%9C%B0%E8%AE%A4%E8%AF%81/









https://www.youtube.com/watch?v=yHICIVhm_Lo&t=621s&ab_channel=Sagit