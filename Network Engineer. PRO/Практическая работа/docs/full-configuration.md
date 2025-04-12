CORE1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname CORE_1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.1 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.5 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 10.0.0.9 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 10.0.0.13 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/0
 ip address 10.0.0.41 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/1
 ip address 10.0.0.17 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/2
 ip address 10.0.0.45 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 default-information originate
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 104.16.0.0 255.255.240.0 10.0.0.42
ip route 104.16.0.0 255.255.240.0 10.0.0.2 100
ip route 198.51.96.0 255.255.248.0 10.0.0.14
ip route 198.51.96.0 255.255.248.0 10.0.0.42 100
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19
end
```

CORE2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname CORE_2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.2 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.61 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 10.0.0.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 10.0.0.25 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/0
 ip address 10.0.0.49 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/1
 ip address 10.0.0.29 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/2
 ip address 10.0.0.53 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet1/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 default-information originate
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 104.16.0.0 255.255.240.0 10.0.0.50
ip route 104.16.0.0 255.255.240.0 10.0.0.26 100
ip route 198.51.96.0 255.255.248.0 10.0.0.26
ip route 198.51.96.0 255.255.248.0 10.0.0.50 100
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19
end
```

Technical_1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Technical2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180

no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.20 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.30 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.66 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.4.0.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 10 ip 10.4.0.1
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.4.1.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 20 ip 10.4.1.1
 standby 20 priority 150
 standby 20 preempt
interface Ethernet0/2.999
 encapsulation dot1Q 999
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
ntp server 10.2.0.19
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

Technical_2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Technical2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ntp server 10.2.0.19
no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.20 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.30 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.66 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.4.0.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 10 ip 10.4.0.1
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.4.1.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 20 ip 10.4.1.1
 standby 20 priority 150
 standby 20 preempt
interface Ethernet0/2.999
 encapsulation dot1Q 999
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

SW1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW1
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 duplex auto
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 duplex auto
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 10
 switchport mode access
interface Ethernet1/0
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19
end
```

SW2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW2
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 duplex auto
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 duplex auto
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 20
 switchport mode access
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

SW3

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW3
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
interface Ethernet0/2
interface Ethernet0/3
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
 switchport access vlan 30
 switchport mode access
interface Ethernet1/2
interface Ethernet1/3
interface Ethernet2/0
interface Ethernet2/1
interface Ethernet2/2
interface Ethernet2/3
interface Ethernet3/0
interface Ethernet3/1
interface Ethernet3/2
interface Ethernet3/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

SW4

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW4
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
 switchport access vlan 40
 switchport mode access
interface Ethernet0/2
interface Ethernet0/3
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 30,40,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
interface Ethernet1/2
 switchport access vlan 40
 switchport mode access
interface Ethernet1/3
interface Ethernet2/0
interface Ethernet2/1
interface Ethernet2/2
interface Ethernet2/3
interface Ethernet3/0
interface Ethernet3/1
interface Ethernet3/2
interface Ethernet3/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

Server_1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Server_1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.4 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.6 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.33 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.30
 encapsulation dot1Q 30
 ip address 10.4.3.3 255.255.255.0
 standby 30 ip 10.4.3.1
 standby 30 priority 150
 standby 30 preempt
 ip ospf network broadcast
 ip ospf 1 area 1
interface Ethernet0/2.40
 encapsulation dot1Q 40
 ip address 10.4.4.3 255.255.255.0
 standby 40 ip 10.4.4.1
 standby 40 preempt
 ip ospf network broadcast
 ip ospf 1 area 1
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 area 1 stub no-summary
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19
end
```

Server_2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Server_2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.6 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.62 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.34 255.255.255.252
interface Ethernet0/2
 no ip address
 ip ospf 1 area 1
interface Ethernet0/2.30
 encapsulation dot1Q 30
 ip address 10.4.3.2 255.255.255.0
 standby 30 ip 10.4.3.1
 standby 30 preempt
 ip ospf network broadcast
 ip ospf 1 area 1
