# TP7 : On dit chiffrer pas crypter

## Sommaire

- [TP7 : On dit chiffrer pas crypter](#tp7--on-dit-chiffrer-pas-crypter)
  - [Sommaire](#sommaire)
- [II. Serveur Web](#ii-serveur-web)
- [III. Serveur VPN](#iii-serveur-vpn)


# II. Serveur Web

# Serveur Web

- [Serveur Web](#serveur-web)
  - [1. HTTP](#1-http)
    - [B. Configuration](#b-configuration)
    - [C. Tests client](#c-tests-client)
    - [D. Analyze](#d-analyze)
  - [2. On rajoute un S](#2-on-rajoute-un-s)
    - [A. Config](#a-config)
    - [B. Test test test analyyyze](#b-test-test-test-analyyyze)


## 1. HTTP

### B. Configuration


🌞 **Lister les ports en écoute sur la machine**
```ps
[root@web ~]# sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1279,fd=6),("nginx",pid=1278,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1279,fd=7),("nginx",pid=1278,fd=7))
```

🌞 **Ouvrir le port dans le firewall de la machine**
```ps
[root@web ~]# sudo firewall-cmd --permanent --add-port=80/tcp
success
[root@web ~]# sudo firewall-cmd --reload
success
```


### C. Tests client


🌞 **Vérifier que ça a pris effet**

- faites un `ping` vers `sitedefou.tp7.b1`

```ps
baptiste@client:~$ ping sitedefou.tp7.b1
PING sitedefou.tp7.b1 (10.7.1.11) 56(84) bytes of data.
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=1 ttl=64 time=1.31 ms
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=2 ttl=64 time=1.41 ms
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=3 ttl=64 time=1.75 ms
^C
--- sitedefou.tp7.b1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2013ms
rtt min/avg/max/mdev = 1.306/1.488/1.746/0.187 ms
```

- visitez `http://sitedefou.tp7.b1`

```ps
baptiste@client:~$ curl http://sitedefou.tp7.b1
meow !
```

### D. Analyze


🌞 **Capture `tcp_http.pcap`**

*voir fichier joint*


🌞 **Voir la connexion établie**

```ps
baptiste@client:~$ sudo ss -npt | grep 80
ESTAB 0      0         10.7.1.101:46080 34.160.144.191:443  users:(("firefox",pid=6013,fd=109))
ESTAB 0      0         10.7.1.101:43536  34.107.221.82:80   users:(("firefox",pid=6013,fd=113))
ESTAB 0      0         10.7.1.101:43526  34.107.221.82:80   users:(("firefox",pid=6013,fd=105))
🌞ESTAB 0      0         10.7.1.101:60356      🌞10.7.1.11:80   users:(("firefox",pid=6013,fd=66)) 
```

## 2. On rajoute un S

### A. Config


🌞 **Lister les ports en écoute sur la machine**

```
[root@web ~]# sudo ss -lnpt | grep nginx
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1317,fd=7),("nginx",pid=1316,fd=7))
🌞LISTEN 0      511        10.7.1.11:443       0.0.0.0:*    users:(("nginx",pid=1317,fd=6),("nginx",pid=1316,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1317,fd=8),("nginx",pid=1316,fd=8))
```


🌞 **Gérer le firewall**

```ps
[root@web ~]# sudo firewall-cmd --permanent --add-port=443/tcp
success
[root@web ~]# sudo firewall-cmd --permanent --remove-port=80/tcp
success
[root@web ~]# sudo firewall-cmd --reload
success
```

### B. Test test test analyyyze


🌞 **Capture `tcp_https.pcap`**

- **capturer une session TCP complète**

*Voir fichier joint:* ```tcp_https.pcap```



# III. Serveur VPN


## Sommaire

- [III. Serveur VPN](#iii-serveur-vpn)
  - [Sommaire](#sommaire)
  - [1. Install et conf Wireguard](#1-install-et-conf-wireguard)
  - [3. Proofs](#3-proofs)
  - [4. Private service](#4-private-service)


## 1. Install et conf Wireguard

🌞 **Prouvez que vous avez bien une nouvelle carte réseau `wg0`**

```ps
[root@vpn ~]# ip a
[...]
🌞4: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
    link/none
    inet 10.7.200.1/24 scope global wg0
       valid_lft forever preferred_lft forever    
```

🌞 **Déterminer sur quel port écoute Wireguard**

```ps
[root@vpn ~]# sudo ss -lnpu | grep 51820
UNCONN 0      0            0.0.0.0:51820      0.0.0.0:*
UNCONN 0      0               [::]:51820         [::]:*
```

🌞 **Ouvrez ce port dans le firewall**

```ps
[root@vpn ~]# sudo firewall-cmd --permanent --add-port=51820/udp
success
```


## 3. Proofs

🌞 **Ping ping ping !**
- donc `ping 10.7.200.1`:

```ps
baptiste@client:/etc$ ping 10.7.200.1
PING 10.7.200.1 (10.7.200.1) 56(84) bytes of data.
64 bytes from 10.7.200.1: icmp_seq=1 ttl=64 time=3.37 ms
64 bytes from 10.7.200.1: icmp_seq=2 ttl=64 time=9.03 ms
64 bytes from 10.7.200.1: icmp_seq=3 ttl=64 time=9.13 ms
64 bytes from 10.7.200.1: icmp_seq=4 ttl=64 time=5.16 ms
^C
--- 10.7.200.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 3.367/6.670/9.132/2.490 ms

```

🌞 **Capture `ping1_vpn.pcap`**



🌞 **Capture `ping2_vpn.pcap`**



🌞 **Prouvez que vous avez toujours un accès internet**

- `traceroute 1.1.1.1`:

```ps
baptiste@client:~$ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  _gateway (10.7.200.1)  5.076 ms  4.905 ms  7.081 ms
 2  10.7.1.254 (10.7.1.254)  27.929 ms  27.841 ms  27.337 ms
 3  10.0.2.2 (10.0.2.2)  37.663 ms  37.670 ms  37.655 ms
 4  10.0.2.2 (10.0.2.2)  50.524 ms  50.536 ms  50.524 ms
```


