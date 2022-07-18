---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️1️⃣ Helm ➜ Basic
---

# Helm Basic


    https://www.youtube.com/watch?v=6mHgb3cDOjU&list=PLmOn9nNkQxJHYUm2zkuf9_7XJJT8kzAph&index=45&ab_channel=%E5%B0%9A%E7%A1%85%E8%B0%B7IT%E5%9F%B9%E8%AE%AD%E5%AD%A6%E6%A0%A1


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Hlem Install 

🔵 K8s tool

    🔶 K8s APP-GUI  ➜ lens  ➜ manager local+remote cluster.  ➜ best 
    🔶 K8s CMD-GUI  ➜ k9s   ➜ manager local cluster.
    🔶 K8s package  ➜ helm  ➜ like apt.  make k8s deploy pod very easy.


        🔻 lens enable prometheus ✅

            lens have builded in promethes
            but you need able it for all cluster.

            cluster >> setting >> lens metric >> enable all >> restart app 


🔵 Helm Desc 

    📗 manual   https://helm.sh/zh/docs/topics/charts/
    📗 manual   https://helm.sh/zh/docs/topics/charts/
    📗 manual   https://helm.sh/zh/docs/topics/charts/


    helm prepair almost all for you. 
        but helm is temple! 
        temple means you have to change some config before use it .

        🔥 helm-V3 is for local cluster. not for remote manage.
            - k8s manager node:      install helm 
            - k3s maneger node:      install helm 
            - minikube    node:      install helm 


🔵 Helm basic 

    helm    ➜ mamage app 
    chart   ➜ app         (exe dmg )
    repo    ➜ app storage (where to doanload app) 
    release ➜ app vision  (like git version )


🔵 Helm Use

    install helm to cluster maneger node.

    add helm app repo  then install helm app

    config app & run 


🔵 Hlem install

    🔶 mac 

        brew install helm
        helm repo add bitnami https://charts.bitnami.com/bitnami
        helm repo update


    🔶 Ubuntu - apt 

        curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
        sudo apt-get install apt-transport-https --yes
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
        sudo apt-get update
        sudo apt-get install helm


🔵 Helm search app 

    🔶 use cmd    👎

        helm search hub  xxx   ➜  search in allavailable    repo 
        helm search repo xxx   ➜  search in installed ready repo 


    🔶 use website 👍

        https://artifacthub.io/packages/search?kind=0&sort=relevance&page=1
            website. tell you how install & config 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Helm app Strecture 

🔵 app importent files ✅

    chart.yaml        ➜  app info:          ➜ app name / version / url / maintainer email 
    requirements.yaml ➜  app depends:       ➜ if need mysql etc. 
    value.yaml        ➜  app default value  ➜ docker port / username / 


🔵 global value  

    app is dependet. like docker. 
    but some time you need other app can visit something in out app.
    so you can use global value for that you need share out.









🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵


🔵 available vaule - check how ✅

    app only allow you change some value. 
    depends on how the helm app is created.

    every app is different available value. 
    you have to check in helm app`s website.


        🔻 use web 👍 
            https://artifacthub.io/packages/helm/bitnami/nginx
            https://artifacthub.io/packages/helm/bitnami/nginx?modal=values

                you can check all info in web. no need cmd. 


        🔻 use cmd 👎
            -- use cmd must add repo first. then check 

            helm repo add bitnami https://charts.bitnami.com/bitnami
            helm show values bitnami/nginx

            helm repo add bitnami https://charts.bitnami.com/bitnami
            helm show values bitnami/mariadb



🔵 available vaule - check demo 

    https://artifacthub.io/packages/helm/bitnami/nginx

        Traffic Exposure parameters
        service.ports.http	80



🔵 custom value 

    two way to set your custom value.   
        -f    ➜ use yaml file 
        --set ➜ use cmd 

    🔶 set demo 

        helm install my-nginx bitnami/nginx --set xxx=yyy
        helm install my-nginx bitnami/nginx --set service.ports.http=880
        helm install my-nginx bitnami/nginx --set service.ports.http=880,service.ports.https=8443


    🔶 yaml demo 

        helm install my-nginx bitnami/nginx -f xxx.yaml 
        helm install my-nginx bitnami/nginx -f config-nginx.yaml 


            cat <<EOF>  config-nginx.yaml
            service.ports.http: 880,
            service.ports.https: 8443,
            EOF

            http:172.16.1.33:880
            https:172.16.1.33:8443







