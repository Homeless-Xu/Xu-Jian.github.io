---
sidebar_position: 4930
title: 🎪🎪🎪🎪-🔐🔐 Auth ➜➜ radius
---


# Auth ➜ radius



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 radius. server Demo Synology ✅

🔵 nas. create ldap & user 

    🔶 create ldap
        fqdn:   guest.rv.ark 
        base dn:  dc=guest,dc=rv,dc=ark
        bind dn:  uid=root,cn=users,dc=guest,dc=rv,dc=ark

    🔶 enable admin & set password

    🔶 create test user   user01   12345678

🔵 join nas to ldap.

    dns address:  need a dns can ping guest.rv.ark

    dn account: admin 
    base dn: dc=guest,dc=rv,dc=ark



🔵 nas create radius.

    so radius can use user in ldap.
    1. auth port   1812. 
    allow ldap user only 



🔵 add radius client

    wifi/ap/ros is client.




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 radius. server Demo ros ✅
  https://kknews.cc/code/o4a4ne6.html

  ros have radius server too. 
  it is user manager. 


🔵 enable radius server  

    maneger >> session > setting ..  enable  
        1812 auth 
        1813 acct

        advance . can use paypel 


🔵 add client device(not user)

    all wifi ap hardware need join radius first
    so their wireless can support radius. 


    name:  any 
    share secrt:    encty between device and ros .
    client device ip:   
    coa port:          3799 default.



🔵 SRV:  Create group  - option

    rg-gst-wifi  guest wifi radius group 
        here can set a lot. like speed limit.



🔵 SRV:  Create user 

    manager >> user >> add 


🔵 SRV:  User Manager Portal 

    http://router-ip/um
    fuck good...


🔵 SRV: Misc 

    🔶 html custom.

    all file can custom.
    html can change everything..
    User Manager data is in user-manager5


🔵 SRC: enable radisu imcoming  ?

    ros >> radius >> incoming >> accrpt   port 3799



🟢 CLI - Device Join Radius  
    mikrotik. chr(client) join mikrotik rb4011.(server)

    radius >> add >>  
        servie:  
        server address:  10.111.111.11
        secret:   test
        protol:  (in srv. manager. set.cert... )


🔵 user manager session  

    user manager >> session is for client user. not for client device.
    you join wifi hardware in. you can not check if joined.
    config wifi. use radius.  use radius user to login wifi.
    then you can see the user in session. 



🟢 CLI - User Join Radius 
    enable ap`s radisu.
    done..
    then you can use raidus server`s user to login 




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Ros. User Manager - Detail

‼️ User Manager offical manual   https://help.mikrotik.com/docs/display/ROS/User+Manager
‼️ User Manager offical manual   https://help.mikrotik.com/docs/display/ROS/User+Manager
‼️ User Manager offical manual   https://help.mikrotik.com/docs/display/ROS/User+Manager



🔵 Srv. let radius support ladp ?
    windows ad have NPS. if use windows ad. it can support radius. 
    openldap ... no ??    so forget it. 
    



🔵 um basic 

    UM  >> user >> 

    ◎ Shared user:  1 ➜ no login many device use one account at same time.  



🔵 func - pay 

