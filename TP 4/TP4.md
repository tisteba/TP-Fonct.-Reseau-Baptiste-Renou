# TP4 : DHCP et accès internet


## Sommaire

- [TP4 : DHCP et accès internet](#tp4--dhcp-et-accès-internet)
  - [Sommaire](#sommaire)
- [I. DHCP](#i-dhcp)
  - [1. Les mains dans le capot](#1-les-mains-dans-le-capot)



# I. DHCP
## 1. Les mains dans le capot

☀️ **Capturez un échange DHCP complet**

J'ai utilisé `ipconfig /release` puis `ipconfig /renew
`

*Voir la capture `dhcp.pcap`*


☀️ **Directement dans Wireshark, vous pouvez voir toutes les infos que vous donne  le serveur DHCP**

- adresse IP proposée:

`Your (client) IP adress: 192.168.1.120`

valeur: `c0 a8 01 78`

- serveur DNS indiqué

```
Option: (6) Domain Name server
Length: 4
192.168.1.254 
```
valeur: ` 06 04 c0 a8 01 fe`


- passerelle du réseau

```
Option: (3) Router
Length: 4
Router 192.168.1.254
```

valeur: `03 04 c0 a8 01 fe`