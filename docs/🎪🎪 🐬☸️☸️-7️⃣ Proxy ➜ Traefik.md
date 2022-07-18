---
sidebar_position: 2930
title: 🎪🎪🐬☸️☸️7️⃣ Proxy ➜ Traefik ❌
---


# K8s Proxy ✶ Traefik


🔵 WHY 











🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 misc 
    https://artifacthub.io/packages/helm/traefik/traefik

    helm repo add traefik https://helm.traefik.io/traefik
    helm install traefik traefik/traefik --namespace=test
    kubectl port-forward traefik 9000:9000






    cat <<EOF>  proxy-traefik.yaml
    apiVersion: traefik.containo.us/v1alpha1
    kind: IngressRoute
    metadata:
      name: dashboard
    spec:
      entryPoints:
        - web
      routes:
        - match: Host(`traefik.localhost`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))
          kind: Rule
          services:
            - name: api@internal
              kind: TraefikService
    EOF

    kubectl apply -f proxy-traefik.yaml





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 K3s. Traefik 

    k3s. use traefik as default. 
    so no need install. just comfig it. 
    cat /var/lib/rancher/k3s/server/manifests/traefik.yaml



🔵 k3s traefik dashboard


    spec:
      chart: https://%{KUBERNETES_API}%/static/charts/traefik-1.81.0.tgz
      set:
        rbac.enabled: "true"
        ssl.enabled: "true"
        metrics.prometheus.enabled: "true"
        kubernetes.ingressEndpoint.useDefaultPublishedService: "true"
        image: "rancher/library-traefik"

        dashboard.enabled: "true"             # <-- add this line
        dashboard.domain: "traefik.internal"  # <-- and this one with a resolvable DNS name


    Helm will pick up the changes automagically 
    and the dashboard will be available under http://traefik.internal/dashboard/.
    Keep in mind that after a reboot of the master the file will be restored without the added lines.


    traefik.k3s.rv.net