interface Ethernet0/2.40
 encapsulation dot1Q 40
 ip address 10.4.4.2 255.255.255.0
 standby 40 ip 10.4.4.1
 standby 40 priority 150
 standby 40 preempt
 ip ospf network broadcast
 ip ospf 1 area 1
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 ip address 10.0.0.73 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 1
router ospf 1
 auto-cost reference-bandwidth 10000
 area 1 stub
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 10.2.0.2
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19
end
```

DHCP-NAT

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW8
boot-start-marker
boot-end-marker
no aaa new-model
clock timezone MSK 3 0
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ip dhcp excluded-address 10.4.0.1 10.4.0.3
ip dhcp excluded-address 10.4.1.1 10.4.1.3
ip dhcp excluded-address 10.4.5.1 10.4.5.3
ip dhcp excluded-address 10.4.6.1 10.4.6.3
ip dhcp excluded-address 10.4.7.1 10.4.7.3
ip dhcp excluded-address 10.4.8.1 10.4.8.3
ip dhcp pool VLAN10
 network 10.4.0.0 255.255.255.0
 default-router 10.4.0.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
ip dhcp pool VLAN20
 network 10.4.1.0 255.255.255.0
 default-router 10.4.1.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
ip dhcp pool VLAN50
 network 10.4.5.0 255.255.255.0
 default-router 10.4.5.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
ip dhcp pool VLAN60
 network 10.4.6.0 255.255.255.0
 default-router 10.4.6.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
ip dhcp pool VLAN70
 network 10.4.7.0 255.255.255.0
 default-router 10.4.7.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
ip dhcp pool VLAN80
 network 10.4.8.0 255.255.255.0
 default-router 10.4.8.1
 dns-server 8.8.8.8
 option 42 ip 10.4.4.10
no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.19 255.255.255.255
 ip ospf 1 area 1
interface Ethernet0/0
 ip address 10.4.4.10 255.255.255.0
 ip ospf network broadcast
 ip ospf 1 area 1
interface Ethernet0/1
 no ip address
interface Ethernet0/2
 no ip address
interface Ethernet0/3
 no ip address
router ospf 1
 area 1 stub
ip forward-protocol nd
no ip http server
no ip http secure-server
access-list 10 permit 10.0.0.0 0.255.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp master 1
end
```

Management_1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Management_1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ntp server 10.2.0.19
no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.9 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.10 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.57 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.50
 encapsulation dot1Q 50
 ip address 10.4.5.2 255.255.255.0
 ip helper-address 10.4.4.10
 standby 50 ip 10.4.5.1
 standby 50 preempt
interface Ethernet0/2.60
 encapsulation dot1Q 60
 ip address 10.4.6.2 255.255.255.0
 ip helper-address 10.4.4.10
 standby 60 ip 10.4.6.1
 standby 60 priority 150
 standby 60 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

Management_2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Management_2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ntp server 10.2.0.19
no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.10 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.22 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.58 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.50
 encapsulation dot1Q 50
 ip address 10.4.5.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 50 ip 10.4.5.1
 standby 50 priority 150
 standby 50 preempt
interface Ethernet0/2.60
 encapsulation dot1Q 60
 ip address 10.4.6.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 60 ip 10.4.6.1
 standby 60 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

SW5

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW6
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
interface Ethernet0/2
 switchport access vlan 60
 switchport mode access
interface Ethernet0/3
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

SW6

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW5
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
interface Ethernet0/2
 switchport access vlan 50
 switchport mode access
interface Ethernet0/3
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 50,60,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

Director_1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Director_1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ntp server 10.2.0.19
no ip domain lookup
ip domain name company.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.8 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.70 255.255.255.252
 ip ospf network point-to-point
interface Ethernet0/1
 ip address 10.0.0.46 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 no ip address
interface Ethernet0/2.70
 encapsulation dot1Q 70
 ip address 10.4.7.2 255.255.255.0
 ip helper-address 10.4.4.10
 standby 70 ip 10.4.7.1
 standby 70 preempt
interface Ethernet0/2.80
 encapsulation dot1Q 80
 ip address 10.4.8.2 255.255.255.0
 ip helper-address 10.4.4.10
 standby 80 ip 10.4.8.1
 standby 80 priority 150
 standby 80 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 auto-cost reference-bandwidth 10000
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

Director_2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Director_2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ntp server 10.2.0.19
no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Ethernet0/0
 ip address 10.0.0.54 255.255.255.252
interface Ethernet0/1
 ip address 10.0.0.69 255.255.255.252
interface Ethernet0/2
 no ip address
interface Ethernet0/2.70
 encapsulation dot1Q 70
 ip address 10.4.7.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 70 ip 10.4.7.1
 standby 70 priority 150
 standby 70 preempt
