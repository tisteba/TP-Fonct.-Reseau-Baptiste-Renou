# TP3 : 32°13'34"N 95°03'27"W


## Sommaire

- [TP3 : 32°13'34"N 95°03'27"W](#tp3--321334n-950327w)
  - [Sommaire](#sommaire)
- [I. ARP basics](#i-arp-basics)
- [II. ARP dans un réseau local](#ii-arp-dans-un-réseau-local)
  - [1. Basics](#1-basics)
  - [2. ARP](#2-arp)
  - [3. Bonus : ARP poisoning](#3-bonus--arp-poisoning)


# I. ARP basics


☀️ **Avant de continuer...**

```ps
PS C:\WINDOWS\system32> ipconfig /all
[...]
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wireless-AC 9462
   ☀️Adresse physique . . . . . . . . . . . : DE-ED-F5-CD-9E-C6
   ```

☀️ **Affichez votre table ARP**
```ps
PS C:\WINDOWS\system32> arp -a

Interface : 10.33.77.198 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.68.254          9c-b6-d0-05-18-db     dynamique
  10.33.70.26           28-d0-ea-3e-bc-14     dynamique
  10.33.72.50           f4-4e-e3-bf-d0-e2     dynamique
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0xa
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  ```


☀️ **Déterminez l'adresse MAC de la passerelle du réseau de l'école**

```ps
PS C:\WINDOWS\system32> arp -a

Interface : 10.33.77.198 --- 0x4
  Adresse Internet      Adresse physique      Type
[...]
☀️10.33.79.254      ☀️ 7c-5a-1c-d3-d8-76     dynamique
[...]
  ```
☀️ **Supprimez la ligne qui concerne la passerelle**
```ps
PS C:\WINDOWS\system32> arp -d 10.33.79.254
```


☀️ **Prouvez que vous avez supprimé la ligne dans la table ARP**

```ps 
PS C:\WINDOWS\system32> arp -d 10.33.79.254
PS C:\WINDOWS\system32> arp -a

Interface : 10.33.77.198 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.67.218          a8-7e-ea-46-e7-8b     dynamique
  10.33.70.26           28-d0-ea-3e-bc-14     dynamique
  10.33.72.50           f4-4e-e3-bf-d0-e2     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0xa
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  ```
  *La ligne 10.33.79.254 a bien disparu.*

☀️ **Wireshark**

Voir le fichier de capture ```arp1.pcap```

# II. ARP dans un réseau local

## 1. Basics

☀️ **Déterminer**
```ps
PS C:\WINDOWS\system32> ipconfig /all
[...]
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wireless-AC 9462
   Adresse physique . . . . . . . . . . . : 3A-8B-BB-D2-40-03
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::4bd5:a8a4:3379:fdfe%4(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.28.149(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Bail obtenu. . . . . . . . . . . . . . : mardi 8 octobre 2024 16:16:06
   Bail expirant. . . . . . . . . . . . . : mardi 8 octobre 2024 17:16:04
   ☀️Passerelle par défaut. . . . . . . . . : 192.168.28.249
   ```
   ```ps
    C:\WINDOWS\system32> arp -a

Interface : 192.168.28.149 --- 0x4
  Adresse Internet      Adresse physique      Type
  ☀️192.168.28.249        6a-b2-90-ea-ae-be     dynamique
[...]
  ```

☀️ **DIY**

```ps
PS C:\WINDOWS\system32> ipconfig

Configuration IP de Windows
[...]

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::4bd5:a8a4:3379:fdfe%4
   ☀️Adresse IPv4. . . . . . . . . . . . . .: 192.168.28.66
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . : 192.168.28.249
   ```
   *l'ip a bien changé*

☀️ **Pingz !**

ping Johann
```ps
PS C:\WINDOWS\system32> ping 192.168.28.33

Envoi d’une requête 'Ping'  192.168.28.33 avec 32 octets de données :
Réponse de 192.168.28.33 : octets=32 temps=348 ms TTL=128
Réponse de 192.168.28.33 : octets=32 temps=50 ms TTL=128
Réponse de 192.168.28.33 : octets=32 temps=71 ms TTL=128
Réponse de 192.168.28.33 : octets=32 temps=189 ms TTL=128

Statistiques Ping pour 192.168.28.33:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 50ms, Maximum = 348ms, Moyenne = 164ms
```
---

Ping Anthony
```ps
PS C:\WINDOWS\system32> ping 192.168.28.1

Envoi d’une requête 'Ping'  192.168.28.1 avec 32 octets de données :
Réponse de 192.168.28.1 : octets=32 temps=10 ms TTL=128
Réponse de 192.168.28.1 : octets=32 temps=27 ms TTL=128
Réponse de 192.168.28.1 : octets=32 temps=10 ms TTL=128
Réponse de 192.168.28.1 : octets=32 temps=10 ms TTL=128

Statistiques Ping pour 192.168.28.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 10ms, Maximum = 27ms, Moyenne = 14ms
```
---

Ping Adrien 
```ps
PS C:\WINDOWS\system32> ping 192.168.28.69

Envoi d’une requête 'Ping'  192.168.28.69 avec 32 octets de données :
Réponse de 192.168.28.69 : octets=32 temps=24 ms TTL=128
Réponse de 192.168.28.69 : octets=32 temps=9 ms TTL=128
Réponse de 192.168.28.69 : octets=32 temps=13 ms TTL=128
Réponse de 192.168.28.69 : octets=32 temps=12 ms TTL=128

Statistiques Ping pour 192.168.28.69:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 9ms, Maximum = 24ms, Moyenne = 14ms
```
---
*Vérification de l'accés à internet :*
```ps
PS C:\WINDOWS\system32> ping dofus.com

Envoi d’une requête 'ping' sur dofus.com [18.245.175.113] avec 32 octets de données :
Réponse de 18.245.175.113 : octets=32 temps=74 ms TTL=247
Réponse de 18.245.175.113 : octets=32 temps=68 ms TTL=247
Réponse de 18.245.175.113 : octets=32 temps=42 ms TTL=247
Réponse de 18.245.175.113 : octets=32 temps=47 ms TTL=247

Statistiques Ping pour 18.245.175.113:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 42ms, Maximum = 74ms, Moyenne = 57ms
```


## 2. ARP

☀️ **Affichez votre table ARP !**

```ps
    Minimum = 9ms, Maximum = 24ms, Moyenne = 14ms
PS C:\WINDOWS\system32> arp -a

Interface : 192.168.28.66 --- 0x4
  Adresse Internet      Adresse physique      Type
 ☀️ 192.168.28.1          d0-65-78-a2-a1-20     dynamique
 ☀️192.168.28.33         a8-41-f4-ea-29-b5     dynamique
 ☀️ 192.168.28.69         58-96-71-9c-c7-6e     dynamique
  ```

➜ **Wireshark that**


☀️ **Capture arp2.pcap**

Voir la capture ```arp2.pcap```

## 3. Bonus : ARP poisoning

**(je ne suis pas en cyber)**

⭐ **Empoisonner la table ARP de l'un des membres de votre réseau**



⭐ **Mettre en place un MITM**


