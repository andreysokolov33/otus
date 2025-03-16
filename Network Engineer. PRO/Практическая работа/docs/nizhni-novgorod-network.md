# Састройка сети в филиале Владимира

В филиале Нижнего-Новгорода будет настроен

1. DHCPv4
2. VRRP
3. NTP
4. DmVPN + IPSec

## Настройка маршрутизаторов

NNGW1

```bash
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.20.1.2 255.255.255.0
 ip helper-address 10.20.2.10
 standby 10 ip 10.20.1.1
 standby 10 priority 150
 standby 10 preempt

interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.20.2.2 255.255.255.0
 ip helper-address 10.20.2.10
 standby 20 ip 10.20.2.1
 standby 20 preempt

interface Ethernet0/3
 no ip address

router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

ntp server 10.20.2.10 
```

NNGW2

```bash
interface Loopback1
 ip address 10.20.0.2 255.255.255.255

interface Ethernet0/0
 ip address 104.16.0.10 255.255.255.252

interface Ethernet0/1
 ip address 10.20.99.2 255.255.255.252

interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.20.1.3 255.255.255.0
 ip helper-address 10.20.2.10
 standby 10 ip 10.20.1.1
 standby 10 preempt

interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.20.2.3 255.255.255.0
 ip helper-address 10.20.2.10
 standby 20 ip 10.20.2.1
 standby 20 priority 150
 standby 20 preempt

router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

ntp server 10.20.2.10
```

NN-DHCP-NAT

```bash
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

ntp master 1
```

## Настройка коммутаторов

SW11

```bash
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk

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

interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk

vlan 10
 name Director
vlan 20
 name Manager
vlan 999
 name Management
vlan 1000
 name Parking_Lot
```

SW12

```bash
interface Ethernet0/0
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk

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

interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk

vlan 10
 name Director
vlan 20
 name Manager
vlan 999
 name Management
vlan 1000
 name Parking_Lot
```

На маршрутизаторах VladimirGW-1 и VladimirGW-2 настроен VRRP

![Alt text](./images/nngw1-show-standby-brief.png)

На SW9 и SW10 настроен Etherchannel

![Alt text](./images/sw11-etherchannel-summary.png)

В сети действует DHCPv4. Пример получения IP на клиенте VPC46

![Alt text](./images/vpc50-dhcp.png)