interface Ethernet0/2.80
 encapsulation dot1Q 80
 ip address 10.4.8.3 255.255.255.0
 ip helper-address 10.4.4.10
 standby 80 ip 10.4.8.1
 standby 80 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 redistribute connected subnets
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

SW7

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW7
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 70
 switchport mode access
interface Ethernet1/0
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

SW8

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW8
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name company.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 70,80,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 80
 switchport mode access
interface Ethernet1/0
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.2.0.19 
end
```

V-DHCP-NTP

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname V-DHCP-NTP
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


ip dhcp excluded-address 10.16.1.1 10.16.1.3
ip dhcp excluded-address 10.16.2.1 10.16.2.3
ip dhcp pool VLAN10
 network 10.16.1.0 255.255.255.0
 default-router 10.16.1.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20
 network 10.16.2.0 255.255.255.0
 default-router 10.16.2.1
 dns-server 8.8.8.8
no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
interface Ethernet0/0
 ip address 10.16.2.10 255.255.255.0
 ip ospf 1 area 0
 duplex auto
interface Ethernet0/1
 no ip address
 duplex auto
interface Ethernet0/2
 no ip address
 duplex auto
interface Ethernet0/3
 no ip address
 duplex auto
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip ssh time-out 60
ip ssh version 2
ipv6 ioam timestamp
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp master 1
end
```

SW10

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW10
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name nn.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 20
 switchport mode access
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.16.2.10
end
```

SW9

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW9
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name nn.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/2
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet0/3
 switchport access vlan 10
 switchport mode access
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.16.2.10
end
```

Moskow GW1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname GW1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ip source-route
ip cef
no ip domain lookup
ip domain name nn.ru
no ipv6 traffic interface-statistics
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.6 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.26 255.255.255.252
 ip nat inside
 ip virtual-reassembly
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.14 255.255.255.252
 ip nat inside
 ip virtual-reassembly
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 no ip address
 shutdown
interface Ethernet0/3
 no ip address
interface Ethernet1/0
 ip address 185.10.20.5 255.255.255.252
interface Ethernet1/1
 ip address 185.10.20.1 255.255.255.252
 ip nat outside
 ip virtual-reassembly
interface Ethernet1/2
 no ip address
interface Ethernet1/3
 no ip address
router ospf 1
 log-adjacency-changes
 auto-cost reference-bandwidth 10000
 default-information originate
router bgp 65000
 no synchronization
 bgp log-neighbor-changes
 network 185.10.20.0 mask 255.255.254.0
 network 185.10.20.0 mask 255.255.255.252
 neighbor 10.2.0.7 remote-as 65000
 neighbor 10.2.0.7 update-source Loopback1
 neighbor 10.2.0.7 next-hop-self
 neighbor 10.2.0.7 route-map SET-LOCALPREF in
 neighbor 185.10.20.2 remote-as 520
 neighbor 185.10.20.6 remote-as 101
 no auto-summary
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list 1 interface Ethernet1/1 overload
ip route 104.16.0.0 255.255.240.0 10.2.0.7
ip route 185.10.20.0 255.255.254.0 Null0
ip route 185.10.20.12 255.255.255.252 10.2.0.7
ip route 203.0.112.0 255.255.252.0 10.2.0.7
ip prefix-list MTS seq 5 permit 104.16.0.0/20
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 10.0.0.0 0.15.255.255
route-map SET-LOCALPREF permit 10
 match ip address prefix-list MTS
 set local-preference 200
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
exception data-corruption buffer truncate
end
```

Moskow GW 2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname GW2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ip source-route
ip cef
no ip domain lookup
ip domain name nn.ru
no ipv6 traffic interface-statistics
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.2.0.7 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 10.0.0.50 255.255.255.252
 ip nat inside
 ip virtual-reassembly
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 10.0.0.42 255.255.255.252
 ip nat inside
 ip virtual-reassembly
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 no ip address
interface Ethernet0/3
 no ip address
interface Ethernet1/0
 ip address 185.10.20.13 255.255.255.252
 ip nat outside
 ip virtual-reassembly
interface Ethernet1/1
 ip address 185.10.20.9 255.255.255.252
interface Ethernet1/2
 no ip address
interface Ethernet1/3
 no ip address
router ospf 1
 log-adjacency-changes
 auto-cost reference-bandwidth 10000
 default-information originate
router bgp 65000
 no synchronization
 bgp log-neighbor-changes
 network 185.10.20.0 mask 255.255.254.0
 network 185.10.20.12 mask 255.255.255.252
 neighbor 10.2.0.6 remote-as 65000
 neighbor 10.2.0.6 update-source Loopback1
 neighbor 10.2.0.6 next-hop-self
 neighbor 185.10.20.14 remote-as 101
 no auto-summary
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list 1 interface Ethernet1/0 overload
ip route 185.10.20.0 255.255.254.0 Null0
ip route 185.10.20.0 255.255.255.252 10.2.0.6
ip route 192.71.0.0 255.255.240.0 10.2.0.6
ip route 198.51.96.0 255.255.248.0 10.2.0.6
ip route 203.0.112.0 255.255.252.0 185.10.20.14
access-list 1 permit 10.0.0.0 0.15.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
exception data-corruption buffer truncate
end
```


