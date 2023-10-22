# TP2 : Environnement virtuel

# I. Topologie réseau

☀️ Sur **`node1.lan1.tp2`**

```
[matastral@node1-lan1 sysconfig]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:49:f3:c8 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe49:f3c8/64 scope link
       valid_lft forever preferred_lft forever
```

```
[matastral@node1-lan1 sysconfig]$ ip n s
10.1.1.254 dev enp0s8 lladdr 08:00:27:ca:b7:03 STALE
10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:08 REACHABLE
```

```
[matastral@node1-lan1 sysconfig]$ ping 10.1.2.12
PING 10.1.2.12 (10.1.2.12) 56(84) bytes of data.
64 bytes from 10.1.2.12: icmp_seq=1 ttl=63 time=1.24 ms
64 bytes from 10.1.2.12: icmp_seq=2 ttl=63 time=0.653 ms
^C
--- 10.1.2.12 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.653/0.945/1.238/0.292 ms
```

```
[matastral@node1-lan1 sysconfig]$ traceroute 10.1.2.12
traceroute to 10.1.2.12 (10.1.2.12), 30 hops max, 60 byte packets
 1  _gateway (10.1.1.254)  1.354 ms  1.324 ms  1.310 ms
 2  10.1.2.12 (10.1.2.12)  2.734 ms !X  2.810 ms !X  2.131 ms !X
```

# II. Interlude accès internet

☀️ **Sur `router.tp2`**

```
[matastral@router ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=56 time=14.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=56 time=15.6 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 14.112/14.836/15.560/0.724 ms
[matastral@router ~]$ ping google.com
PING google.com (142.250.179.78) 56(84) bytes of data.
64 bytes from par21s19-in-f14.1e100.net (142.250.179.78): icmp_seq=1 ttl=113 time=26.5 ms
64 bytes from par21s19-in-f14.1e100.net (142.250.179.78): icmp_seq=2 ttl=113 time=26.3 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 26.291/26.385/26.479/0.094 ms
```

☀️ **Accès internet LAN1 et LAN2**

```
[matastral@node2-lan1 ~]$ cat /etc/sysconfig/network
# Created by anaconda
GATEWAY=10.1.1.254
```

```
[matastral@node2-lan1 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=55 time=16.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=55 time=16.2 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 16.192/16.448/16.705/0.256 ms
[matastral@node2-lan1 ~]$ ping google.com
PING google.com (216.58.214.174) 56(84) bytes of data.
64 bytes from mad01s26-in-f14.1e100.net (216.58.214.174): icmp_seq=1 ttl=55 time=23.9 ms
64 bytes from par10s42-in-f14.1e100.net (216.58.214.174): icmp_seq=2 ttl=55 time=23.7 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1031ms
rtt min/avg/max/mdev = 23.728/23.832/23.937/0.104 ms
```

# III. Services réseau

## 1. DHCP

☀️ **Sur `dhcp.lan1.tp2`**

```
[matastral@dhcp-lan1 ~]$ hostname
dhcp-lan1.tp2
```

```
[matastral@dhcp-lan1 ~]$ ip a
[...]
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e4:eb:aa brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.253/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee4:ebaa/64 scope link
       valid_lft forever preferred_lft forever
```

```
[matastral@dhcp-lan1 ~]$ sudo dnf -y install dhcp-server
```

```
[matastral@dhcp-lan1 ~]$ sudo cat /etc/dhcp/dhcpd.conf
[sudo] password for matastral:
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.1.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.1.1.100 10.1.1.200;
    # specify broadcast address
    option broadcast-address 10.1.1.255;
    # specify gateway
    option routers 10.1.1.254;
    option domain-name-servers 1.1.1.1;
}
```

```
[matastral@dhcp-lan1 ~]$ systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset>
     Active: active (running) since Sun 2023-10-22 17:26:57 CEST; 8min ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1324 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 4611)
     Memory: 4.6M
        CPU: 7ms
     CGroup: /system.slice/dhcpd.service
             └─1324 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd>
```

☀️ **Sur `node1.lan1.tp2`**

```
[matastral@node1-lan1 ~]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8
[sudo] password for matastral:
[matastral@node1-lan1 ~]$ sudo systemctl restart NetworkManager
[matastral@node1-lan1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:49:f3:c8 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet 10.1.1.100/24 brd 10.1.1.255 scope global secondary dynamic noprefixroute enp0s8
       valid_lft 597sec preferred_lft 597sec
    inet6 fe80::a00:27ff:fe49:f3c8/64 scope link
       valid_lft forever preferred_lft forever
```

```
[matastral@node1-lan1 ~]$ ping 10.1.2.12
PING 10.1.2.12 (10.1.2.12) 56(84) bytes of data.
64 bytes from 10.1.2.12: icmp_seq=1 ttl=63 time=1.30 ms
64 bytes from 10.1.2.12: icmp_seq=2 ttl=63 time=0.902 ms
^C
--- 10.1.2.12 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.902/1.103/1.304/0.201 ms
```

## 2. Web web web

☀️ **Sur `web.lan2.tp2`**

```
[matastral@web-lan2 ~]$ hostname
web-lan2.tp2
```

```
[matastral@web-lan2 ~]$ sudo dnf install nginx
```

```
[matastral@web-lan2 var]$ sudo mkdir -p /var/www/site_nul/
```

```
[matastral@web-lan2 ~]$ sudo cat /etc/nginx/conf.d/site_nul.tp2.conf
server {
    listen 80;
    server_name site_nul.tp2;

    location / {
        root /var/www/site_nul;
        index index.html;
    }
}
```

```
[matastral@web-lan2 conf.d]$ sudo systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset>
     Active: active (running) since Sun 2023-10-22 18:14:28 CEST; 4s ago
    Process: 2006 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, >
    Process: 2007 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SU>
    Process: 2008 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 2009 (nginx)
      Tasks: 2 (limit: 4611)
     Memory: 2.0M
        CPU: 15ms
     CGroup: /system.slice/nginx.service
             ├─2009 "nginx: master process /usr/sbin/nginx"
             └─2010 "nginx: worker process"
```

```
[matastral@web-lan2 nginx]$ sudo firewall-cmd --permanent --add-service=http
success
[matastral@web-lan2 nginx]$ sudo systemctl restart firewalld
[matastral@web-lan2 nginx]$ sudo firewall-cmd --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client http ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

```
[matastral@web-lan2 ~]$ sudo ss -alpnt | grep nginx
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=2221,fd=6),("nginx",pid=2182,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=2221,fd=7),("nginx",pid=2182,fd=7))
```

☀️ **Sur `node1.lan1.tp2`**

```
[matastral@node1-lan1 ~]$ cat /etc/hosts
[...]

10.1.2.12 site_nul.tp2
```

```
[matastral@node1-lan1 ~]$ curl site_nul.tp2
<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <title>Site Nul</title>
</head>

<header>
    Coucou c'est Mattis et on kiffe le réseau grâce a Léo !
</header>


</html>
```