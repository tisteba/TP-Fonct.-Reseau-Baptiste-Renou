# TP Reseau 1

## 1. Récolte d'informations
### 🌞 Adresses IP de ma machine:

```
PS C:\Users\grott> ipconfig /all
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wireless-AC 9462
   Adresse physique . . . . . . . . . . . : DE-ED-F5-CD-9E-C6
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::c94c:2bf8:e687:e47%4(préféré)
   🌞Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.198(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : vendredi 27 septembre 2024 13:56:31
   Bail expirant. . . . . . . . . . . . . : samedi 28 septembre 2024 13:56:30
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 81718773
   DUID de client DHCPv6. . . . . . . . : 00-03-00-01-DE-ED-F5-CD-9E-C6
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
           1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ```

   Pas de cartes éthernet

### 🌞 Si t'as un accès internet normal, d'autres infos sont forcément dispos...

-  Adresse ip de la passerelle réseau local:
- affiche l'adresse IP du serveur DNS que connaît mon PC:
- affiche l'adresse IP du serveur DHCP que connaît ton PC: 

``` 
PS C:\Users\grott> ipconfig /all

 Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wireless-AC 9462
   Adresse physique . . . . . . . . . . . : DE-ED-F5-CD-9E-C6
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::c94c:2bf8:e687:e47%4(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.198(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : vendredi 27 septembre 2024 13:56:31
   Bail expirant. . . . . . . . . . . . . : samedi 28 septembre 2024 13:56:30
   🌞 Passerelle par défaut. . . . . . . . . : 10.33.79.254
   🌞Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 81718773
   DUID de client DHCPv6. . . . . . . . : 00-03-00-01-DE-ED-F5-CD-9E-C6
  🌞 Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ```

-  BONUS : Détermine s'il y a un pare-feu actif sur ta machine:

```
PS C:\Users\grott> Get-NetFirewallProfile | Select-Object Name, Enabled

Name    Enabled
----    -------
Domain     True
Private    True
Public     True
```

- Si oui, je veux aussi voir une commande pour lister les règles du pare-feu

```
PS C:\WINDOWS\system32> Get-NetFirewallRule -all


Name                          : SNMPTRAP-In-UDP
DisplayName                   : Service d’interruption SNMP (UDP entrant)
Description                   : Règle de trafic entrant pour que le service d’interruption SNMP autorise les interruptions SNMP. [UDP 162]
DisplayGroup                  : Interruption SNMP
Group                         : @firewallapi.dll,-50323
Enabled                       : False
Profile                       : Private, Public
Platform                      : {}
Direction                     : Inbound
Action                        : Allow
EdgeTraversalPolicy           : Block
LooseSourceMapping            : False
LocalOnlyMapping              : False
Owner                         :
PrimaryStatus                 : OK
Status                        : La règle a été analysée à partir de la banque. (65536)
EnforcementStatus             : NotApplicable
PolicyStoreSource             : PersistentStore
PolicyStoreSourceType         : Local
RemoteDynamicKeywordAddresses :
PolicyAppId                   :

Name                          : SNMPTRAP-In-UDP-NoScope
DisplayName                   : Service d’interruption SNMP (UDP entrant)
Description                   : Règle de trafic entrant pour que le service d’interruption SNMP autorise les interruptions SNMP. [UDP 162]
DisplayGroup                  : Interruption SNMP
Group                         : @firewallapi.dll,-50323
Enabled                       : False
Profile                       : Domain
Platform                      : {}
Direction                     : Inbound
Action                        : Allow
EdgeTraversalPolicy           : Block
LooseSourceMapping            : False
LocalOnlyMapping              : False
Owner                         :
PrimaryStatus                 : OK
Status                        : La règle a été analysée à partir de la banque. (65536)
EnforcementStatus             : NotApplicable
PolicyStoreSource             : PersistentStore
PolicyStoreSourceType         : Local
RemoteDynamicKeywordAddresses :
PolicyAppId                   :

[...]
```


## II. Utiliser le réseau

### 🌞 Envoie un ping vers...

- Toi-même !
```
PS C:\Users\grott> ping 10.33.77.198

Envoi d’une requête 'Ping'  10.33.77.198 avec 32 octets de données :
Réponse de 10.33.77.198 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.198 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.198 : octets=32 temps<1ms TTL=128
Réponse de 10.33.77.198 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 10.33.77.198:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```


- vers l'adresse IP 127.0.0.1:
```
PS C:\Users\grott> ping 127.0.0.1

Envoi d’une requête 'Ping'  127.0.0.1 avec 32 octets de données :
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 127.0.0.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

### 🌞 On continue avec ping. Envoie un ping vers...
- Ma passerelle

```
PS C:\Users\grott> ping 10.33.79.254

Envoi d’une requête 'Ping'  10.33.79.254 avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.

Statistiques Ping pour 10.33.79.254:
    Paquets : envoyés = 2, reçus = 0, perdus = 2 (perte 100%),
Ctrl+C
``` 

- un(e) pote/intervenant sur le réseau:
```
PS C:\Users\grott> ping 10.33.66.78