VladimirGW-1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname VladimirGW1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name vladimir.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
crypto ikev2 proposal IKEv2-DMVPN
 encryption aes-cbc-256
 integrity sha256
 group 21
crypto ikev2 policy IKEv2-POLICY
 proposal IKEv2-DMVPN
crypto ikev2 keyring IKEv2-PSK
 peer HUB
  address 185.10.20.255
  pre-shared-key OTUS
crypto ikev2 profile IKEv2-PROFILE
 match identity remote address 185.10.20.255 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local IKEv2-PSK
crypto ipsec transform-set IPSEC-TRANSFORM esp-aes 256 esp-sha256-hmac
 mode transport
crypto ipsec profile DMVPN-PROFILE
 set transform-set IPSEC-TRANSFORM
 set ikev2-profile IKEv2-PROFILE
interface Loopback1
 ip address 10.16.0.1 255.255.255.255
interface Tunnel100
 ip address 10.1.0.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 185.10.20.255
 ip nhrp map 10.1.0.1 185.10.20.255
 ip nhrp network-id 100
 ip nhrp nhs 10.1.0.1
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/1
 tunnel mode gre multipoint
 tunnel protection ipsec profile DMVPN-PROFILE
interface Ethernet0/0
 ip address 10.16.255.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/1
 ip address 198.51.96.10 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
interface Ethernet0/2
 no ip address
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.16.1.2 255.255.255.0
 ip helper-address 10.16.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 10 ip 10.16.1.1
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.16.2.2 255.255.255.0
 ip helper-address 10.16.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 20 ip 10.16.2.1
 standby 20 priority 150
 standby 20 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list NAT-LOCAL interface Ethernet0/1 overload
ip route 0.0.0.0 0.0.0.0 198.51.96.9
ip route 0.0.0.0 0.0.0.0 10.16.255.2 100
ip route 10.0.0.0 255.240.0.0 10.1.0.1
ip route 10.20.0.0 255.252.0.0 10.1.0.3
ip access-list standard NAT-LOCAL
 permit 10.16.0.0 0.3.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.16.2.10
end
```

VladimirGW-2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname VladimirGW2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name vladimir.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.16.0.2 255.255.255.255
interface Ethernet0/0
 ip address 198.51.96.6 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
interface Ethernet0/1
 ip address 10.16.255.2 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2
 no ip address
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.16.1.3 255.255.255.0
 ip helper-address 10.16.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 10 ip 10.16.1.1
 standby 10 priority 150
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.16.2.3 255.255.255.0
 ip helper-address 10.16.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 20 ip 10.16.2.1
 standby 20 preempt
interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native
interface Ethernet0/3
 no ip address
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list NAT-LOCAL interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 198.51.96.5
ip route 0.0.0.0 0.0.0.0 10.16.255.1 100
ip access-list standard NAT-LOCAL
 permit 10.16.0.0 0.3.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.16.2.10
end
```

NN-DHCP-NAT

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname NN-DHCP-NAT
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
ip dhcp excluded-address 10.20.1.1 10.20.1.3
ip dhcp excluded-address 10.20.2.1 10.20.2.3
ip dhcp pool VLAN10
 network 10.20.1.0 255.255.255.0
 default-router 10.20.1.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20
 network 10.20.2.0 255.255.255.0
 default-router 10.20.2.1
 dns-server 8.8.8.8
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
interface Ethernet0/0
 ip address 10.20.2.10 255.255.255.0
 duplex auto
interface Ethernet0/1
 no ip address
 duplex auto
interface Ethernet0/2
 no ip address
 duplex auto
