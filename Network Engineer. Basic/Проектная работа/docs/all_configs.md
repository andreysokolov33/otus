## Полная конфигурация всех сетевых устройств

Приведен экспорт настроек по каждому устройству, которое настраивалось в данной схеме

### NAT уровень

R1-NAT:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname R1-NAT

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

username admin privilege 15 password 7 0822455D0A16

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

spanning-tree mode pvst

interface Tunnel1
 ip address 172.30.1.1 255.255.255.252
 mtu 1476
 tunnel source FastEthernet0/0
 tunnel destination 109.245.27.26

interface FastEthernet0/0
 ip address 109.245.17.10 255.255.255.252
 ip nat outside
 duplex auto
 speed auto

interface FastEthernet0/1
 ip address 89.215.20.70 255.255.255.252
 duplex auto
 speed auto
 shutdown

interface FastEthernet1/0
 ip address 172.30.0.1 255.255.255.252
 ip access-group DENY-INTERNET in
 ip nat inside
 duplex auto
 speed auto

interface FastEthernet1/1
 ip address 172.30.0.5 255.255.255.252
 ip access-group DENY-INTERNET in
 ip nat inside
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

ip nat inside source list LOCAL-NET interface FastEthernet0/0 overload
ip nat inside source static tcp 109.245.17.10 21 10.10.1.37 21 
ip nat inside source static tcp 109.245.17.10 80 10.10.1.36 80 
ip nat inside source static tcp 109.245.17.10 443 10.10.1.36 443 
ip nat inside source static tcp 10.10.1.36 80 109.245.17.10 80 
ip nat inside source static tcp 10.10.1.36 443 109.245.17.10 443 
ip nat inside source static tcp 10.10.1.37 21 109.245.17.10 21 
ip classless
ip route 0.0.0.0 0.0.0.0 109.245.17.9 
ip route 0.0.0.0 0.0.0.0 89.215.20.69 2
ip route 10.10.0.0 255.255.240.0 172.30.0.2 
ip route 10.10.0.0 255.255.240.0 172.30.0.6 2
ip route 192.168.1.0 255.255.255.0 172.30.1.2 

ip access-list standard LOCAL-NET
 permit 10.10.0.0 0.0.15.255
ip access-list standard DENY-INTERNET
 remark Deny Access To Internet
 deny 10.10.1.0 0.0.0.31
 deny 10.10.2.0 0.0.0.255
 deny 10.10.10.32 0.0.0.31
 permit any

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 exec-timeout 0 0
 password 7 0822455D0A16
 logging synchronous
 login

line vty 0 4
 login local

end
```

R2-NAT:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname R2-NAT

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

no ip cef
no ipv6 cef

username admin privilege 15 password 7 0822455D0A16

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

spanning-tree mode pvst

interface FastEthernet0/0
 ip address 109.245.17.14 255.255.255.252
 duplex auto
 speed auto
 shutdown

interface FastEthernet0/1
 ip address 89.215.20.66 255.255.255.252
 ip nat outside
 duplex auto
 speed auto

interface FastEthernet1/0
 ip address 172.30.0.9 255.255.255.252
 ip access-group DENY-INTERNET in
 ip nat inside
 duplex auto
 speed auto

interface FastEthernet1/1
 ip address 172.30.0.13 255.255.255.252
 ip access-group DENY-INTERNET in
 ip nat inside
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

ip nat inside source list LOCAL-NET interface FastEthernet0/1 overload
ip nat inside source static tcp 10.10.1.36 80 89.215.20.66 80 
ip nat inside source static tcp 10.10.1.36 443 89.215.20.66 443 
ip nat inside source static tcp 10.10.1.37 21 89.215.20.66 21 
ip classless
ip route 0.0.0.0 0.0.0.0 89.215.20.65 
ip route 0.0.0.0 0.0.0.0 109.245.17.13 2
ip route 10.10.0.0 255.255.240.0 172.30.0.14 
ip route 10.10.0.0 255.255.240.0 172.30.0.10 2

ip access-list standard LOCAL-NET
 permit 10.10.0.0 0.0.15.255
ip access-list standard DENY-INTERNET
 remark Deny Access To Internet
 deny 10.10.1.0 0.0.0.31
 deny 10.10.2.0 0.0.0.255
 deny 10.10.10.32 0.0.0.31
 permit any

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^^C

line con 0
 exec-timeout 0 0
 password 7 0822455D0A16
 logging synchronous
 login

line vty 0 4
 login local

end
```

