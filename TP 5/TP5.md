# TP5 : Un ptit LAN à nous



## Sommaire

- [TP5 : Un ptit LAN à nous](#tp5--un-ptit-lan-à-nous)
  - [Sommaire](#sommaire)
- [I. Setup](#i-setup)
- [II. Accès internet pour tous](#ii-accès-internet-pour-tous)
  - [1. Accès internet routeur](#1-accès-internet-routeur)
  - [2. Accès internet clients](#2-accès-internet-clients)
- [III. Serveur SSH](#iii-serveur-ssh)
- [IV. Serveur DHCP](#iv-serveur-dhcp)
    - [A. Installation et configuration du serveur DHCP](#a-installation-et-configuration-du-serveur-dhcp)
    - [B. Test avec un nouveau client](#b-test-avec-un-nouveau-client)
    - [C. Consulter le bail DHCP](#c-consulter-le-bail-dhcp)



# I. Setup

### ☀️ **Uniquement avec des commandes, prouvez-que :**

#### vous avez bien configuré les adresses IP demandées (un ip a suffit hein)
---
Sur mon pc:
```ps 
PS C:\WINDOWS\system32> ipconfig

Configuration IP de Windows
[...]

Carte Ethernet Ethernet 4 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::6f18:8f06:ca91:fdf%54
  ☀️ Adresse IPv4. . . . . . . . . . . . . .: 10.5.1.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
   ```
---
   Client 1:
   ```PS
   baptiste@client1:~$ ip a
[...]
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:8a:12:54 brd ff:ff:ff:ff:ff:ff
   ☀️ inet 10.5.1.11/24 brd 10.5.1.255 scope global enp0s8
[...]
```
---
Client 2:
```ps
baptiste@client2:~$ ip a
[...]
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:aa:5a:ff brd ff:ff:ff:ff:ff:ff
   ☀️ inet 10.5.1.12/24 brd 10.5.1.255 scope global enp0s8
[...]
```
---
Routeur:
```ps
[tisteba@routeur ~]$ ip a
[...]
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:3d:0e:ac brd ff:ff:ff:ff:ff:ff
   ☀️ inet 10.5.1.254/24 brd 10.5.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe3d:eac/64 scope link
       valid_lft forever preferred_lft forever
```

**vous avez bien configuré les hostnames demandés**

---
client 1:
```ps
baptiste@client1:~$ hostname
☀️client1.tp5.b1
```
---
client 2:
```ps
baptiste@client2:~$ hostname
☀️client2.tp5.b1
```
---
routeur:
```ps
[tisteba@routeur ~]$ hostname
☀️routeur.tp5.b1
```

**Tout le monde peut se ping au sein du réseau 10.5.1.0/24**
depuis mon pc:
```ps
PS C:\WINDOWS\system32> ping 10.5.1.11

☀️Envoi d’une requête 'Ping'  10.5.1.11 avec 32 octets de données :
Réponse de 10.5.1.11 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.11 : octets=32 temps=1 ms TTL=64
Réponse de 10.5.1.11 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.11 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 10.5.1.11:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
PS C:\WINDOWS\system32> ping 10.5.1.12

☀️Envoi d’une requête 'Ping'  10.5.1.12 avec 32 octets de données :
Réponse de 10.5.1.12 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.12 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.12 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.12 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 10.5.1.12:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
PS C:\WINDOWS\system32> ping 10.5.1.254

☀️Envoi d’une requête 'Ping'  10.5.1.254 avec 32 octets de données :
Réponse de 10.5.1.254 : octets=32 temps=1 ms TTL=64
Réponse de 10.5.1.254 : octets=32 temps=1 ms TTL=64
Réponse de 10.5.1.254 : octets=32 temps<1ms TTL=64
Réponse de 10.5.1.254 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 10.5.1.254:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
```

depuis le routeur:
```ps
[tisteba@routeur ~]$ ping 10.5.1.1
☀️PING 10.5.1.1 (10.5.1.1) 56(84) bytes of data.
64 bytes from 10.5.1.1: icmp_seq=1 ttl=128 time=0.405 ms
64 bytes from 10.5.1.1: icmp_seq=2 ttl=128 time=0.411 ms
64 bytes from 10.5.1.1: icmp_seq=3 ttl=128 time=1.03 ms
64 bytes from 10.5.1.1: icmp_seq=4 ttl=128 time=0.579 ms
^C
--- 10.5.1.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3038ms
rtt min/avg/max/mdev = 0.405/0.606/1.030/0.254 ms
☀️[tisteba@routeur ~]$ ping 10.5.1.11
PING 10.5.1.11 (10.5.1.11) 56(84) bytes of data.
64 bytes from 10.5.1.11: icmp_seq=1 ttl=64 time=1.06 ms
64 bytes from 10.5.1.11: icmp_seq=2 ttl=64 time=1.42 ms
64 bytes from 10.5.1.11: icmp_seq=3 ttl=64 time=1.23 ms
64 bytes from 10.5.1.11: icmp_seq=4 ttl=64 time=1.43 ms
64 bytes from 10.5.1.11: icmp_seq=5 ttl=64 time=0.992 ms
^C
--- 10.5.1.11 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4008ms
rtt min/avg/max/mdev = 0.992/1.227/1.426/0.178 ms
☀️[tisteba@routeur ~]$ ping 10.5.1.12
PING 10.5.1.12 (10.5.1.12) 56(84) bytes of data.
64 bytes from 10.5.1.12: icmp_seq=1 ttl=64 time=1.04 ms
64 bytes from 10.5.1.12: icmp_seq=2 ttl=64 time=1.28 ms
64 bytes from 10.5.1.12: icmp_seq=3 ttl=64 time=1.12 ms
64 bytes from 10.5.1.12: icmp_seq=4 ttl=64 time=1.44 ms
^C
--- 10.5.1.12 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 1.039/1.218/1.436/0.153 ms
```
On peut bien ping chaques VM depuis mon pc et la vm routeur, et on a une répons des clients à chaques fois.

# II. Accès internet pour tous


## 1. Accès internet routeur

☀️ **Déjà, prouvez que le routeur a un accès internet**

```ps
[tisteba@routeur ~]$ ping dofus.com
PING dofus.com (65.9.95.99) 56(84) bytes of data.
64 bytes from server-65-9-95-99.prg50.r.cloudfront.net (65.9.95.99): icmp_seq=1 ttl=235 time=84.4 ms
64 bytes from server-65-9-95-99.prg50.r.cloudfront.net (65.9.95.99): icmp_seq=2 ttl=235 time=61.6 ms
64 bytes from server-65-9-95-99.prg50.r.cloudfront.net (65.9.95.99): icmp_seq=3 ttl=235 time=70.1 ms
^X^C
--- dofus.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 61.618/72.039/84.434/9.418 ms
```

☀️ **Activez le routage**

```ps
[root@routeur ~]# sudo firewall-cmd --add-masquerade --permanent
success
[root@routeur ~]# sudo firewall-cmd --reload
success
```


## 2. Accès internet clients


☀️ **Prouvez que les clients ont un accès internet**


client 1:
```
baptiste@client1:~$ ping dofus.com
PING dofus.com (99.86.4.77) 56(84) bytes of data.
64 bytes from server-99-86-4-77.fra6.r.cloudfront.net (99.86.4.77): icmp_seq=1 ttl=237 time=86.0 ms
64 bytes from server-99-86-4-77.fra6.r.cloudfront.net (99.86.4.77): icmp_seq=2 ttl=237 time=81.5 ms
64 bytes from server-99-86-4-77.fra6.r.cloudfront.net (99.86.4.77): icmp_seq=3 ttl=237 time=74.2 ms
^C
--- dofus.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2032ms
rtt min/avg/max/mdev = 74.171/80.552/85.982/4.868 ms
```

---
client 2
```
baptiste@client2:/etc/netplan$ ping dofus.com
PING dofus.com (18.239.50.78) 56(84) bytes of data.
64 bytes from server-18-239-50-78.ams58.r.cloudfront.net (18.239.50.78): icmp_seq=1 ttl=239 time=81.0 ms
64 bytes from server-18-239-50-78.ams58.r.cloudfront.net (18.239.50.78): icmp_seq=2 ttl=239 time=64.2 ms
64 bytes from server-18-239-50-78.ams58.r.cloudfront.net (18.239.50.78): icmp_seq=3 ttl=239 time=54.3 ms
^C
--- dofus.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2014ms
rtt min/avg/max/mdev = 54.323/66.505/81.021/11.023 ms
```

☀️ **Montrez-moi le contenu final du fichier de configuration de l'interface réseau**
```
baptiste@client2:/etc/netplan$ sudo cat 01-network.yaml 
[sudo] password for baptiste: 

network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s8:
      dhcp4: no
      addresses: [10.5.1.12/24]
      gateway4: 10.5.1.254
      nameservers:
        addresses: [1.1.1.1,8.8.8.8]
```

# III. Serveur SSH

☀️ **Sur `routeur.tp5.b1`, déterminer sur quel port écoute le serveur SSH**

```ps
[root@routeur ~]# sudo ss -lnpt | grep 22
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=707,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=707,fd=4))
```
port numero 22


☀️ **Sur `routeur.tp5.b1`, vérifier que ce port est bien ouvert**

```
[root@routeur ~]# sudo firewall-cmd --permanent --add-port=22/tcp
success
[root@routeur ~]# sudo firewall-cmd --reload
success
```
# IV. Serveur DHCP

### A. Installation et configuration du serveur DHCP


☀️ **Installez et configurez un serveur DHCP sur la machine `routeur.tp5.b1`**

- je veux toutes les commandes réalisées:
```
[root@routeur ~]# sudo dnf install dhcp-server
[root@routeur ~]# sudo nano /etc/dhcp/dhcpd.conf
[root@routeur ~]# firewall-cmd --add-service=dhcp

success
[root@routeur ~]# firewall-cmd --runtime-to-permanent

success 
[root@routeur ~]# sudo firewall-cmd --reload
success
[root@routeur ~]# sudo systemctl enable --now dhcpd
```


- et le contenu du fichier de configuration:
```        
default-lease-time 2592000;

max-lease-time 3888000;

authoritative;

subnet 10.5.1.0 netmask 255.255.255.0 {

    ☀️range dynamic-bootp 10.5.1.137 10.5.1.237;

    ☀️option routers 10.5.1.254;

    option subnet-mask 255.255.255.0;

    ☀️option domain-name-servers 1.1.1.1;

}
```

### B. Test avec un nouveau client

☀️ **Créez une nouvelle machine client `client3.tp5.b1`**

- définissez son *hostname*
```
baptiste@baptiste-VirtualBox:~$ hostnamectl set-hostname client3.tp5.b1
baptiste@baptiste-VirtualBox:~$ hostname
client3.tp5.b1
```
- définissez une IP en DHCP
On modifie le fichier ```01-netcfg.yaml``` tel que:
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s8:
      dhcp4: yes
```
```
baptiste@client3:~$ sudo netplan apply 
```
- vérifiez que c'est bien une adresse IP entre `.137` et `.237`

```
baptiste@client3:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    ☀️ link/ether 08:00:27:a6:ac:bb brd ff:ff:ff:ff:ff:ff
    ☀️inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s8
       valid_lft 2591479sec preferred_lft 2591479sec
    inet6 fe80::a00:27ff:fea6:acbb/64 scope link 
       valid_lft forever preferred_lft forever

```

- prouvez qu'il a immédiatement un accès internet

```
baptiste@client3:~$ ping dofus.com
PING dofus.com (18.239.50.22) 56(84) bytes of data.
64 bytes from server-18-239-50-22.ams58.r.cloudfront.net (18.239.50.22): icmp_seq=1 ttl=239 time=69.4 ms
64 bytes from server-18-239-50-22.ams58.r.cloudfront.net (18.239.50.22): icmp_seq=2 ttl=239 time=66.7 ms
64 bytes from server-18-239-50-22.ams58.r.cloudfront.net (18.239.50.22): icmp_seq=3 ttl=239 time=68.0 ms
^C
--- dofus.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2033ms
rtt min/avg/max/mdev = 66.674/68.032/69.447/1.132 ms

```

### C. Consulter le bail DHCP

☀️ **Consultez le *bail DHCP* qui a été créé pour notre client**


- afficher le contenu du fichier qui contient les *baux DHCP*
```ps
authoring-byte-order little-endian;

server-duid "\000\001\000\001.\242mH\010\000'=\016\254";

☀️lease 10.5.1.137 {
  starts 0 2024/10/20 11:27:19;
  ends 2 2024/11/19 11:27:19;
  cltt 0 2024/10/20 11:27:19;
  binding state active;
  next binding state free;
  rewind binding state free;
 ☀️ hardware ethernet 08:00:27:a6:ac:bb;
  uid "\377\257\201\217}\000\002\000\000\253\021z|\207i\204\367\367K";
}
```


☀️ **Confirmez qu'il s'agit bien de la bonne adresse MAC**

```
baptiste@client3:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
   ☀️ link/ether 08:00:27:a6:ac:bb brd ff:ff:ff:ff:ff:ff
   ☀️ inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s8
       valid_lft 2591324sec preferred_lft 2591324sec
    inet6 fe80::a00:27ff:fea6:acbb/64 scope link 
       valid_lft forever preferred_lft forever

```