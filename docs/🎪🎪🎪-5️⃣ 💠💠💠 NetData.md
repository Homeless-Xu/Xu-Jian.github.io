---
sidebar_position: 3930
title: 🎪🎪🎪-5️⃣💠💠💠 ➜ NetData
---

# Moniter ▪  NetData 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵HomeLAB ✶ Moniter ▪  NetData 




🔵 Manual 

🔶 Offical
https://learn.netdata.cloud/docs/agent/packaging/installer/methods/kubernetes


⭐️⭐️⭐️⭐️⭐️⭐️⭐️
https://www.gitiu.com/journal/netdata-install/




🔵 WHY  
zabbix + grafana too heavy. 


🔵 NetData Desc 
https://github.com/netdata/netdata




🔵 Docker install 


  docker run -d --name=netdata \
    -p 19999:19999 \
    -v netdataconfig:/etc/netdata \
    -v netdatalib:/var/lib/netdata \
    -v netdatacache:/var/cache/netdata \
    -v /etc/passwd:/host/etc/passwd:ro \
    -v /etc/group:/host/etc/group:ro \
    -v /proc:/host/proc:ro \
    -v /sys:/host/sys:ro \
    -v /etc/os-release:/host/etc/os-release:ro \
    --restart unless-stopped \
    --cap-add SYS_PTRACE \
    --security-opt apparmor=unconfined \
    netdata/netdata



🔵 Cloud free ... netdata ..



🔵  Usage.. server + agent 





🔵 Agent  - MAC 






🔵 Agent. esxi ??