### Корневой уровень

CORE1-SW:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname CORE1-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

no ip cef
ip routing

no ipv6 cef

username admin password 7 0822455D0A16

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

spanning-tree mode rapid-pvst
spanning-tree vlan 55-56,60,90-93,111,125,130 priority 24576
spanning-tree vlan 57,77,80,88-89,100,200 priority 28672

interface Port-channel1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/3
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/4
 no switchport
 ip address 172.30.0.2 255.255.255.252
 ip access-group PERMIT-INTERNET out
 duplex auto
 speed auto

interface GigabitEthernet1/0/5
 no switchport
 ip address 172.30.0.10 255.255.255.252
 ip access-group PERMIT-INTERNET out
 duplex auto
 speed auto

interface GigabitEthernet1/0/6
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/7
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/8
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,77,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/9
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,88,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/10
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,89,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/11
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,90-91,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/12
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,92-93,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/13
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/14
 switchport trunk native vlan 573
 switchport trunk allowed vlan 56,80,100,125,200
 switchport mode trunk

interface GigabitEthernet1/0/15
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/16
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/17
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/18
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/19
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/20
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/21
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/22
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/23
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/24
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/1
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/2
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/3
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/4
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface Vlan1
 no ip address
 shutdown

interface Vlan55
 mac-address 0002.179d.ed01
 ip address 10.10.1.2 255.255.255.224
 ip access-group NO-INTERNET out
 standby version 2
 standby 55 ip 10.10.1.1
 standby 55 priority 120
 standby 55 preempt

interface Vlan56
 mac-address 0002.179d.ed02
 ip address 10.10.2.2 255.255.255.0
 ip helper-address 10.10.10.20
 ip access-group NO-INTERNET out
 standby version 2
 standby 56 ip 10.10.2.1
 standby 56 priority 120
 standby 56 preempt

interface Vlan57
 mac-address 0002.179d.ed03
 ip address 10.10.1.130 255.255.255.128
 standby version 2
 standby 57 ip 10.10.1.129
 standby 57 preempt

interface Vlan60
 mac-address 0002.179d.ed04
 ip address 10.10.1.66 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 60 ip 10.10.1.65
 standby 60 priority 120
 standby 60 preempt

interface Vlan77
 mac-address 0002.179d.ed05
 ip address 10.10.12.2 255.255.254.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 77 ip 10.10.12.1
 standby 77 preempt

interface Vlan80
 mac-address 0002.179d.ed06
 ip address 10.10.10.2 255.255.255.224
 ip access-group VLAN-80 out
 standby version 2
 standby 80 ip 10.10.10.1
 standby 80 preempt

interface Vlan88
 mac-address 0002.179d.ed07
 ip address 10.10.14.2 255.255.255.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 88 ip 10.10.14.1
 standby 88 preempt

interface Vlan89
 mac-address 0002.179d.ed08
 ip address 10.10.15.2 255.255.255.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 89 ip 10.10.15.1
 standby 89 preempt

interface Vlan90
 mac-address 0002.179d.ed09
 ip address 10.10.3.2 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 90 ip 10.10.3.1
 standby 90 priority 120
 standby 90 preempt

interface Vlan91
 mac-address 0002.179d.ed0a
 ip address 10.10.3.66 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 91 ip 10.10.3.65
 standby 91 priority 120
 standby 91 preempt

interface Vlan92
 mac-address 0002.179d.ed0b
 ip address 10.10.3.130 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 92 ip 10.10.3.129
 standby 92 priority 120
 standby 92 preempt

interface Vlan93
 mac-address 0002.179d.ed0c
 ip address 10.10.3.194 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 93 ip 10.10.3.193
 standby 93 priority 120
 standby 93 preempt

interface Vlan100
 mac-address 0002.179d.ed0d
 ip address 10.10.1.34 255.255.255.224
 ip access-group DMZ out
 standby version 2
 standby 100 ip 10.10.1.33
 standby 100 preempt

interface Vlan111
 mac-address 0002.179d.ed0e
 ip address 10.10.4.2 255.255.252.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 111 ip 10.10.4.1
 standby 111 priority 120
 standby 111 preempt

interface Vlan125
 mac-address 0002.179d.ed0f
 ip address 10.10.10.34 255.255.255.224
 ip helper-address 10.10.10.20
 ip access-group NO-INTERNET out
 standby version 2
 standby 125 ip 10.10.10.33
 standby 125 priority 120
 standby 125 preempt

