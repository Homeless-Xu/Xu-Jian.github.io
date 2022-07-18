---
sidebar_position: 4930
title: 🎪🎪🎪🎪-🔐 Auth
---


# Auth ➜ LDAP SSO oAuth2



🔵 LDAP Desc  

    🔶 Why LDAP
	
        almost all softeare support LDAP.
        LDAP allow you use one account(create in ladp). to login all software.
            windows  login 
            linux ssh
            wifi account


    🔶 LDAP desc

        LightWeight  Directory  Access  Protocol
        jsut a software. storage some user/company information.
        
        LDAP is CS mode:  server + client
            Build & config ladp server first.
            create staff user in ladp server
            join client device (win/mac/linux/wifi) into ladp server.
            login client device use account you created in ladp server
            

    🔶 LDAP software 

        windows server: AD  is most common.
        but i no like win. so use linux ldap software:  openLDAP.


🔵 SSO 

    🔶 Why SSO 

        LDAP allow you use one account at all app
        but in each app you need type in same username/passwrod ! 
        if you have lots app need login. 
        we have a better choose: SSO 

        SSO. only you login one app. then auto login all other app.
        SSO: sigle sign-on



    🔶 SSO Software 

        Authelia 


🔵 0Auth2 

    🔶 Why oAuth2

        SSO is local use. not for internet. 
        oAuth2: Open Auth. it is for internet.

        a lot internet app allow you login via google/facebook ...
        that app use oAuth.
        that app get user info from google (not username password)
        (because you sign google. already.)





