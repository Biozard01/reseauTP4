# TP4

## I. Topologie 1 : simple

### 2. Mise en place

#### B. Définition d'IPs statiques

```
> admin1
```

```bash
[root@admin1 ~]# sestatus
SELinux status:                     enabled
SELinuxfs mount:                    /sys/fs/selinux
SELinux root directory:             /etc/selinux
Loaded policy name:                 targeted
Current mode:                       permissive
Mode from config file:              permissive
Policy MLS status:                  enabled
Policy deny_unknown status:         allowed
Max kernel policy version:          31
[root@admin1 ~]#
```

```bash
[root@admin1 ~]# cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
NAME=enp0s8
DEVICE=enp0s8

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.4.1.11
NETMASK=255.255.255.0
GATEWAY=10.4.1.254
[root@admin1 ~]#
```

```bash
[root@admin1 ~]# hostname
admin1.b1.tp4
[root@admin1 ~]#
```

```
> router1
```

```cisco
router1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.4.1.254      YES manual up                    up
FastEthernet1/0            10.4.2.254      YES manual up                    up
router1#
```

```
> guest1
```

```cisco
Hostname is too long. (Maximum 6 characters)

VPCS> ip 10.4.2.11
Checking for duplicate address...
PC1 : 10.4.2.11 255.255.255.0

VPCS> save
Saving startup configuration to startup.vpc
.  done

VPCS>
```

```
> Vérification
```

```cisco
VPCS> ping 10.4.2.254
10.4.2.254 icmp_seq=1 timeout
84 bytes from 10.4.2.254 icmp_seq=2 ttl=255 time=10.782 ms
84 bytes from 10.4.2.254 icmp_seq=3 ttl=255 time=5.150 ms
84 bytes from 10.4.2.254 icmp_seq=4 ttl=255 time=13.135 ms
^C
VPCS>
```

```cisco
router1#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.4.1.11               0   0800.27ef.6914  ARPA   FastEthernet0/0
Internet  10.4.2.11              73   0050.7966.6800  ARPA   FastEthernet1/0
Internet  10.4.1.254              -   cc01.0535.0000  ARPA   FastEthernet0/0
Internet  10.4.2.254              -   cc01.0535.0010  ARPA   FastEthernet1/0
router1#
```

```bash
[root@admin1 ~]$ ip neigh show
10.4.1.254 dev enp0s8 lladdr cc:01:05:35:00:00 STALE
[root@admin1 ~]$
```

```cisco
VPCS> show arp

cc:01:05:35:00:10  10.4.2.254 expires in 117 seconds

VPCS>
```

#### C. Routage

```
> admin1
```

```bash
[arthur@admin1 ~]$ cat /etc/sysconfig/network-scripts/route-enp0s8
10.4.2.0/24 via 10.4.1.254 dev enp0s8
[arthur@admin1 ~]$
```

```
> guest1
```

```cisco
VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 10.4.2.11/24
GATEWAY     : 10.4.2.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20007
RHOST:PORT  : 127.0.0.1:20008
MTU:        : 1500

VPCS> save
Saving startup configuration to startup.vpc
.  done
```

```
> Vérification
```

```bash
[arthur@admin1 ~]$ ping 10.4.2.11
PING 10.4.2.11 (10.4.2.11) 56(84) bytes of data.
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=1 ttl=64 time=3013 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=2 ttl=64 time=5011 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=3 ttl=64 time=7012 ms
^C
--- 10.4.2.11 ping statistics ---
3 packets transmitted, 3 received, 75% packet loss, time 11016ms
rtt min/avg/max/mdev = 3013.440/5012.286/7012.419/1632.578 ms, pipe 8
[arthur@admin1 ~]$
```

```bash
[arthur@admin1 ~]$ traceroute 10.4.2.11
traceroute to 10.4.2.11 (10.4.2.11), 30 hops max, 60 byte packets
 1 gateway (10.4.1.254) 8.906 ms 8.731 ms 8.669 ms
 2 10.4.2.11 (10.4.2.11) 3013.008 ms **
[arthur@admin1 ~]$
```

```cisco
VPCS> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=19.893 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=13.564 ms
84 bytes from 10.4.1.11 icmp_seq=3 ttl=63 time=19.129 ms
^C
VPCS>
```

## II. Topologie 2 : dumb switches

### 2. Mise en place

#### C. Vérification