interface Ethernet0/3
 no ip address
 duplex auto
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip ssh time-out 60
ip ssh version 2
ipv6 ioam timestamp
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp master 1
end
```

NNGW1

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname NNGW1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
crypto ikev2 proposal IKEv2-DMVPN
 encryption aes-cbc-256
 integrity sha256
 group 21
crypto ikev2 policy IKEv2-POLICY
 proposal IKEv2-DMVPN
crypto ikev2 keyring IKEv2-PSK
 peer HUB
  address 185.10.20.255
  pre-shared-key OTUS
crypto ikev2 profile IKEv2-PROFILE
 match identity remote address 185.10.20.255 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local IKEv2-PSK
crypto ipsec transform-set IPSEC-TRANSFORM esp-aes 256 esp-sha256-hmac
 mode transport
crypto ipsec profile DMVPN-PROFILE
 set transform-set IPSEC-TRANSFORM
 set ikev2-profile IKEv2-PROFILE
interface Loopback1
 ip address 10.20.0.1 255.255.255.255
interface Tunnel100
 ip address 10.1.0.3 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 185.10.20.255
 ip nhrp map 10.1.0.1 185.10.20.255
 ip nhrp network-id 100
 ip nhrp nhs 10.1.0.1
 ip tcp adjust-mss 1360
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
 tunnel protection ipsec profile DMVPN-PROFILE
interface Ethernet0/0
 ip address 104.16.0.6 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
interface Ethernet0/1
 ip address 10.20.99.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2
 no ip address
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.20.1.2 255.255.255.0
 ip helper-address 10.20.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 10 ip 10.20.1.1
 standby 10 priority 150
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.20.2.2 255.255.255.0
 ip helper-address 10.20.2.10
 ip nat outside
 ip virtual-reassembly in
 standby 20 ip 10.20.2.1
 standby 20 preempt
interface Ethernet0/3
 ip address 10.20.10.9 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list 1 interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 104.16.0.5
ip route 0.0.0.0 0.0.0.0 10.20.99.2 100
ip route 10.0.0.0 255.240.0.0 10.1.0.1
ip route 10.16.0.0 255.252.0.0 10.1.0.2
access-list 1 permit 10.20.0.0 0.3.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.20.2.10
end
```

NNGW2

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname NNGW2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name nn.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 10.20.0.2 255.255.255.255
interface Ethernet0/0
 ip address 104.16.0.10 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
interface Ethernet0/1
 ip address 10.20.99.2 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2
 no ip address
 ip nat inside
 ip virtual-reassembly in
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.20.1.3 255.255.255.0
 ip helper-address 10.20.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 10 ip 10.20.1.1
 standby 10 preempt
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.20.2.3 255.255.255.0
 ip helper-address 10.20.2.10
 ip nat inside
 ip virtual-reassembly in
 standby 20 ip 10.20.2.1
 standby 20 priority 150
 standby 20 preempt