interface Vlan130
 mac-address 0002.179d.ed10
 ip address 10.10.8.2 255.255.254.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 130 ip 10.10.8.1
 standby 130 priority 120
 standby 130 preempt

interface Vlan200
 mac-address 0002.179d.ed11
 ip address 10.10.10.130 255.255.255.128
 standby version 2
 standby 200 ip 10.10.10.129
 standby 200 preempt

ip classless
ip route 0.0.0.0 0.0.0.0 172.30.0.1 
ip route 0.0.0.0 0.0.0.0 172.30.0.9 2

ip access-list extended VLAN-80
 remark Full Access For IT Department and SSH For Management
 permit ip 10.10.12.0 0.0.1.255 any
 permit tcp 10.10.10.128 0.0.0.127 any eq 22
 remark Open DHCP, DNS, FTP Server for all
 permit udp any host 10.10.10.20 range bootps bootpc
 permit udp any host 10.10.10.10 eq domain
 remark Permit ICMP from any to Admin PC
 permit icmp any host 10.10.10.4
 remark Permit DMZ HTTP and HTTPS
 permit tcp host 10.10.1.36 eq www host 10.10.10.4
 permit tcp host 10.10.1.36 eq 443 host 10.10.10.4
 remark permit DMZ EMAIL
 permit tcp host 10.10.1.38 eq smtp host 10.10.10.4
 permit tcp host 10.10.1.38 eq pop3 host 10.10.10.4
 remark permit RADIUS Traffic
 permit udp any host 10.10.10.40 range 1812 1813
 remark Permit Estableshed Connections
 permit tcp any any established
 remark Accept UDP only not from 10.10.0.0/20
 deny udp 10.10.0.0 0.0.15.255 any
 permit udp any any
ip access-list standard NO-INTERNET
 remark Full Access For IT Department, Testing, Management
 permit 10.10.12.0 0.0.1.255
 permit 10.10.15.0 0.0.0.255
 permit 10.10.10.128 0.0.0.127
ip access-list extended DMZ
 remark Full Access For IT Department, Management
 permit ip 10.10.12.0 0.0.1.255 any
 permit tcp 10.10.10.128 0.0.0.127 any eq 22
 remark Permit For All Email, FTP, WEB
 permit tcp any host 10.10.1.36 eq www
 permit tcp any host 10.10.1.36 eq 443
 permit tcp any host 10.10.1.38 eq smtp
 permit tcp any host 10.10.1.38 eq pop3
 remark Permit Estableshed Connections
 permit tcp any host 10.10.1.37
 permit tcp any any established

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 exec-timeout 0 0
 password 7 0822455D0A16
 logging synchronous
 login

line vty 0 4
 access-class 1 in
 login local
 transport input ssh

