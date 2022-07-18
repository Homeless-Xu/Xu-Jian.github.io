---
sidebar_position: 1910
title: 🎪-9️⃣📀 STO NAS ➜➜ Cloud Driver
---

# NAS ✶ Cloud ➜ Alist




🔵 Needs: Sync ✅  cloud & nas 

    1. use synology cloud sync app: sync all dropbox & google drive to NAS
    2. use nas / alist to manage synced folder in NAS


🔵 Needs: Sync ❌  local & local 


🔵 Need: Manager ✅

    connect all cloud drive & local drive to alist.
    use alist website to manage all dirve.

    you can share file to other people use alist too. 
    have guest mode. 




🔵 Misc 

    s3cmd?  
        linux mount nas`s s3.
        use rsync. put all config in  s3






🔵 Needs:  Version Control / git 





🔵 Need: Share / web/ internet / webdav

	how share file to other people .











🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  Alist  Demo 

🔵 Alist - Docker 

	docker run -d --restart=always 
	-v /etc/alist:/opt/alist/data 
	-p 5244:5244 
	--name="alist" 
	xhofe/alist:latest




🔵 Login 

    http://10.1.1.89:5244/@manage/login

    🔶 default password 

        check log
        docker exec -it alist ./alist -password



🔵 change admin password.


🔵 add local folder

    🔶 mount 

        mount nas folder into docker.
        alist mount local folder.


    🔶 add account/folder

        type: native (local dirve)
        virtual path:   /xxx   ➜ what path show on alist dashboard.
        index (set folder order)
        /


        extract_folder​
            front  when order. put folder at head
            back:  when order. put folder at last



🔵 add folder password 

    alisy.0214.icu/Dir1/xxx
    alisy.0214.icu/Dir1/yyy

    if want give folder Dir1  add password 

    meta
    path: /Dir1
    password:  set a password 
    save 



🔵 set alist reserve proxy 

    use traefik 






🔵 Add remote google Driver  ✅✅✅✅✅✅

	‼️  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/
	‼️  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/
	‼️  Offical best manual    https://alist-doc.nn.ci/docs/driver/googledrive/


🔶 token tool:

    https://tool.nn.ci/google/request


