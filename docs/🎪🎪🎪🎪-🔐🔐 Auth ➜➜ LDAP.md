---
sidebar_position: 4930
title: 🎪🎪🎪🎪-🔐🔐 Auth ➜➜ OpenLDAP
---


# Auth ➜ OpenLDAP



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 openLDAP.  basic 

no need database. 


🔵 LDAP Desc Detail💯

    🔶 LDAP Desc 

        ldap is just like domain/url 
        domain must have two parts. do ladp must have 2+ dc.
        and you can set sub ldap too.  just like sub domain

            root domain:  *.0214.ark     ➜ dc=0214,dc=ark
            sub  domain:  xx.0214.ark    ➜ dc=xx,dc=0214,dc=ark

        CN. OU  is just like folder/file 
            CN=test,OU=developer,DC=rv,DC=ark
                cn:  a file    named test
                ou:  a folder  named developer

            if you need find file/cn 
                use this
                    CN=test,OU=developer,DC=rv,DC=ark




    🔶 Create a Domain

        ◎ LDAP  domain:      xxx.yyy          rv.ark 
        ◎ LDAP_BASE_DN:      dc=xxx,dc=yyy    dc=rv,dc=ark


    🔶 Domain tree demo 

        dc=rv,dc=ark
            cn=file_0
            ou=Folder_1
                cn=file_1
            ou=Folder_2
                cn=file_21
                cn=file_22

    🔶 DC OU CN 
        
        DC( like root /):  top of ldap 
        OU( like folder):  ... 
        CN( like file  ):  end of ldap
            you must have CN.
            you can  have OU
            you can  have lots OU
            file can under root  (DC)
            file can under foler (OU)


        ‼️ every cn have his own unique path   cn-dc   cn-ou-dc  ‼️ 
        ‼️ every cn have his own unique path   cn-dc   cn-ou-dc  ‼️ 
        ‼️ every cn have his own unique path   cn-dc   cn-ou-dc  ‼️ 



    🔶 DN /  base_dn
        Distinguished Name         ➜  file cn`s path   ➜
        Base Distinguished Name    ➜  root`s path      ➜   dc=rv,dc=ark







🔵 database prepair 

    🔶 needs 

        i have mariadb already.
        i use same mariadb for all docker.
        so i not create new mariadb. i use what i have already.

        ldap db:             dbldap
        ldap user:           userldap
        ldap user password:  userldappassword
        set permit:  let user: userldap  can only visit dbldap


    🔶 cmd 

        MariaDB [(none)]> CREATE DATABASE dbldap;
        MariaDB [(none)]> CREATE USER 'userldap'@'%' IDENTIFIED BY 'password';
        MariaDB [(none)]> GRANT ALL PRIVILEGES ON dbldap.* TO 'userldap'@'%';
        MariaDB [(none)]> show databases;






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 openLDAP. Docker demo  - prepair 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 openLDAP. Docker demo  - prepair 
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 openLDAP. Docker demo  - prepair 

🔵 Other Demo 
  https://gist.github.com/thomasdarimont/d22a616a74b45964106461efb948df9c




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 worked demo 1

