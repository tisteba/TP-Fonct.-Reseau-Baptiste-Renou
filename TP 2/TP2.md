# TP2 : Hey yo tell a neighbor tell a friend


# Sommaire

- [TP2 : Hey yo tell a neighbor tell a friend](#tp2--hey-yo-tell-a-neighbor-tell-a-friend)
- [Sommaire](#sommaire)
- [I. Simplest LAN](#i-simplest-lan)
  - [0. Setup](#0-setup)
  - [1. Quelques pings](#1-quelques-pings)
- [II. Utilisation des ports](#ii-utilisation-des-ports)
  - [0. Intro blabla](#0-intro-blabla)
  - [1. Animal numérique](#1-animal-numérique)
- [III. Analyse de vos applications usuelles](#iii-analyse-de-vos-applications-usuelles)
  - [1. Serveur web](#1-serveur-web)
  - [2. Autres services](#2-autres-services)



# I. Simplest LAN

## 0. Setup

🌞Je n'ai pas de port ethernet, donc je réalise ce tp en connexion SSH avec ma VM.

## 1. Quelques pings

🌞 **Prouvez que votre configuration est effective**

🌞Depuis mon PC:
 ```
PS C:\WINDOWS\system32> Get-NetIPAddress -InterfaceAlias "Ethernet 2"

IPAddress         : fe80::c739:6b6d:184d:2dca%10
InterfaceIndex    : 10
InterfaceAlias    : Ethernet 2
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 192.168.56.1
InterfaceIndex    : 10
InterfaceAlias    : Ethernet 2
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore
```

🌞Pour la VM: 
```
baptiste@baptiste-VirtualBox:~$ ip addr show enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:bd:89:1e brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.106/24 brd 192.168.56.255 scope global dynamic noprefixroute enp0s8
       valid_lft 333sec preferred_lft 333sec
    inet6 fe80::41e7:bda8:8e6b:f2bf/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```


🌞 **Tester que votre LAN + votre adressage IP est fonctionnel**

🌞Depuis mon PC: 
```
PS C:\WINDOWS\system32> ping 192.168.56.106

Envoi d’une requête 'Ping'  192.168.56.106 avec 32 octets de données :
Réponse de 192.168.56.106 : octets=32 temps<1ms TTL=64
Réponse de 192.168.56.106 : octets=32 temps=1 ms TTL=64
Réponse de 192.168.56.106 : octets=32 temps=1 ms TTL=64
Réponse de 192.168.56.106 : octets=32 temps=1 ms TTL=64

Statistiques Ping pour 192.168.56.106:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
```

🌞Depuis la VM:
```
baptiste@baptiste-VirtualBox:~$ ping 192.168.56.1
PING 192.168.56.1 (192.168.56.1) 56(84) bytes of data.
64 bytes from 192.168.56.1: icmp_seq=1 ttl=128 time=0.357 ms
64 bytes from 192.168.56.1: icmp_seq=2 ttl=128 time=1.34 ms
64 bytes from 192.168.56.1: icmp_seq=3 ttl=128 time=1.13 ms
64 bytes from 192.168.56.1: icmp_seq=4 ttl=128 time=1.04 ms
^C
--- 192.168.56.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3028ms
rtt min/avg/max/mdev = 0.357/0.968/1.339/0.368 ms
```


🌞 **Capture de `ping`**

 🌞*Voir le fichier ping.pcap*

# II. Utilisation des ports

## 1. Animal numérique


🌞 **Sur le PC serveur**

On désigne ma VM comme PC serveur, avec le port 2525:
```
🌞baptiste@baptiste-VirtualBox:~$ nc -l 2525
```
🌞 **Sur le PC serveur toujours**
```
baptiste@baptiste-VirtualBox:~$ sudo ss -lnpt
                   
🌞LISTEN    0         1                  0.0.0.0:2525             0.0.0.0:*        users:(("nc",pid=5491,fd=3))                                                   
```
*La commande a été faite alors que j'étais déja connecté*


🌞 **Sur le PC client**

```
 PS C:\Users\grott\Downloads\netcat-1.11> 🌞.\nc.exe 192.168.56.106 2525
```

🌞 **Echangez-vous des messages**

```
PS C:\Users\grott\Downloads\netcat-1.11> ./nc 192.168.56.106 2525

sudo ss -lnpt
🌞test
🌞ça fonctionne !
```

*Les messages s'affichent bien des deux cotés*

🌞 **Utilisez une commande qui permet de voir la connexion en cours**

Sur mon pc (Client):
```
PS C:\WINDOWS\system32> netstat -a -b -n | Select-String nc.exe -Context 1,0

    TCP    192.168.56.1:42687     🌞192.168.56.106:2525    ESTABLISHED
>  [nc.exe]
```

Sur ma VM (serveur):

```
baptiste@baptiste-VirtualBox:~$ sudo ss -lnpt
State     Recv-Q    Send-Q       Local Address:Port        Peer Address:Port    Process                                                                         
LISTEN    0         1                🌞0.0.0.0:2525             0.0.0.0:*        users:(("nc",pid=5491,fd=3))     
```
*On voit bien le port 2525 dans les 2 cas*.


🌞 **Faites une capture Wireshark complète d'un échange**

Voir le fichier joint:
```netcat1.pcap```

🌞 **Inversez les rôles**

*Fais avec VM, les 2 d'un coup.*

# III. Analyse de vos applications usuelles

## 1. Serveur web

🌞 **Utilisez Wireshark pour capturer du trafic HTTP**

voir: ```http.pcap```


## 2. Autres services

🌞 **Pour les 5 applications**

**Pour dofus.com :**

ip= 18.245.175.113

port= 443

fichier: Service1.pcap
```
PS C:\Users\grott\Downloads\netcat-1.11> netstat -anb | findstr "443"
[...]
 🌞TCP    192.168.1.120:13135    18.245.175.113:443     ESTABLISHED
```
---
**Pour Spotify :**

ip=35.186.224.41

port=443

fichier= service2.pcap

```
PS C:\Users\grott\Downloads\netcat-1.11> netstat -ano -b
[...]
 🌞TCP    192.168.1.120:13414    35.186.224.41:443      ESTABLISHED     20064
 [Spotify.exe]

```

---
**Pour topachat.com :**

ip=91.211.166.37

port=443

fichier= service3.pcap

```
PS C:\Users\grott\Downloads\netcat-1.11> netstat -ano -b | findstr :443
[...]
 🌞TCP    192.168.1.120:14030    91.211.166.37:443      ESTABLISHED     18548

```
---
**Pour Steam :**

ip=92.122.218.106

port=80

fichier= service4.pcap

```
PS C:\Users\grott\Downloads\netcat-1.11> netstat -ano -b
[...]
 🌞TCP    192.168.1.120:14119    92.122.218.106:80      ESTABLISHED     11628
 [steam.exe]
```
---

**Pour Discord :**

ip=162.159.135.232

port=443

fichier= service5.pcap

```
PS C:\Users\grott\Downloads\netcat-1.11> netstat -ano -b
[...]
 🌞TCP    192.168.1.120:14415    162.159.135.232:443    ESTABLISHED     17088
 [Discord.exe]
```