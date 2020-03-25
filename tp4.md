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
Internet  10.4.2.11              16   0050.7966.6800  ARPA   FastEthernet1/0
Internet  10.4.1.254              -   cc01.0535.0000  ARPA   FastEthernet0/0
Internet  10.4.2.254              -   cc01.0535.0010  ARPA   FastEthernet1/0
router1#
```

```cisco
VPCS> show arp

cc:01:05:35:00:10  10.4.2.254 expires in 117 seconds

VPCS>
```