end
```

CORE2-SW:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname CORE2-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

no ip cef
ip routing

no ipv6 cef

username admin password 7 0822455D0A16

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

spanning-tree mode rapid-pvst
spanning-tree vlan 57,77,80,88-89,100,200 priority 24576
spanning-tree vlan 55-56,60,90-93,111,125,130 priority 28672

interface Port-channel1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/3
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,60,77,80,88-93,100,111,125,130,200
 switchport mode trunk
 channel-group 1 mode desirable

interface GigabitEthernet1/0/4
 no switchport
 ip address 172.30.0.6 255.255.255.252
 ip access-group PERMIT-INTERNET out
 duplex auto
 speed auto

interface GigabitEthernet1/0/5
 no switchport
 ip address 172.30.0.14 255.255.255.252
 ip access-group PERMIT-INTERNET out
 duplex auto
 speed auto

interface GigabitEthernet1/0/6
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/7
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/8
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,77,80,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/9
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,88,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/10
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,89,111,125,200
 switchport mode trunk

interface GigabitEthernet1/0/11
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,90-91,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/12
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,92-93,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/13
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,130,200
 switchport mode trunk

interface GigabitEthernet1/0/14
 switchport trunk native vlan 573
 switchport trunk allowed vlan 56,80,100,125,200
 switchport mode trunk

interface GigabitEthernet1/0/15
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/16
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/17
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/18
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/19
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/20
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/21
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/22
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/23
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/0/24
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/1
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/2
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/3
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface GigabitEthernet1/1/4
 switchport access vlan 357
 switchport mode access
 switchport nonegotiate
 shutdown

interface Vlan1
 no ip address
 shutdown

interface Vlan55
 mac-address 0002.175a.6601
 ip address 10.10.1.3 255.255.255.224
 ip access-group NO-INTERNET out
 standby version 2
 standby 55 ip 10.10.1.1
 standby 55 preempt

interface Vlan56
 mac-address 0002.175a.6602
 ip address 10.10.2.3 255.255.255.0
 ip helper-address 10.10.10.20
 ip access-group NO-INTERNET out
 standby version 2
 standby 56 ip 10.10.2.1
 standby 56 preempt

interface Vlan57
 mac-address 0002.175a.6603
 ip address 10.10.1.131 255.255.255.128
 standby version 2
 standby 57 ip 10.10.1.129
 standby 57 priority 120

interface Vlan60
 mac-address 0002.175a.6604
 ip address 10.10.1.67 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 60 ip 10.10.1.65
 standby 60 preempt

interface Vlan77
 mac-address 0002.175a.6605
 ip address 10.10.12.3 255.255.254.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 77 ip 10.10.12.1
 standby 77 priority 120
 standby 77 preempt

interface Vlan80
 mac-address 0002.175a.6606
 ip address 10.10.10.3 255.255.255.224
 ip access-group VLAN-80 out
 standby version 2
 standby 80 ip 10.10.10.1
 standby 80 priority 120
 standby 80 preempt

interface Vlan88
 mac-address 0002.175a.6607
 ip address 10.10.14.3 255.255.255.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 88 ip 10.10.14.1
 standby 88 priority 120
 standby 88 preempt

interface Vlan89
 mac-address 0002.175a.6608
 ip address 10.10.15.3 255.255.255.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 89 ip 10.10.15.1
 standby 89 priority 120
 standby 89 preempt

interface Vlan90
 mac-address 0002.175a.6609
 ip address 10.10.3.3 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 90 ip 10.10.3.1
 standby 90 preempt

interface Vlan91
 mac-address 0002.175a.660a
 ip address 10.10.3.67 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 91 ip 10.10.3.65
 standby 91 preempt

interface Vlan92
 mac-address 0002.175a.660b
 ip address 10.10.3.131 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 92 ip 10.10.3.129
 standby 92 preempt

interface Vlan93
 mac-address 0002.175a.660c
 ip address 10.10.3.195 255.255.255.192
 ip helper-address 10.10.10.20
 standby version 2
 standby 93 ip 10.10.3.193
 standby 93 preempt

interface Vlan100
 mac-address 0002.175a.660d
 ip address 10.10.1.35 255.255.255.224
 ip access-group DMZ out
 standby version 2
 standby 100 ip 10.10.1.33
 standby 100 priority 120
 standby 100 preempt

interface Vlan111
 mac-address 0002.175a.660e
 ip address 10.10.4.3 255.255.252.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 111 ip 10.10.4.1
 standby 111 preempt

interface Vlan125
 mac-address 0002.175a.660f
 ip address 10.10.10.35 255.255.255.224
 ip helper-address 10.10.10.20
 ip access-group NO-INTERNET out
 standby version 2
 standby 125 ip 10.10.10.33
 standby 125 preempt

interface Vlan130
 mac-address 0002.175a.6610
 ip address 10.10.8.3 255.255.254.0
 ip helper-address 10.10.10.20
 standby version 2
 standby 130 ip 10.10.8.1
 standby 130 preempt

interface Vlan200
 mac-address 0002.175a.6611
 ip address 10.10.10.131 255.255.255.128
 standby version 2
 standby 200 ip 10.10.10.129
 standby 200 priority 120
 standby 200 preempt

ip classless
ip route 0.0.0.0 0.0.0.0 172.30.0.5 2
ip route 0.0.0.0 0.0.0.0 172.30.0.13 
ip route 192.168.1.0 255.255.255.0 172.30.0.5 

ip access-list extended VLAN-80
 remark Full Access For IT Department and SSH For Management
 permit ip 10.10.12.0 0.0.1.255 any
 permit tcp 10.10.10.128 0.0.0.127 any eq 22
 remark Open DHCP, DNS, FTP Server for all
 permit udp any host 10.10.10.20 range bootps bootpc
 permit udp any host 10.10.10.10 eq domain
 remark Permit ICMP from any to Admin PC
 permit icmp any host 10.10.10.4
 remark Permit DMZ HTTP and HTTPS
 permit tcp host 10.10.1.36 eq www host 10.10.10.4
 permit tcp host 10.10.1.36 eq 443 host 10.10.10.4
 remark permit DMZ EMAIL
 permit tcp host 10.10.1.38 eq smtp host 10.10.10.4
 permit tcp host 10.10.1.38 eq pop3 host 10.10.10.4
 remark permit RADIUS Traffic
 permit udp any host 10.10.10.40 range 1812 1813
 remark Permit Estableshed Connections
 permit tcp any any established
 remark Accept UDP only not from 10.10.0.0/20
 deny udp 10.10.0.0 0.0.15.255 any
 permit udp any any
ip access-list standard NO-INTERNET
 remark Full Access For IT Department, Testing, Management
 permit 10.10.12.0 0.0.1.255
 permit 10.10.15.0 0.0.0.255
 permit 10.10.10.128 0.0.0.127
ip access-list extended DMZ
 remark Full Access For IT Department, Management
 permit ip 10.10.12.0 0.0.1.255 any
 permit tcp 10.10.10.128 0.0.0.127 any eq 22
 remark Permit For All Email, FTP, WEB
 permit tcp any host 10.10.1.36 eq www
 permit tcp any host 10.10.1.36 eq 443
 permit tcp any host 10.10.1.38 eq smtp
 permit tcp any host 10.10.1.38 eq pop3
 permit tcp any host 10.10.1.37 eq ftp
 remark Permit Estableshed Connections
 permit tcp any host 10.10.1.37
 permit tcp any any established

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 exec-timeout 0 0
 password 7 0822455D0A16
 logging synchronous
 login

line vty 0 4
 access-class 1 in
 login local
 transport input ssh

end
```