```bash
[arthur@admin1 ~]$ ping 10.4.2.11
PING 10.4.2.11 (10.4.2.11) 56(84) bytes of data.
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=1 ttl=64 time=23.2 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=2 ttl=64 time=15.6 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=3 ttl=64 time=16.9 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=1 ttl=64 time=18.3 ms
64 bytes from 10.4.2.11 (10.4.2.11): icmp_seq=2 ttl=64 time=17.2 ms
^C
--- 10.4.2.11 ping statistics ---
6 packets transmitted, 5 received, 16% packet loss, time 5007ms
rtt min/avg/max/mdev = 15.612/18.276/23.258/2.642 ms
[arthur@admin1 ~]$
```

```cisco
VPCS> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=19.643 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=19.632 ms
^C
VPCS>
```

```bash
[arthur@admin1 ~]$ traceroute 10.4.2.11
traceroute to 10.4.2.11 (10.4.2.11), 30 hops max, 60 byte packets
 1 gateway (10.4.1.254) 15.090 ms 14.986 ms 14.905 ms
 2 10.4.2.11 (10.4.2.11) 26.018 ms 25.969 ms 25.827
[arthur@admin1 ~]$
```

## III. Topologie 3 : adding nodes and NAT

#### B. VPCS

```cisco
VPCS> ip 10.4.2.12/24 10.4.2.254
Checking for duplicate address...
PC1 : 10.4.2.12 255.255.255.0 gateway 10.4.2.254

VPCS> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=19.234 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=16.570 ms
^C
VPCS> save
Saving startup configuration to startup.vpc
.  done

VPCS>
```

```cisco
VPCS> ip 10.4.2.13/24 10.4.2.254
Checking for duplicate address...
PC1 : 10.4.2.13 255.255.255.0 gateway 10.4.2.254

VPCS> ping 10.4.1.11
84 bytes from 10.4.1.11 icmp_seq=1 ttl=63 time=13.535 ms
84 bytes from 10.4.1.11 icmp_seq=2 ttl=63 time=13.505 ms
84 bytes from 10.4.1.11 icmp_seq=3 ttl=63 time=19.439 ms
^C
VPCS>

VPCS> save
Saving startup configuration to startup.vpc
.  done

VPCS>
```

#### C. Accès WAN

```
> Donner un accès WAN au routeur
```

```cisco
router1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.4.1.254      YES NVRAM  up                    up
FastEthernet1/0            10.4.2.254      YES NVRAM  up                    up
FastEthernet2/0            192.168.122.61  YES DHCP   up                    up
NVI0                       unassigned      NO  unset  up                    up
router1#
```

```
> Configurer les clients
```

```bash
[arthur@admin1 ~]$ cat /etc/sysconfig/network
# Created by anaconda
GATEWAY=10.4.1.254
[arthur@admin1 ~]$
```

```
> Vérification
```

```cisco
router1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 64/73/100 ms
router1#
```

```bash
[arthur@admin1 ~]$ ping 8.8.8.8
PING  8.8.8.8 ( 8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8 (8.8.8.8): icmp_seq=1 ttl=51 time=39.1 ms
64 bytes from 8.8.8.8 (8.8.8.8): icmp_seq=2 ttl=51 time=27.6 ms
64 bytes from 8.8.8.8 (8.8.8.8): icmp_seq=3 ttl=51 time=38.7 ms
^C
---  8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 27.654/35.177/39.131/5.321 ms
[arthur@admin1 ~]$
```

```cisco
VPCS> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=51 time=29.688 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=51 time=26.029 ms
^C
VPCS>
```

```bash
[arthur@admin1 ~]$ dig guest1.b1.tp4
; <<>> DiG 9.11.4-P2-RedHat-9.11.4-9.P2.el7 <<>> guest1.b1.tp4
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51161
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1452
;; QUESTION SECTION:
;guest1.b1.tp4.                    IN      A

;; AUTHORITY SECTION:
.                       9611       IN      SOA         a.root-server.net. nstld.verisign-grs.com. 2020033000 1800 900 604800 86400

;; Query time: 33 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Mon Mar 30 18:22:30 CEST 2020
;; MSG SIZE  rcvd: 117
[arthur@admin1 ~]$
```

## IV. Topologie 4 : home-made DHCP

### 2. Mise en place

#### B. Setup de la VM

```bash
[arthur@dhcp ~]$ sudo ss -lnu
State   Recv-Q Send-Q   Local Adress:Port             Peer Adress:Port
UNCONN  0      0                   *:67                         *:*
```

Les ports DHCP sont les ports 67 (serveur) et 68 (client). Nous voyons donc que le serveur est en écoute.
