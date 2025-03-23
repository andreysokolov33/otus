# Састройка сети в филиале Владимира

В филиале Владимира будет настроен

1. DHCPv4
2. VRRP
3. NTP
4. DmVPN + IPSec

## Настройка маршрутизаторов

VladimirGW_1

```bash
interface Loopback1
 ip address 10.16.0.1 255.255.255.255

interface Ethernet0/0
 ip address 10.16.255.1 255.255.255.252

interface Ethernet0/1
 ip address 198.51.96.10 255.255.255.252

interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.16.1.2 255.255.255.0
 ip helper-address 10.16.2.10
 standby 10 ip 10.16.1.1
 standby 10 preempt

interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.16.2.2 255.255.255.0
 ip helper-address 10.16.2.10
 standby 20 ip 10.16.2.1
 standby 20 priority 150
 standby 20 preempt

interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native

router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

ip route 0.0.0.0 0.0.0.0 198.51.96.9
ip route 0.0.0.0 0.0.0.0 10.16.255.2 100 
```

VladimirGW_2

```bash
interface Loopback1
 ip address 10.16.0.2 255.255.255.255

interface Ethernet0/0
 ip address 198.51.96.6 255.255.255.252

interface Ethernet0/1
 ip address 10.16.255.2 255.255.255.252

interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 10.16.1.3 255.255.255.0
 ip helper-address 10.16.2.10
 standby 10 ip 10.16.1.1
 standby 10 priority 150
 standby 10 preempt

interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 10.16.2.3 255.255.255.0
 ip helper-address 10.16.2.10
 standby 20 ip 10.16.2.1
 standby 20 preempt

interface Ethernet0/2.1000
 encapsulation dot1Q 1000 native

router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

ip route 0.0.0.0 0.0.0.0 198.51.96.5
ip route 0.0.0.0 0.0.0.0 10.16.255.1 100
```

V-DHCP-NTP

```bash
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

interface Ethernet0/0
 ip address 10.16.2.10 255.255.255.0
 duplex auto

router ospf 1
 network 10.0.0.0 0.255.255.255 area 0
```

## Настрйока коммутаторв

SW9

```bash
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

interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
```

SW10

```bash
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

interface Port-channel1
 switchport trunk allowed vlan 10,20,999
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
```

На маршрутизаторах VladimirGW-1 и VladimirGW-2 настроен VRRP

![Alt text](./images/vladimir-gw1-show-standby-brief.png)

На SW9 и SW10 настроен Etherchannel

![Alt text](./images/sw9-etherchannel-summary.png)

В сети действует DHCPv4. Пример получения IP на клиенте VPC46

![Alt text](./images/vpc46-dhcp.png)