### Уровень распределения


Охрана

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname GuardSW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 57
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 switchport access vlan 56
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 switchport access vlan 55
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/4
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/5
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,80,111,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-57,80,111,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.132 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

Ресепшн

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname ReseptionSW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

sanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 switchport access vlan 60
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/4
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/5
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,130,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,130,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.133 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

Серверная

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname ServerSW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 switchport access vlan 56
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 switchport access vlan 80
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/4
 switchport access vlan 80
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/5
 switchport access vlan 80
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/6
 switchport access vlan 80
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/7
 switchport access vlan 80
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/8
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/23
 switchport access vlan 200
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/24
 switchport access vlan 200
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,60,80,111,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.134 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login


end
```

IT Отдел

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname IT-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 77
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 switchport access vlan 77
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/4
 switchport access vlan 56
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/5
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,77,80,111,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,77,80,111,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.135 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

Ремонтный отдел

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname RepairSW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/2
 switchport access vlan 88
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/3
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/4
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/5
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,88,111,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,88,111,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.136 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

Тестировщики

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname TestingSW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/2
 switchport access vlan 89
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/3
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/4
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/5
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,89,111,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,89,111,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.137 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

HR/Юристы

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname HR-Jur-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/2
 switchport access vlan 91
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/3
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/4
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/5
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 shutdown

interface FastEthernet0/23
 switchport access vlan 90
 switchport mode access
 spanning-tree portfast

interface FastEthernet0/24
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,90-91,111,125,130,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,90-91,111,125,130,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.138 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

Бухгалтерия/Администрация

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption

hostname ACC-ADMIN-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 7 0822455D0A16

spanning-tree mode rapid-pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 92
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 switchport access vlan 92
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/4
 switchport access vlan 56
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/5
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/6
 switchport access vlan 125
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 shutdown

interface FastEthernet0/23
 switchport access vlan 93
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/24
 switchport access vlan 111
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,92-93,111,125,130,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 55-56,80,92-93,111,125,130,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

interface Vlan200
 ip address 10.10.10.139 255.255.255.128

ip default-gateway 10.10.10.129

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

DMZ

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname DMZ-SW

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

ip ssh version 2
no ip domain-lookup
ip domain-name company.ru

username admin privilege 1 password 0 cisco

spanning-tree mode pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport access vlan 100
 switchport mode access

interface FastEthernet0/2
 switchport access vlan 100
 switchport mode access

interface FastEthernet0/3
 switchport access vlan 100
 switchport mode access

interface FastEthernet0/4
 switchport access vlan 56
 switchport mode access

interface FastEthernet0/5
 switchport access vlan 125
 switchport mode access

interface FastEthernet0/6
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/7
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/8
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/9
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/10
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/11
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/12
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/13
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/14
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/15
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/16
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/17
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/18
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/19
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/20
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/21
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/22
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/23
 switchport access vlan 357
 switchport mode access
 shutdown

interface FastEthernet0/24
 switchport access vlan 357
 switchport mode access
 shutdown

interface GigabitEthernet0/1
 switchport trunk native vlan 573
 switchport trunk allowed vlan 56,80,100,125,200
 switchport mode trunk

interface GigabitEthernet0/2
 switchport trunk native vlan 573
 switchport trunk allowed vlan 56,80,100,125,200
 switchport mode trunk

interface Vlan1
 no ip address
 shutdown

banner motd ^C!!! NO UNAUTHORIZED ACCES !!!^C

line con 0
 password cisco
 logging synchronous
 login
 exec-timeout 0 0

line vty 0 4
 access-class 1 in
 login local
 transport input ssh
line vty 5 15
 login

end
```