interface Ethernet0/3
 no ip address
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
ip forward-protocol nd
no ip http server
no ip http secure-server
ip nat inside source list 1 interface Ethernet0/0 overload
ip route 0.0.0.0 0.0.0.0 104.16.0.9
ip route 0.0.0.0 0.0.0.0 10.20.99.1 100
access-list 1 permit 10.20.0.0 0.3.255.255
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
ntp server 10.20.2.10
end
```

SW11

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW11
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name nn.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
interface Ethernet0/2
 switchport access vlan 10
 switchport mode access
interface Ethernet0/3
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

SW12

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
hostname SW12
boot-start-marker
boot-end-marker
no aaa new-model
no ip domain-lookup
ip domain-name nn.ru
ip cef
no ipv6 cef
spanning-tree mode pvst
spanning-tree extend system-id
vlan internal allocation policy ascending
ip ssh time-out 60
ip ssh version 2
interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
interface Ethernet0/1
interface Ethernet0/2
 switchport access vlan 20
 switchport mode access
interface Ethernet0/3
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
interface Ethernet1/1
interface Ethernet1/2
interface Ethernet1/3
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R26:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R27
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.3 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 172.20.0.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 172.20.0.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 172.20.0.25 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 192.71.0.1 255.255.255.252
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 redistribute connected
 redistribute static
 neighbor 520 peer-group
 neighbor 520 remote-as 520
 neighbor 520 update-source Loopback1
 neighbor 520 route-reflector-client
 neighbor 520 next-hop-self
 neighbor 172.20.255.5 peer-group 520
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R28:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R28
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.4 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 172.20.0.14 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 172.20.0.29 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 no ip address
interface Ethernet0/3
 no ip address
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 redistribute static
 neighbor 172.20.255.2 remote-as 520
 neighbor 172.20.255.2 update-source Loopback1
 neighbor 172.20.255.2 next-hop-self
 neighbor 172.20.255.5 remote-as 520
 neighbor 172.20.255.5 update-source Loopback1
 neighbor 172.20.255.5 next-hop-self
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

Triada_RR_1:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Triada_RR_1
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.6 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 172.20.0.18 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 172.20.0.22 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 172.20.0.33 255.255.255.252
interface Ethernet0/3
 no ip address
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 neighbor 520 peer-group
 neighbor 520 remote-as 520
 neighbor 520 update-source Loopback1
 neighbor 520 route-reflector-client
 neighbor 520 next-hop-self
 neighbor 172.20.255.1 peer-group 520
 neighbor 172.20.255.2 peer-group 520
 neighbor 172.20.255.5 remote-as 520
 neighbor 172.20.255.5 update-source Loopback1
 neighbor 172.20.255.5 next-hop-self
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

Triada_RR_2:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Triada_RR_2
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.5 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 192.71.0.5 255.255.255.252
interface Ethernet0/1
 ip address 172.20.0.30 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 172.20.0.26 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 172.20.0.34 255.255.255.252
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 network 192.71.0.4 mask 255.255.255.252
 redistribute connected
 redistribute static
 neighbor 520 peer-group
 neighbor 520 remote-as 520
 neighbor 520 update-source Loopback1
 neighbor 520 route-reflector-client
 neighbor 520 next-hop-self
 neighbor 172.20.255.3 peer-group 520
 neighbor 172.20.255.4 peer-group 520
 neighbor 172.20.255.6 remote-as 520
 neighbor 172.20.255.6 update-source Loopback1
 neighbor 172.20.255.6 next-hop-self
ip forward-protocol nd
no ip http server
no ip http secure-server
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R27:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R27
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.3 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 172.20.0.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 172.20.0.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 172.20.0.25 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 192.71.0.1 255.255.255.252
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 redistribute connected
 redistribute static
 neighbor 520 peer-group
 neighbor 520 remote-as 520
 neighbor 520 update-source Loopback1
 neighbor 520 route-reflector-client
 neighbor 520 next-hop-self
 neighbor 172.20.255.5 peer-group 520
 neighbor 192.71.0.2 remote-as 101
 neighbor 192.71.0.2 route-map FILTER_KITRON_OUT out
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 192.71.0.0 255.255.240.0 Null0
ip prefix-list TO-KITRON seq 5 deny 172.20.0.0/16 le 32
ip prefix-list TO-KITRON seq 10 permit 0.0.0.0/0 le 32
route-map FILTER_KITRON_OUT permit 10
 match ip address prefix-list TO-KITRON
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R18:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R18
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name triada.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.1 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 185.10.20.2 255.255.255.252
interface Ethernet0/1
 ip address 172.20.0.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 172.20.0.5 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/3
 ip address 185.10.20.10 255.255.255.252
router ospf 1
router bgp 520
 bgp log-neighbor-changes
 network 0.0.0.0
 redistribute connected
 redistribute static
 neighbor 172.20.255.6 remote-as 520
 neighbor 172.20.255.6 update-source Loopback1
 neighbor 172.20.255.6 next-hop-self
 neighbor 185.10.20.1 remote-as 65000
 neighbor 185.10.20.1 route-map DEFAULT-ROUTE-TO-AS65000 out
 neighbor 185.10.20.9 remote-as 65000
 neighbor 185.10.20.9 route-map DEFAULT-ROUTE-TO-AS65000 out
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
ip route 192.71.0.0 255.255.240.0 Null0
ip prefix-list DEFAULT-ROUTE-TO-AS65000 seq 5 permit 0.0.0.0/0
route-map DEFAULT-ROUTE-TO-AS65000 permit 10
 match ip address prefix-list DEFAULT-ROUTE-TO-AS65000
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R36:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R36
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name lamas.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.1 255.255.255.255
interface Ethernet0/0
 ip address 192.71.0.6 255.255.255.252
interface Ethernet0/1
 ip address 198.51.96.1 255.255.255.252
interface Ethernet0/2
 ip address 198.51.96.5 255.255.255.252
interface Ethernet0/3
 ip address 198.51.96.9 255.255.255.252
interface Ethernet1/0
 no ip address
 shutdown
interface Ethernet1/1
 no ip address
 shutdown
interface Ethernet1/2
 no ip address
 shutdown
interface Ethernet1/3
 no ip address
 shutdown
router bgp 301
 bgp log-neighbor-changes
 network 198.51.96.0 mask 255.255.248.0
 neighbor 192.71.0.5 remote-as 520
 neighbor 192.71.0.5 route-map FILTER-TELNET in
 neighbor 198.51.96.2 remote-as 8359
 neighbor 198.51.96.2 soft-reconfiguration inbound
 neighbor 198.51.96.2 route-map FILTER-TELNET in
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 198.51.96.0 255.255.248.0 Null0
ip prefix-list BLOCK-TELNET seq 5 deny 40.0.0.0/8 le 32
ip prefix-list BLOCK-TELNET seq 10 permit 0.0.0.0/0 le 32
ip prefix-list ONLY-DEFAULT seq 5 permit 0.0.0.0/0
route-map FILTER-TELNET deny 10
 match ip address prefix-list BLOCK-TELNET
route-map FILTER-TELNET permit 20
route-map FILTER-ONLY-DEFAULT deny 10
 match ip address prefix-list ONLY-DEFAULT
route-map FILTER-ONLY-DEFAULT permit 20
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

MTS:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname MTS
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name mts.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.1 255.255.255.255
interface Ethernet0/0
 ip address 203.0.112.2 255.255.255.252
interface Ethernet0/1
 ip address 104.16.0.5 255.255.255.252
interface Ethernet0/2
 ip address 104.16.0.9 255.255.255.252
interface Ethernet0/3
 ip address 198.51.96.2 255.255.255.252
interface Ethernet1/0
 ip address 104.16.0.1 255.255.255.252
interface Ethernet1/1
 no ip address
 shutdown
interface Ethernet1/2
 no ip address
 shutdown
interface Ethernet1/3
 no ip address
 shutdown
router bgp 8359
 bgp router-id 172.20.255.254
 bgp log-neighbor-changes
 network 104.16.0.0 mask 255.255.240.0
 neighbor 198.51.96.1 remote-as 301
 neighbor 203.0.112.1 remote-as 101
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 104.16.0.0 255.255.240.0 Null0
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R31:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R31
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name kitron.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.1 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 185.10.20.6 255.255.255.252
interface Ethernet0/1
 ip address 172.20.0.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 185.10.20.14 255.255.255.252
interface Ethernet0/3
 no ip address
router ospf 1
router bgp 101
 bgp log-neighbor-changes
 network 0.0.0.0
 redistribute static
 neighbor 172.20.255.10 remote-as 101
 neighbor 172.20.255.10 update-source Loopback1
 neighbor 172.20.255.10 next-hop-self
 neighbor 185.10.20.5 remote-as 65000
 neighbor 185.10.20.5 route-map DEFAULT-ROUTE-TO-AS65000 out
 neighbor 185.10.20.13 remote-as 65000
 neighbor 185.10.20.13 route-map DEFAULT-ROUTE-TO-AS65000 out
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
ip route 203.0.112.0 255.255.252.0 Null0
ip prefix-list DEFAULT-ROUTE-TO-AS65000 seq 5 permit 0.0.0.0/0
route-map DEFAULT-ROUTE-TO-AS65000 permit 10
 match ip address prefix-list DEFAULT-ROUTE-TO-AS65000
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

KitronRR

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname Kitron_RR
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.10 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 ip address 172.20.0.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/1
 ip address 172.20.0.6 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 192.71.0.2 255.255.255.252
interface Ethernet0/3
 ip address 203.0.112.5 255.255.255.252
router ospf 1
router bgp 101
 bgp router-id 172.20.255.10
 bgp log-neighbor-changes
 neighbor 101 peer-group
 neighbor 101 remote-as 101
 neighbor 101 update-source Loopback1
 neighbor 101 route-reflector-client
 neighbor 101 next-hop-self
 neighbor 172.20.255.1 peer-group 101
 neighbor 172.20.255.2 peer-group 101
 neighbor 192.71.0.1 remote-as 520
 neighbor 203.0.112.6 remote-as 888
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 203.0.112.0 255.255.252.0 Null0
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R33:

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R33
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name kitron.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.2 255.255.255.255
 ip ospf 1 area 0
interface Ethernet0/0
 no ip address
interface Ethernet0/1
 ip address 172.20.0.5 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
interface Ethernet0/2
 ip address 203.0.112.1 255.255.255.252
interface Ethernet0/3
 no ip address
router ospf 1
router bgp 101
 bgp log-neighbor-changes
 neighbor 172.20.255.10 remote-as 101
 neighbor 172.20.255.10 update-source Loopback1
 neighbor 172.20.255.10 next-hop-self
 neighbor 203.0.112.2 remote-as 8359
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 203.0.112.0 255.255.252.0 Null0
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

R36 - LAMAS

```bash
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname R36
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip domain name lamas.ru
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
ip ssh time-out 60
ip ssh version 2
interface Loopback1
 ip address 172.20.255.1 255.255.255.255
