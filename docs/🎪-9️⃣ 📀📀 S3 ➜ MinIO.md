---
sidebar_position: 1920
title: 🎪-9️⃣📀📀 S3 ➜ MinIO
---

# Storage ✶ S3 ➜ MinIO



🔵 MinIO Desc 

    OSS: 
        give file a address/url.  
        use url to use file like download
        Amazon S3 / MinIO 


    a lot service need s3 storage.`
    ceph has s3, but config not easy. so use this.



🔵 Demo Desc



🔵 tool 

    s3cmd.
    mount s3 to linux.  like local disk.



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Docker Compose Demo 

    STOminio:
        container_name: STO-S3-MinIO
        image: minio/minio:latest
        restart: always    
        privileged: true
        ports:
            - "9000:9000"    # api   port
            - "9009:9001"    # webui port
        environment:
            MINIO_ROOT_USER: admin             # webui:  user name 
            MINIO_ROOT_PASSWORD: admin         # webui:  user password


        volumes:    
            - /mnt/dpnvme/DMGS-DKP/STO-S3-MinIO-Disk-NeverDEL:/data
            # disk
            - /mnt/dpnvme/DMGS-DKP/STO-S3-MinIO-Config:/root/.minio/
            # config

        command: server --console-address ':9001' /data  
            # let mini use data as disk 







🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Config



🔵 Basic 

    Bucket:   like disk
    Object:   like file / folder



🔵 Create Bucket Config 

    s3-dir-wikijs
        versioning ‼️
            only can enable on create.
            

🔵 set Bucket permit

    want easy    use policy
    want custom  use rules

        xx bucket >> manage >> access policy  ➜ private / public  / custom (use rules blow)
        xx bucket >> manage >> access rules   ➜ 



🔵 visit public bucket file

    s3-dir-wikijs/11.png
    http://172.16.1.140:9000/s3-dir-wikijs/11.png



🔵 visit Private bucket file

🔶 Create API user! 

    no any use by default.
    the docker cmd only create minio webui user.  not api user.
    here need api use.


🔶 Visit private file

    if have webui access. visit use webui.
    if no webui.  install a tool. use cmd to visit file.
        in linux use s3cmd. is very good. tool 


🔵 s3cmd  linux  use demo

    login.
    https://www.bilibili.com/video/BV1w34y1k7BG?vd_source=7a6c9ba7c1460c134545b9e1f189e1cd