### Дом клиента

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname Router

ip cef
no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet0/0
 ip address 89.216.2.10 255.255.255.252
 ip nat outside
 duplex auto
 speed auto

interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

ip nat inside source list 1 interface GigabitEthernet0/0 overload
ip nat inside source list WAN interface GigabitEthernet0/0 overload
ip classless
ip route 0.0.0.0 0.0.0.0 89.216.2.9 

access-list 1 permit 192.168.1.0 0.0.0.255
ip access-list standard WAN
 permit 192.168.1.0 0.0.0.255

no cdp run

line con 0
 exec-timeout 0 0

line vty 0 4
 login

end
```

### Дом инженера

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname Router

ip cef
no ipv6 cef

spanning-tree mode pvst

interface Tunnel1
 ip address 172.30.1.2 255.255.255.252
 mtu 1476
 tunnel source GigabitEthernet0/0
 tunnel destination 109.245.17.10

interface GigabitEthernet0/0
 ip address 109.245.27.26 255.255.255.252
 ip nat outside
 duplex auto
 speed auto

interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

ip nat inside source list 1 interface GigabitEthernet0/0 overload
ip nat inside source list WAN interface GigabitEthernet0/0 overload
ip classless
ip route 0.0.0.0 0.0.0.0 109.245.27.25 
ip route 10.10.0.0 255.255.240.0 172.30.1.1 

access-list 1 permit 192.168.1.0 0.0.0.255
ip access-list standard WAN
 permit 192.168.1.0 0.0.0.255

no cdp run

line con 0
 exec-timeout 0 0

line vty 0 4
 login

end
```


### Интернет

ROSTELECOM:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname ROSTELECOM

no ip cef
ip routing

no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet1/0/1
 no switchport
 ip address 109.245.17.9 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/2
 no switchport
 ip address 109.245.17.13 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/3
 no switchport
 ip address 109.250.12.10 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

router ospf 10
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet1/0/3

ip classless

line con 0
 exec-timeout 0 0

line vty 0 4
 login

end
```

MTS:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname MTS

no ip cef
ip routing

no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet1/0/1
 no switchport
 ip address 89.215.20.65 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/2
 no switchport
 ip address 89.215.20.69 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/3
 no switchport
 ip address 89.215.30.22 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

router ospf 10
 router-id 2.2.2.2
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet1/0/3

ip classless

line con 0
 exec-timeout 0 0

line vty 0 4
 login

end
```

INTERNET:

```
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname INTERNET

no ip cef
ip routing

no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet1/0/1
 no switchport
 ip address 8.8.8.1 255.255.255.0
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/2
 no switchport
 ip address 109.250.12.9 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/3
 no switchport
 ip address 89.215.30.21 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/4
 no switchport
 no ip address
 duplex auto
 speed auto

interface GigabitEthernet1/0/5
 no switchport
 ip address 89.215.40.1 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/6
 no switchport
 ip address 109.245.27.25 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface GigabitEthernet1/0/7
 no switchport
 ip address 89.216.2.9 255.255.255.252
 ip ospf 10 area 0
 duplex auto
 speed auto

interface Vlan1
 no ip address
 shutdown

router ospf 10
 router-id 10.10.10.10
 log-adjacency-changes
 passive-interface default
 no passive-interface GigabitEthernet1/0/2
 no passive-interface GigabitEthernet1/0/3

ip classless

line con 0
 exec-timeout 0 0

line vty 0 4
 login

end
```



Предыдущая страница: [Дальнейшее улучшение схемы офиса](./next_steps.md)

Главная страница: [Главная страница](../README.md)