---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️1️⃣ Helm ➜ Demo
---

# Helm Demo



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 ❌❌❌❌❌❌❌❌

🔵 Helm Demo ➜ Dashy  

    https://artifacthub.io/packages/helm/krzwiatrzyk/dashy


    helm repo add krzwiatrzyk https://krzwiatrzyk.github.io/charts/
    helm install dashy-v1 krzwiatrzyk/dashy --version 0.0.3
    helm status dashy-v1

        🔥 if use helm in k3s           export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
        🔥 if use helm in k3s           export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
        🔥 if use helm in k3s           export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

    here our dashy not start. 
        some app like nginx no   need config         and  start success
            ➜ use default config

        some app like dashy must need config.        or   start fail 
            so how to config app like dashy?
                every app diffs. -.-  check app`s config manual 


    🔶 check manual 

        https://artifacthub.io/packages/helm/krzwiatrzyk/dashy

        helm upgrade --install --set-file configMap.config.data."config\.yml"=config.yml
        helm upgrade --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yml

            --set-file configMap.config.data."config\.yml"=xxxx.yml 
                ➜ left  part means this app neea a config file named: config.yml. 
                ➜ xxxx.yml means you can use your own name to overwrite the default filename. 

            so we create a compigmap file. named: cm-dashy-config.yml
                then use belw cmd to import cm-dashy-config.yml as app`s config file
                    helm upgrade --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yml

❌ ❌❌❌❌❌❌

        kubectl create configmap cm-dashy-config.yaml --from-file=/root/dashy.conf
        kubectl create configmap config.yaml --from-file=/root/dashy.conf


        helm          install dashy-v1 krzwiatrzyk/dashy --version 0.0.3

        helm upgrade --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yml
        helm upgrade --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yml
        helm upgrade dashy-v1 krzwiatrzyk/dashy --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yaml


    sh.helm.release.v1.dashy-v1.v1




    🔶 create configmap 


    🔥 it is volume.  not file  fuck 

    MountVolume.SetUp failed for volume "config" : configmap references non-existent config key: conf.yml

    helm  mount volume.. 





    configmap:
    config:
        enabled: true
        data:
        conf.yml: |
            {{- .Files.Get "conf.yml" | nindent 4 }}

    # -- Probe configuration
    # -- [[ref]](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
    # @default -- See below


    persistence:
    # -- Default persistence for configuration files.
    # @default -- See below
    config:
        # -- Enables or disables the persistence item
        enabled: true

        # -- Sets the persistence type
        # Valid options are pvc, emptyDir, hostPath, secret, configMap or custom
        type: configMap

        # -- Where to mount the volume in the main container.
        # Defaults to `/<name_of_the_volume>`,
        # setting to '-' creates the volume but disables the volumeMount.
        mountPath:  /app/public/conf.yml

        # -- Used in conjunction with `existingClaim`. Specifies a sub-path inside the referenced volume instead of its root
        subPath:  conf.yml
        name: dashy-config
        items:
        - key: conf.yml
        path: conf.yml





🔵 volume 
    
    Unable to attach or mount volumes: unmounted volumes=[config], unattached volumes=[config]: timed out waiting for the condition




    persistence:
        config:
        accessMode: ReadWriteOnce
        enabled: false
        readOnly: false
        retain: false
        size: 1Gi
        type: pvc
        shared:
        enabled: false
        mountPath: /shared
        type: emptyDir


        -.-


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 

🔵 helm available valuse  

    helm show values chart-name.  ➜ check what value can set .
    helm show values krzwiatrzyk/dashy




🔵 Helm APP Custom 

    almost all app need some config. 
        - set value. 
        - mount config file 
        - expose port

    in helm. need two part.
        -f xxx.yaml ➜ use   custom value  file ➜ change app`s defaule setting 
        --set-file  ➜ mount custom config file ➜ like nginx.conf

            helm install --dry-run --debug \
            stable/rabbitmq \
            --name testrmq \
            --namespace rmq \
            -f rabbit-values.yaml \
            --set-file rabbitmq.advancedConfig=rabbitmq.config
        
    we manybe no need change app`s default value.
    but most time need. mount custom config.



🔵 Helm Custom ➜ config 

    🔶 check default value 

        we want change an helm app`s config.
        need konw app`s default setting first.
        this can check in website 
            https://artifacthub.io/packages/helm/krzwiatrzyk/dashy
            https://artifacthub.io/packages/helm/krzwiatrzyk/dashy?modal=values


    🔶 analy default value 

        configmap:
            config:
                enabled: true
                data:
                    conf.yml: |
                        {{- .Files.Get "conf.yml" | nindent 4 }}

        helm upgrade --install --set-file configMap.config.data."config\.yml"=config.yml

        it need a custom file: named :  config.yml   prepair first.
        but i want change the filename:   cm-dashy.yaml 
            so i need change default value first. 
            how overwrite heml `s defalt value.


🔵 Helm overwrite default value

    the app`s default custom filename is  config.yml 
    we want change it to cm-appname-config.yml 

    in helm we can use --set flag to overwrite a value.

    helm upgrade --install <service>-green -f <service>-values.yaml <service>-100.0.112+xxxx.tgz --set <service>.deployment.strategy=blue-green
    helm upgrade --install <service>       -f <service>-values.yaml <service>-100.0.112+xxxx.tgz --set <service>.image.tag=9.0.1xx.xx.xx


    helm upgrade --install --set-file configMap.config.data."config\.yml"=config.yml
    helm upgrade --install --set-file configMap.config.data."config\.yml"=cm-dashy-config.yml





        we need use 


    helm install --dry-run --debug \
        stable/rabbitmq \
        --name testrmq \
        --namespace rmq \
        -f rabbit-values.yaml \
        --set-file rabbitmq.advancedConfig=rabbitmq.config



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 helm demo nginx 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 helm demo nginx 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 helm demo nginx 


🔵 Mount Config 

    in docker we use volumes when start docker
        volumes:
            - ./license.dat:/etc/sys0/license.dat
            - ./config.json:/etc/sys0/config.json


    in k8s/helm same 

        helm install my-nginx                            bitnami/nginx
        helm install my-nginx -f values.yaml             bitnami/nginx
        helm install my-nginx --set ingress.enabled=true bitnami/nginx


    or better change config use values.yaml.
    and run app use your own value.yaml.
    all value you can use in docker cmd.
    you can set them into value.yaml 

