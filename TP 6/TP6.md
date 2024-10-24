# TP6 : Des bo services dans des bo LANs

## Sommaire

- [TP6 : Des bo services dans des bo LANs](#tp6--des-bo-services-dans-des-bo-lans)
  - [Sommaire](#sommaire)
- [I. Le setup](#i-le-setup)
  - [0. Schéma](#0-schéma)
  - [II. LAN clients](#ii-lan-clients)
  - [2. Client](#2-client)
- [III. LAN serveurzzzz](#iii-lan-serveurzzzz)
  - [3. Serveur DHCP](#3-serveur-dhcp)


# I. Le setup

☀️ **Prouvez que...**

- une machine du LAN1 peut joindre internet (ping un nom de domaine)

Avec dhcp.tp6.b1:
```ps
[tisteba@dhcp ~]$ ping dofus.com
PING dofus.com (18.245.175.113) 56(84) bytes of data.
64 bytes from server-18-245-175-113.cdg55.r.cloudfront.net (18.245.175.113): icmp_seq=1 ttl=239 time=380 ms
64 bytes from server-18-245-175-113.cdg55.r.cloudfront.net (18.245.175.113): icmp_seq=2 ttl=239 time=45.5 ms
64 bytes from server-18-245-175-113.cdg55.r.cloudfront.net (18.245.175.113): icmp_seq=3 ttl=239 time=44.1 ms
64 bytes from server-18-245-175-113.cdg55.r.cloudfront.net (18.245.175.113): icmp_seq=4 ttl=239 time=50.3 ms
^C
--- dofus.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 44.105/129.897/379.701/144.242 ms

```

- une machine du LAN2 peut joindre internet (ping nom de domaine)

Avec dns.tp6.b1:
```ps
[tisteba@dns ~]$ ping dofus.com
PING dofus.com (18.245.175.45) 56(84) bytes of data.
64 bytes from server-18-245-175-45.cdg55.r.cloudfront.net (18.245.175.45): icmp_seq=1 ttl=239 time=54.9 ms
64 bytes from server-18-245-175-45.cdg55.r.cloudfront.net (18.245.175.45): icmp_seq=2 ttl=239 time=46.7 ms
64 bytes from server-18-245-175-45.cdg55.r.cloudfront.net (18.245.175.45): icmp_seq=3 ttl=239 time=43.1 ms
64 bytes from server-18-245-175-45.cdg55.r.cloudfront.net (18.245.175.45): icmp_seq=4 ttl=239 time=52.2 ms
^C
--- dofus.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 43.146/49.260/54.924/4.604 ms
```

- une machine du LAN1 peut joindre une machine du LAN2 (ping une adresse IP)

Avec ```dhcp.tp6.b1``` vers ```dns.tp6.b1```:

```ps
[tisteba@dhcp ~]$ ping 10.6.2.12
PING 10.6.2.12 (10.6.2.12) 56(84) bytes of data.
64 bytes from 10.6.2.12: icmp_seq=1 ttl=63 time=1.22 ms
64 bytes from 10.6.2.12: icmp_seq=2 ttl=63 time=2.72 ms
64 bytes from 10.6.2.12: icmp_seq=3 ttl=63 time=2.85 ms
^C
--- 10.6.2.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 1.218/2.262/2.849/0.740 ms
```

## II. LAN clients

## 2. Client



☀️ **Prouvez que...**

- le client a bien récupéré une adresse IP en DHCP

```ps
baptiste@baptiste-VirtualBox:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e1:b3:3a brd ff:ff:ff:ff:ff:ff
   ☀️ inet 10.6.1.37/24 metric 100 brd 10.6.1.255 scope global dynamic☀️ enp0s8
       valid_lft 2591867sec preferred_lft 2591867sec
    inet6 fe80::a00:27ff:fee1:b33a/64 scope link 
       valid_lft forever preferred_lft forever
```

- vous avez bien `1.1.1.1` en DNS
```
baptiste@baptiste-VirtualBox:~$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s8)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
☀️Current DNS Server: 1.1.1.1
       DNS Servers: 1.1.1.1

```
- vous avez bien la bonne passerelle indiquée
```
baptiste@baptiste-VirtualBox:~$ ip r s
default via 10.6.1.254 dev enp0s8 proto dhcp src 10.6.1.37 metric 100 
1.1.1.1 via 10.6.1.254 dev enp0s8 proto dhcp src 10.6.1.37 metric 100 
10.6.1.0/24 dev enp0s8 proto kernel scope link src 10.6.1.37 metric 100 
☀️10.6.1.254 dev enp0s8 proto dhcp scope link src 10.6.1.37 metric 100 

```

- que ça `ping` un nom de domaine public sans problème magueule

```ps
baptiste@baptiste-VirtualBox:~$ ping dofus.com
PING dofus.com (18.66.248.67) 56(84) bytes of data.
64 bytes from server-18-66-248-67.dus51.r.cloudfront.net (18.66.248.67): icmp_seq=1 ttl=57 time=38.2 ms
64 bytes from server-18-66-248-67.dus51.r.cloudfront.net (18.66.248.67): icmp_seq=2 ttl=57 time=53.7 ms
64 bytes from server-18-66-248-67.dus51.r.cloudfront.net (18.66.248.67): icmp_seq=3 ttl=57 time=45.9 ms
^C
--- dofus.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 38.177/45.901/53.668/6.324 ms

```
# III. LAN serveurzzzz

## 1. Serveur Web
### 3. Analyse et test

☀️ **Déterminer sur quel port écoute le serveur NGINX**

```ps
[root@web ~]# sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1428,fd=6),("nginx",pid=1427,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1428,fd=7),("nginx",pid=1427,fd=7))
```

☀️ **Ouvrir ce port dans le firewall**

```ps
[root@web ~]# sudo firewall-cmd --permanent --add-port=80/tcp
success
[root@web ~]# sudo firewall-cmd --reload
success
```

☀️ **Visitez le site web !**

```html
baptiste@baptiste-VirtualBox:~$ curl http://10.6.2.11
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/
      
      html {
        height: 100%;
        width: 100%;
      }  
        body {
  background: rgb(20,72,50);
  background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
  background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
  background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  [...]
```


## 2. Serveur DNS

## 4. Analyse du service

☀️ **Déterminer sur quel(s) port(s) écoute le service BIND9**

```ps
[root@dns ~]# sudo ss -lnpt | grep 53
LISTEN 0      10         10.6.2.12:53        0.0.0.0:*    users:(("named",pid=1866,fd=25))
LISTEN 0      10         127.0.0.1:53        0.0.0.0:*    users:(("named",pid=1866,fd=22))
LISTEN 0      4096       127.0.0.1:953       0.0.0.0:*    users:(("named",pid=1866,fd=28))
LISTEN 0      10             [::1]:53           [::]:*    users:(("named",pid=1866,fd=27))
LISTEN 0      4096           [::1]:953          [::]:*    users:(("named",pid=1866,fd=29))
```

☀️ **Ouvrir ce(s) port(s) dans le firewall**
```ps
[root@dns ~]# sudo firewall-cmd --permanent --add-port=80/tcp
success
[root@dns ~]# sudo firewall-cmd --reload
success
```
## 5. Tests manuels

☀️ **Effectuez des requêtes DNS manuellement** depuis le serveur DNS lui-même dans un premier temps

```ps
[root@dns ~]# dig web.tp6.b1 @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21287
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 6896965f5dcfa9ce01000000671674e5ac12c4e8d352f655 (good)
;; QUESTION SECTION:
;web.tp6.b1.                    IN      A

;; ANSWER SECTION:
☀️web.tp6.b1.             86400   IN      A       10.6.2.11

;; Query time: 1 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Mon Oct 21 17:36:05 CEST 2024
;; MSG SIZE  rcvd: 83
```

```ps
[root@dns ~]# dig ynov.com @10.6.2.12

; <<>> DiG 9.16.23-RH <<>> ynov.com @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31064
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 90e9d493b2986f8a0100000067167737987b9a491bb4b134 (good)
;; QUESTION SECTION:
;ynov.com.                      IN      A

☀️;; ANSWER SECTION:
ynov.com.               300     IN      A       104.26.10.233
ynov.com.               300     IN      A       172.67.74.226
ynov.com.               300     IN      A       104.26.11.233

;; Query time: 82 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Mon Oct 21 17:45:59 CEST 2024
;; MSG SIZE  rcvd: 113
```

☀️ **Effectuez une requête DNS manuellement** depuis `client1.tp6.b1`

- pour obtenir l'adresse IP qui correspond au nom `web.tp6.b1`

```ps
baptiste@baptiste-VirtualBox:~$ dig web.tp6.b1 @10.6.2.12

; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36477
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: f01a14b58430e4b101000000671a10d917922e0c2ae7037c (good)
;; QUESTION SECTION:
;web.tp6.b1.			IN	A

;; ANSWER SECTION:
web.tp6.b1.		86400	IN	A	10.6.2.11

;; Query time: 4 msec
;; SERVER: 10.6.2.12#53(10.6.2.12) (UDP)
;; WHEN: Thu Oct 24 11:16:20 CEST 2024
;; MSG SIZE  rcvd: 83

```




☀️ **Capturez une requête DNS et la réponse de votre serveur**

*voir fichier toto.pcap*



## 3. Serveur DHCP

☀️ **Créez un nouveau client `client2.tp6.b1` vitefé**


```ps
baptiste@baptiste-VirtualBox:~$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s8)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 10.6.2.12
       DNS Servers: 10.6.2.12
```



