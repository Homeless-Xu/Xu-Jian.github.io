---
sidebar_position: 1718
title: 🎪-7️⃣🌐-7️⃣ VPN ➜➜ Wireguard
---

# VPN ✶ Wireguard


🔵 Goal

    vps_ubuntu(vpn.srv) == (vpn.cli)Ros_router
    
        under ros. a lot vlan.
            we want visit homelab vlan. via vps. 
                because homelab no public internet ip.


🔵 wireguard  must know 

    all node is even (both can be server or client)

    client: send     vpn connect request  
    server: accept   vpn connect request  

    only need one node(client node) join another node (server node ).  and vpn is builded! 
        no need both node send request to each other.
            means server node. no need join client node.

    usually 
        server:  have public ip 
        client:  no public ip 
        let client node to join server node. 





🔵 How build wireguaed vpn 

    🔶 server enable ip forward 

    🔶 vpn role 

        one server node. 
        lot client node.
            let client node join same server node. 


    🔶 Peer 

        peer is node.   
        one peer one node. 

        server need add lots client peer. (one client one peer )
        client need add one  server peer


    🔶 Peer.key 

        all node need genrate a pair key. 
            server node need add all client public key. one client peer add one client public key. 
            client node need add one server public key.





🔵 ip forward 

    🔶 check 

        cat /proc/sys/net/ipv4/ip_forward
            1 ok 
            0 deny 

    🔶 enable 

        vi /etc/sysctl.conf
        add
        net.ipv4.ip_forward=1






🔵 VPN Server Basic config


    🔶 install                            apt install wireguard -y

    🔶 generate private key               wg genkey > private
    🔶 use private gnerate public:        wg pubkey < private 

    🔶 create basic confug 

        vi /etc/wireguard/wg-vps.conf

        [Interface]
        Address = 10.214.214.214/32
        ListenPort = 4455
        PrivateKey = yJore1MNmgAAYFunP8DGRl16/9qVFvm83VJVDGDeCUg=



    🔶 enable firewall 

        apt install ufw -y 
        systemctl enable ufw
        systemctl start ufw
        ufw allow 4455 


    🔶 config nic autostart
        
        sudo systemctl enable wg-quick@wg-vps
        sudo systemctl start wg-quick@wg-vps
        sudo systemctl restart wg-quick@wg-vps

        sudo systemctl status wg-quick@wg-vps

        ip a 


    🔶 check status 

        VPS ~ wg
        interface: wg-vps
        public key: fOa79C1HXltCoiSujJK9zIlx2hRD8eqSWL5lr+5TuSI=
        private key: (hidden)
        listening port: 4455








🔵 VPN Client - Ubuntu 💯

        vi /etc/wireguard/wg-dkp.conf



    [Interface]
    Address = 10.214.214.140/32
    PrivateKey = IAnGIuJI+ly3czJ0E1ZKy08CuqVUIOymGPb5ob5PNWw=
    ListenPort = 19629

    [Peer]
    PublicKey = fOa79C1HXltCoiSujJK9zIlx2hRD8eqSWL5lr+5TuSI=
    AllowedIPs = 10.214.214.0/24
    Endpoint = 172.93.42.232:4455
    PersistentKeepalive = 25



        sudo systemctl enable wg-quick@wg-vps
        sudo systemctl start wg-quick@wg-vps
        sudo systemctl restart wg-quick@wg-vps

        sudo systemctl status wg-quick@wg-dkt

        ip a 

 



🔵 VPN Client - Ubuntu Ros: 


    🔶 general key & nic 

        wireguard >> new >> VPN-WG-ROS.. 
        this will create a private and a public key .


    🔶 set ip 

        10.214.214.11/32  VPN-WG-ROS


    🔶 connect vps  

        wireguard >> peer >> add 

            interface:   VPN-WG-ROS
            public key:  vps`s public key. 
            endpiunt:    vps`s internet ip 
            port:        use wg cmd check in vps.  
            allow address:  empty 

        not . vpn is connect. but not ping other vpn node! yet. 




🔵 keep alive 

    add this. or it will disconnect. 

    PersistentKeepalive = 25



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵
🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵


🔵 Design VPN network ✅

    server   wg vpn ip ➜ 10.214.214.214/32

    client_1 wg vpn ip ➜ 10.214.214.11/32
    client_2 wg vpn ip ➜ 10.214.214.22/32
    client_3 wg vpn ip ➜ 10.214.214.33/32

        one vpn node only need one ip.  no need use /24.  that will confuse you. 



🔵 VPN Desc 

    if you have a working handshake. it means vpn works. 
    a working handshake. no need config any allowed ips. 
    
    working vpn. not means you can ping node use vpn ip. 
    this function need config allowed ip.




🔵 Allowed IPS ➜ VPN part 

    if vpn client & vpn server need ping each other use vpn ip. 
    this need config on both client peer. and server peer. 

    allowed ip means. what traffic can use wireguard vpn.


    🔶 Server peer_client_11  Allowed IPS  ➜ add client_11 `s vpn ip   ➜ 10.214.214.11/32
    🔶 server peer_client_22  Allowed IPS  ➜ add client_22 `s vpn ip   ➜ 10.214.214.22/32
    🔶 server peer_client_33  Allowed IPS  ➜ add client_33 `s vpn ip   ➜ 10.214.214.33/32
            this let 10.214.214.xx/32 can use vpn tunnel.   let server can ping 10.214.214.xx


    🔶 Client_11 peer         Allowed IPS  ➜ add server `s vpn ip:     ➜ 10.214.214.214/32
    🔶 Client_22 peer         Allowed IPS  ➜ add server `s vpn ip:     ➜ 10.214.214.214/32 
    🔶 Client_33 peer         Allowed IPS  ➜ add server `s vpn ip:     ➜ 10.214.214.214/32 
            this let 10.214.214.214/32 can use vpn tunnel.   let client can ping 10.214.214.214


    now: 
        server can ping all client.
        all client can ping server node 
        but. client can not ping other client.
            if you need allow client ping client.   change allowed ip on client node.
            🔶 Client_11 peer         Allowed IPS  ➜  ➜ 10.214.214.0/24
            🔶 Client_22 peer         Allowed IPS  ➜  ➜ 10.214.214.0/24
            🔶 Client_33 peer         Allowed IPS  ➜  ➜ 10.214.214.0/24 
                this allow 10.214.214.0/24 use vpn tunnel. in client node.
                    so when you ping 10.214.214.0/24  in client node.
                        client node will forward 10.214.214.0/24 to vpn server. 
                            vpn server. can ping all node.
                                so client can ping other node. 


            🔥 no need change server peer Allowed IPS
                peer under vpn server.
                only decide what client. vpn server can ping out.







🔵 homelab vlan 

    client_11  have vlan_11 
    client_22  have vlan_22 
    server     need visit vlan_11 under client_11

    vlan_11 is under client_11. 
    client_11 of course can visit vlan_11. 

    all need todo is give server a route:  let vlan_11 use vpn tunnel.
    and let sever send vlan_11 traffic to client_11 node.


    just config server `s client_11 `s peer. 
    add a allowed ips like 
    allowed ips = 172.16.1.0/24
        now server can ping 172.16.1.0/24