Envoi d’une requête 'Ping'  10.33.66.78 avec 32 octets de données :
Réponse de 10.33.66.78 : octets=32 temps=39 ms TTL=64
Réponse de 10.33.66.78 : octets=32 temps=42 ms TTL=64
Réponse de 10.33.66.78 : octets=32 temps=39 ms TTL=64
Réponse de 10.33.66.78 : octets=32 temps=41 ms TTL=64

Statistiques Ping pour 10.33.66.78:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 39ms, Maximum = 42ms, Moyenne = 40ms
```

- Un site internet:
```
PS C:\Users\grott> ping www.dofus.com

Envoi d’une requête 'ping' sur d3vwc19tz5sjxb.cloudfront.net [18.245.175.120] avec 32 octets de données :
Réponse de 18.245.175.120 : octets=32 temps=23 ms TTL=248
Réponse de 18.245.175.120 : octets=32 temps=117 ms TTL=248
Réponse de 18.245.175.120 : octets=32 temps=116 ms TTL=248
Réponse de 18.245.175.120 : octets=32 temps=14 ms TTL=248

Statistiques Ping pour 18.245.175.120:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 14ms, Maximum = 117ms, Moyenne = 67ms
```

### 🌞 Faire une requête DNS à la main
Effectue une requête DNS à la main pour obtenir l'adresse IP qui correspond aux noms de domaine suivants:
```
www.thinkerview.com
www.wikileaks.org
www.torproject.org
```

```
PS C:\Users\grott> nslookup www.thinkerview.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.thinkerview.com
Addresses:  2a06:98c1:3121::7
          2a06:98c1:3120::7
          188.114.96.7
          188.114.97.7
```
```
PS C:\Users\grott> nslookup www.wikileaks.org
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    wikileaks.org
Addresses:  80.81.248.21
          51.159.197.136
Aliases:  www.wikileaks.org
```
```
PS C:\Users\grott> nslookup www.torproject.org
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    www.torproject.org
Addresses:  2620:7:6002:0:466:39ff:fe7f:1826
          2a01:4f8:fff0:4f:266:37ff:feae:3bbc
          2a01:4f9:c010:19eb::1
          2620:7:6002:0:466:39ff:fe32:e3dd
          2a01:4f8:fff0:4f:266:37ff:fe2c:5d19
          204.8.99.144
          95.216.163.36
          116.202.120.166
          116.202.120.165
          204.8.99.146
```


## III. Sniffer le réseau

On ne remarque rien dans Wireshark lorsque l'on se ping sois même.

## IV. Network scanning et adresses IP
### 🌞 Effectue un scan du réseau auquel tu es connecté

```
PS C:\Users\grott> nmap -sn -PR 10.33.64.0/20
Starting Nmap 7.95 ( https://nmap.org ) at 2024-09-27 17:25 Paris, Madrid (heure dÆÚtÚ)
Nmap scan report for 10.33.66.78
Host is up (0.12s latency).
MAC Address: E4:B3:18:48:36:68 (Intel Corporate)
Nmap scan report for 10.33.69.82
Host is up (0.70s latency).
MAC Address: 50:A6:D8:A9:55:02 (Apple)
Nmap scan report for 10.33.69.136
Host is up (0.097s latency).
MAC Address: BA:E9:DD:6B:50:32 (Unknown)
Nmap scan report for 10.33.69.137
Host is up (0.64s latency).
MAC Address: 74:A6:CD:A1:5D:5C (Apple)
Nmap scan report for 10.33.69.157
Host is up (0.27s latency).
MAC Address: 3A:71:A3:B0:D6:76 (Unknown)
Nmap scan report for 10.33.70.32
Host is up (0.051s latency).
MAC Address: 9C:FC:E8:37:07:94 (Intel Corporate)
Nmap scan report for 10.33.70.41
Host is up (0.0070s latency).
MAC Address: CE:71:C6:DF:63:76 (Unknown)
Nmap scan report for 10.33.70.82
Host is up (0.93s latency).
MAC Address: F8:FF:C2:02:5D:DA (Apple)
Nmap scan report for 10.33.71.156
Host is up (0.14s latency).
MAC Address: FA:F1:D8:3F:F4:CF (Unknown)
Nmap scan report for 10.33.71.182
Host is up (0.068s latency).
MAC Address: 5C:E9:1E:B3:EB:86 (Apple)
Nmap scan report for 10.33.71.187
Host is up (0.082s latency).
Nmap done: 4096 IP addresses (178 hosts up) scanned in 160.40 seconds
```
### 🌞 Changer d'adresse IP
```PS C:\Users\grott> ipconfig

Configuration IP de Windows


Carte Ethernet Ethernet 2 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::c739:6b6d:184d:2dca%10
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.56.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :

Carte réseau sans fil Connexion au réseau local* 3 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte réseau sans fil Connexion au réseau local* 12 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::c94c:2bf8:e687:e47%4
  🌞 Adresse IPv4. . . . . . . . . . . . . .: 10.33.70.117
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   ```

   