⭐️ only change domain & password. no other ⭐️


		version: '3'
		services:
		openldap:
			image: osixia/openldap:latest
			container_name: openldap
			domainname: "rv.ark"
			hostname: "openldap"
			environment:
			LDAP_LOG_LEVEL: "256"
			LDAP_ORGANISATION: "RVLAB Inc."
			LDAP_DOMAIN: "rv.ark"
			LDAP_BASE_DN: "dc=rv,dc=ark"
			LDAP_ADMIN_PASSWORD: "changeme"
			LDAP_CONFIG_PASSWORD: "changeme"
			LDAP_READONLY_USER: "false"
			LDAP_READONLY_USER_USERNAME: "readonly"
			LDAP_READONLY_USER_PASSWORD: "readonly"
			LDAP_RFC2307BIS_SCHEMA: "false"
			LDAP_BACKEND: "mdb"
			LDAP_TLS: "true"
			LDAP_TLS_CRT_FILENAME: "ldap.crt"
			LDAP_TLS_KEY_FILENAME: "ldap.key"
			LDAP_TLS_CA_CRT_FILENAME: "ca.crt"
			LDAP_TLS_ENFORCE: "false"
			LDAP_TLS_CIPHER_SUITE: "SECURE256:-VERS-SSL3.0"
			LDAP_TLS_PROTOCOL_MIN: "3.1"
			LDAP_TLS_VERIFY_CLIENT: "demand"
			LDAP_REPLICATION: "false"
			#LDAP_REPLICATION_CONFIG_SYNCPROV: "binddn="cn=admin,cn=config" bindmethod=simple credentials=$LDAP_CONFIG_PASSWORD searchbase="cn=config" type=refreshAndPersist retry="60 +" timeout=1 starttls=critical"
			#LDAP_REPLICATION_DB_SYNCPROV: "binddn="cn=admin,$LDAP_BASE_DN" bindmethod=simple credentials=$LDAP_ADMIN_PASSWORD searchbase="$LDAP_BASE_DN" type=refreshAndPersist interval=00:00:00:10 retry="60 +" timeout=1 starttls=critical"
			#docker-compose.ymlLDAP_REPLICATION_HOSTS: "#PYTHON2BASH:['ldap://ldap.example.org','ldap://ldap2.example.org']"
			KEEP_EXISTING_CONFIG: "false"
			LDAP_REMOVE_CONFIG_AFTER_SETUP: "true"
			LDAP_SSL_HELPER_PREFIX: "ldap"
			tty: true
			stdin_open: true
			volumes:
			- /var/lib/ldap
			- /etc/ldap/slapd.d
			- /container/service/slapd/assets/certs/
			ports:
			- "389:389"
			- "636:636"
		phpldapadmin:
			image: osixia/phpldapadmin:latest
			container_name: phpldapadmin
			environment:
			PHPLDAPADMIN_LDAP_HOSTS: "openldap"
			PHPLDAPADMIN_HTTPS: "false"
			ports:
			- "8080:80"
			depends_on:
			- openldap





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 simple demo 2

		version: '3'
		services:
		openldap:
			image: osixia/openldap:latest
			container_name: openldap
			domainname: "rv.ark"
			hostname: "openldap"
			environment:
			LDAP_DOMAIN: "rv.ark"
			LDAP_BASE_DN: "dc=rv,dc=ark"
			LDAP_ADMIN_PASSWORD: "changeme"
			LDAP_CONFIG_PASSWORD: "changeme"
			volumes:
			- /mnt/dpnvme/DMGS-DKP/Auth-openLDAP/ldap.dir:/var/lib/ldap
			- /mnt/dpnvme/DMGS-DKP/Auth-openLDAP/slapd.dir:/etc/ldap/slapd.d
			ports:
			- "389:389"
			- "636:636"

		phpldapadmin:
			image: osixia/phpldapadmin:latest
			container_name: phpldapadmin
			environment:
			PHPLDAPADMIN_LDAP_HOSTS: "openldap"
			PHPLDAPADMIN_HTTPS: "false"
			ports:
			- "8080:80"
			depends_on:
			- openldap





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Use demo 


🔵 visit & login 

  http://172.16.1.144:8080
    default cn user is admin.    
      
      cn=admin,dc=rv,dc=ark
      cn=admin,dc=example,dc=org


🔵 design ldap 

  domain 
    cn:  xxxxx  ➜ admin/manager / root ..   manager  account  (default ??? )
    ou:  prod 
      cn: xxxx
    ou:  test 
      cn: yyyy
    ou:  IT 

  🔶 Design 

    two kinds:  cn & ou. 
    cn is user.  ou is group. 

    a lot pc. (department )

    router. firewall. wifi. 
    win. linux. 
    traefik. hemidall. 

    local vm.  remote vps. 









🔵 creatr user demo 

  🔶 create group. 

    ‼️ if want create user( need gpid ) 
    must create group first. (this will create a gpid)
    >> Generic: Postfix Group 


  🔶 create user

    Generic: User Account


  🔶 add email to user 

    user >>  Add new attribute >> email ...



🔵 Create OU 

	Generic: Organisational Unit







🔵 add device to ldap 



🔵 How Join 

    -  join ldap  from client. 
    -  add client in server

            two way to add client to domain.
            usually use client to join server.
            somethings error.  try add client from server. 


  🔶 mikrotik 
  
    ros radius
    no support.openldap.  . 
    only support windows ad (NPS). 



