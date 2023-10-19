# TP1 : Maîtrise réseau du poste

# I. Basics

☀️ **Carte réseau WiFi**

```
PS C:\Users\matas> ipconfig /all
[...]
Carte réseau sans fil Wi-Fi :
Adresse physique . . . . . . . . . . . : D4-54-8B-A5-2C-DD
Adresse IPv4. . . . . . . . . . . . . .: 10.33.76.200(préféré)
```

```
/20
Masque de sous-réseau. . . . . . . . . : 255.255.240.0
```

☀️ **Déso pas déso**

Adresse de réseau : 10.33.64.0
Adresse de broadcast : 10.33.79.255
Nombre d'adresses IP disponibles : 4094


☀️ **Hostname**

```
PS C:\Users\matas> hostname
Mata_milo
```

☀️ **Passerelle du réseau**

```
PS C:\Users\matas> ipconfig /all
Passerelle par défaut. . . . . . . . . : 10.33.79.254
```

```
PS C:\Users\matas> arp -a
[...]
Adresse Internet      Adresse physique      Type
[...]
 10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```

☀️ **Serveur DHCP et DNS**
```
PS C:\Users\matas> ipconfig /all
[...]
Carte réseau sans fil Wi-Fi :
[...]
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
```

☀️ **Table de routage**

```
PS C:\Users\matas> netstat -rn
[...]
IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0     10.33.79.254     10.33.76.200     30
```
# II. Go further

☀️ **Hosts ?**

```
PS C:\Users\matas> cat C:\Windows\System32\Drivers\etc\hosts
[...]
1.1.1.1         b2.hello.vous
```
```
PS C:\Users\matas> ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=10 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=11 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=11 ms TTL=57

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 3, reçus = 3, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 10ms, Maximum = 11ms, Moyenne = 10ms
```

☀️ **Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...**

```
PS C:\Users\matas> netstat -n -b -a | Select-String 77.136.192.86 -Context 0,1

>   UDP    0.0.0.0:52790          77.136.192.86:443
   [chrome.exe]
```

[Capture wireshark](./ip_server_youtube.pcapng)

☀️ **Requêtes DNS**

```
PS C:\Users\matas> nslookup ynov.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::681a:be9
          2606:4700:20::ac43:4ae2
          104.26.11.233
          104.26.10.233
          172.67.74.226
```

```
PS C:\Users\matas>
                   nslookup 174.43.238.89
Serveur :   dns.google
Address:  8.8.8.8

Nom :    89.sub-174-43-238.myvzw.com
Address:  174.43.238.89
```

☀️ **Hop hop hop**

```
PS C:\Users\matas> tracert www.ynov.com

Détermination de l’itinéraire vers www.ynov.com [104.26.11.233]
avec un maximum de 30 sauts :

  1     4 ms     2 ms     1 ms  10.33.79.254
  2     3 ms     1 ms     1 ms  145.117.7.195.rev.sfr.net [195.7.117.145]
  3     2 ms     1 ms     3 ms  237.195.79.86.rev.sfr.net [86.79.195.237]
  4     3 ms     2 ms     2 ms  196.224.65.86.rev.sfr.net [86.65.224.196]
  5    12 ms    98 ms    10 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  6    10 ms    10 ms     9 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  7    10 ms    10 ms    10 ms  141.101.67.48
  8    11 ms    11 ms    10 ms  172.71.124.4
  9    10 ms    10 ms    10 ms  104.26.11.233

Itinéraire déterminé.
```

☀️ **IP publique**

```
PS C:\Users\matas> curl ifconfig.me | Select-String 195.7.117.146

195.7.117.146
```

☀️ **Scan réseau**

```
PS C:\Users\matas> arp -a

[...]
Interface : 10.33.76.200 --- 0x17
  Adresse Internet      Adresse physique      Type
  10.33.68.234          bc-f4-d4-c0-b7-2f     dynamique
  10.33.69.1            08-d2-3e-35-00-a2     dynamique
  10.33.70.195          f0-b6-1e-0a-d0-34     dynamique
  10.33.71.185          84-1b-77-fd-5c-a0     dynamique
  10.33.71.244          10-68-38-f9-61-f8     dynamique
  10.33.79.197          90-e8-68-d3-6f-8f     dynamique
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

# III. Le requin

☀️ **Capture ARP**

[Capture Arp](./arp.pcap)

J'ai utilisé le filtre "arp".

☀️ **Capture DNS**

```
PS C:\Users\matas> nslookup www.youtube.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    youtube-ui.l.google.com
Addresses:  2a00:1450:4002:411::200e
          2a00:1450:4002:414::200e
          2a00:1450:4002:416::200e
          2a00:1450:4002:402::200e
          142.250.203.110
          216.58.215.238
          172.217.168.46
          172.217.168.78
Aliases:  www.youtube.com
```

J'ai utilisé le filtre "dns".

[Capture DNS](./dns.pcap)

☀️ **Capture TCP**

[Capture TCP](./tcp.pcap)

J'ai utilisé le filtre "tcp".