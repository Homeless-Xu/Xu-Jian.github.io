---
sidebar_position: 3930
title: 🎪🎪🎪-5️⃣💠💠💠 ➜ Bitwarden
---

# Password Manager 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 DSM instal DSM 

	install docker 
	install bitwarden

	

🔵 Config DSM Docker Https 

  🔶 desc 

    if docker in dsm need https/ssl 
    it is easy to set in dsm. 
    dsm have build in reverse proxy. 

    like here. 
    bitwarden docker use http 88 port.
    but bitwaren have to use https.
    so we froward dsm`s https://xxx.9443 to docker`s 80.

    now we visit url  https://bit.rv.ark:9443 it forward to bitwarden 80.


  🔶 make ssl 

	use  mkcert to generate local ssl *.rv.ark 


  🔶 set static dns 

    bit.rv.ark  >>  10.1.1.89 (dsm host ip)


🔵 run bitwarden docker in dsm.



🔵 web visit 
	https://bit.rv.ark:9443




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 USE 

	bitwarden app.
	>> left above corner 
		>> set local bitwarden url https://bit.rv.ark:9443

	login to local bit 


🔶 import old data 

  use web.  no found import in mac app. 
  web >> tool >> import 

	now . web is useless.
	just run the server. 
	everything else can be done in bitwarden app. 



	


	vps.   

	frp.0214.ICU  ssl.
	>> dvm.frpc



	https://bit.rv.ark:1443



🔵 nginx fxdl  to homelab.

	dvm.0214.icu >>

	rdvm.0214.icu >> lo...




