---
sidebar_position: 4930
title: 🎪🎪🎪🎪-🔐🔐🔐 SSO ➜ Authelia install
---

# SSO Authelia


🔵 file 

    🔶 Offical config manual 

    https://www.authelia.com/configuration/


    🔶 docker - compose 

    https://github.com/authelia/authelia/blob/master/examples/compose/lite/docker-compose.yml




🔵 prepair 

    authelia is for something like traefik.
    you better config traefik first. and enable ssl.

    you need like openldap too. 

    domain. & SSL. 




🔵 Authelia Docker needs.

    1. redis    - must 
    2. database - option 
        Authelia have buildin database  sqlite3.




🔵 email / verify / 2FA 

    every user first login authelia need use 2FA verify.
    set 2FA means. must set email first.


    🔶 send & receive email 

        how sender  email:   use email`s smtp. function.  ➜ in authelia config 
        who receive emial:   config email for login user. ➜ in ldap / of file. 


    🔶 gmail smtp 

        very easy.  just need email address and a app password.
        ‼️ must login google create a app password. email password no work ‼️
        ‼️ must login google create a app password. email password no work ‼️
        ‼️ must login google create a app password. email password no work ‼️


    🔶 how verify 

        1. authelia  send out a email.
        2. ldap user receive a email
        3. ldap get a link from email. 
            link have two parts.
                one: paste link to 2fa app like google auth... or  bitwarden. to get password.
                one: type password back to verify.








🔵 Config must know 

🔶 authelia URL  ✅

    default_redirection_url: https://auth.0214.icu 
        authelia need a url to work.
            auth.domain  sso.domain  any name you like. 
            if you have a port to visit authelia. add it behind.
                    https://xxx.0214.icu:yyy


🔶 default rule 

    access_control:
        default_policy: deny
    
            authelia should like firewall. 
            deny all incoming request by default.
            but we need allow the app (who use authelia)

        -  bypass       ➜  no need password 
        -  one_factor   ➜  password
        -  two_factor   ➜  passwor d  + 2FA


🔶 Auth method:

    authentication_backend:
        file:  ➜ means you choose file to auth user.
        ldap:  ➜ means you choose ldap to auth user.

    if file: 
        need create a new file. 
        input user * passwor in that file. 
            🔥     https://github.com/authelia/authelia/tree/master/examples/compose/lite/authelia

    if ldap: 
        my demo .




🔵 SMTP comfig 💯 

    https://www.authelia.com/configuration/notifications/smtp/


    notifier:
    smtp:
        username: xx2610@gmail.com 
        password: nfmthdxxxxxxx
        sender: xx2610@gmail.com
        host: smtp.gmail.com
        port: 587


    ‼️ password is not email password. it is google app passowr ‼️
    ‼️ password is not email password. it is google app passowr ‼️
    ‼️ password is not email password. it is google app passowr ‼️






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  docker compose ✅


    DBredis:
        container_name: DB-Redis
        image: 'bitnami/redis:latest'
        ports:
            - "6379:6379"
        environment:
            - ALLOW_EMPTY_PASSWORD=yes
        volumes:
            - /mnt/dpnvme/DMGS-DKP/DB-Redis:/bitnami
                # need set host folder permit like 777
                # default puid is like 1001. 

    AUTHauthelia:
        container_name: Auth-SSO-Authelia
        image: authelia/authelia:latest
        restart: unless-stopped
        ports:
            - 9091:9091
        volumes:
            - /mnt/dpnvme/DMGS-DKP/Auth-SSO-Authelia:/config
        depends_on:
            - DBredis







🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  config demo 💯 ✅


		# yamllint disable rule:comments-indentation
		---
		###############################################################################
		#                           Authelia Configuration                            #
		###############################################################################

		theme: light #light/dark
		jwt_secret: 7tiqSgZY8kb8JthmoVoHWja2 
		#any text or number you want to add here to create jwt Token
		default_redirection_url: https://auth.0214.icu   # ⭐️⭐️⭐️⭐️⭐️
		server:
		host: 0.0.0.0
		port: 9091
		path: ""
		read_buffer_size: 4096
		write_buffer_size: 4096
		enable_pprof: false
		enable_expvars: false
		disable_healthcheck: false
		tls:
			key: ""
			certificate: ""

		log:
		level: debug

		totp:
		issuer: 0214.icu        # ⭐️⭐️⭐️⭐️⭐️  no any port. just main domain.   not sub domain
		period: 30
		skew: 1

		authentication_backend:
		ldap:
			implementation: custom
			url: ldap://openldap
			# ⭐️ must 
			# if in same docker network. can use dns name. 
			# 389 is default port.
			timeout: 5s
			start_tls: false
			tls:
			server_name: ldap.example.com
			skip_verify: false
			minimum_version: TLS1.2
			base_dn: OU=OU-SSO-Authelia,DC=rv,DC=ark
			# ⭐️ DC=rv,DC=ark                    ➜ this allow all domain user login to authelia.
			# ⭐️ OU=OU-SSO-Authelia,DC=rv,DC=ark ➜ this only allow user under OU-SSO-Authelia can login authelia
			users_filter: (&({username_attribute}={input})(objectClass=person))
			username_attribute: uid
			mail_attribute: mail
			display_name_attribute: displayName
			groups_filter: (&(member={dn})(objectClass=groupOfNames))
			group_name_attribute: cn
			permit_referrals: false
			permit_unauthenticated_bind: false
			user: CN=admin,DC=rv,DC=ark
			# ⭐️ cn just use admin. no change; 
			password: Ys
			# ⭐️ domain admin `s password

			# ⭐️⭐️⭐️⭐️⭐️ openldap. username must not include . ➜ xxyy ok     xx.yy no ok. 
			# ⭐️⭐️⭐️⭐️⭐️ openldap. username must not include . ➜ xxyy ok     xx.yy no ok. 
			# ⭐️⭐️⭐️⭐️⭐️ openldap. username must not include . ➜ xxyy ok     xx.yy no ok. 


		access_control:
		default_policy: deny

		rules:
			## bypass rule
			- domain: "auth.0214.icu" # ⭐️⭐️ he url that authelia itself use.  no port.
			policy: bypass
			- domain: 
			- "dashy.0214.icu"      #⭐️⭐️xample domain to protect no port.
			- "jumpserver.0214.icu" #⭐️⭐️example domain to protect no port.
			policy: one_factor
			- domain: 
			- "dsm.0214.icu"        #⭐️⭐️example subdomain to protec no port.
			- "dvm.0214.icu"        #⭐️⭐️xample subdomain to protect no port.
			policy: two_factor

		session:
		name: authelia_session
		secret: unsecure_session_secret #any text or number you want to add here to create jwt Token
		expiration: 3600  # 1 hour
		inactivity: 300  # 5 minutes
		domain: 0214.icu  # ⭐️⭐️⭐️⭐️⭐️  no port.  main domain. 

		regulation:
		max_retries: 3
		find_time: 10m
		ban_time: 12h

		storage:
		local:
			path: /config/db.sqlite3          # can use MySQL too
		encryption_key: tujXiHx2ety6HRErqquML35m # encryption  database info
		

		notifier:
		smtp:
			username: xx2610@gmail.com 
			password: nfmthdxnopxx
			sender: xx2610@gmail.com
			host: smtp.gmail.com
			port: 587