interface Ethernet0/0
 ip address 192.71.0.6 255.255.255.252
interface Ethernet0/1
 ip address 198.51.96.1 255.255.255.252
interface Ethernet0/2
 ip address 198.51.96.5 255.255.255.252
interface Ethernet0/3
 ip address 198.51.96.9 255.255.255.252
interface Ethernet1/0
 no ip address
 shutdown
interface Ethernet1/1
 no ip address
 shutdown
interface Ethernet1/2
 no ip address
 shutdown
interface Ethernet1/3
 no ip address
 shutdown
router bgp 301
 bgp log-neighbor-changes
 network 198.51.96.0 mask 255.255.248.0
 neighbor 192.71.0.5 remote-as 520
 neighbor 198.51.96.2 remote-as 8359
 neighbor 198.51.96.2 soft-reconfiguration inbound
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 198.51.96.0 255.255.248.0 Null0
ip prefix-list BLOCK-TELNET seq 5 deny 40.0.0.0/8 le 32
ip prefix-list BLOCK-TELNET seq 10 permit 0.0.0.0/0 le 32
ip prefix-list ONLY-DEFAULT seq 5 permit 0.0.0.0/0
route-map FILTER-TELNET deny 10
 match ip address prefix-list BLOCK-TELNET
route-map FILTER-TELNET permit 20
route-map FILTER-ONLY-DEFAULT deny 10
 match ip address prefix-list ONLY-DEFAULT
route-map FILTER-ONLY-DEFAULT permit 20
control-plane
line con 0
 exec-timeout 0 0
 logging synchronous
line aux 0
line vty 0 4
 exec-timeout 15 0
 login local
 transport input ssh
end
```

VPN

```
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
hostname VPN
boot-start-marker
boot-end-marker
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180


no ip domain lookup
ip cef
no ipv6 cef
multilink bundle-name authenticated
redundancy
crypto ikev2 proposal IKEv2-GRE
 encryption aes-cbc-256
 integrity sha256
 group 21
crypto ikev2 policy IKEv2-POLICY
 proposal IKEv2-GRE
crypto ikev2 keyring IKEv2-PSK
 peer SPOKE
  address 0.0.0.0 0.0.0.0
  pre-shared-key OTUS
crypto ikev2 profile IKEv2-PROFILE
 match identity remote address 0.0.0.0
 authentication remote pre-share
 authentication local pre-share
 keyring local IKEv2-PSK
crypto ipsec transform-set IPSEC-TRANSFORM esp-aes 256 esp-sha256-hmac
 mode transport
crypto ipsec profile DMVPN-PROFILE
 set transform-set IPSEC-TRANSFORM
 set ikev2-profile IKEv2-PROFILE
interface Loopback1
 ip address 185.10.20.255 255.255.255.255
 ip ospf 1 area 1
interface Tunnel100
 ip address 10.1.0.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast dynamic
 ip nhrp network-id 100
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
 tunnel protection ipsec profile DMVPN-PROFILE
interface Ethernet0/0
 ip address 10.0.0.74 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 1
interface Ethernet0/1
 no ip address
 shutdown
interface Ethernet0/2
 no ip address
 shutdown
interface Ethernet0/3
 no ip address
 shutdown
router ospf 1
 router-id 10.2.0.30
 auto-cost reference-bandwidth 10000
 area 1 stub
 redistribute static subnets
 network 185.10.20.255 0.0.0.0 area 1
ip forward-protocol nd
no ip http server
no ip http secure-server
ip route 10.16.0.0 255.252.0.0 10.1.0.2
ip route 10.20.0.0 255.252.0.0 10.1.0.3
control-plane
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
end
```