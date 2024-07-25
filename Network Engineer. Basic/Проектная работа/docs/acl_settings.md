## Настройка списков ACL

Политика ACL для данной компании:

В какие сети какой VLAN имеет доступ, а также может ли устройство в данном VLAN выходить в Интернет

Списками ACL закрыт полный доступ в серверную, в DMZ зону и на камеры, СКУД и датчики дыма. Также запрещается доступ в Интернет тем, кто не должен выходить в сеть


ACL для серверной (VLAN 80):
1. Для IT Department разрешен полный доступ
2. Для Management сети открыт порт 22
3. Открыты порты до DHCP, DNS, FTP серверов для всех
4. Разрешен пинк РС админа
5. Разрешем трафик из DMZ зоны через HTTP, HTTPS, SMTP, POP3
6. Разрешен RADIUS трафик
7. Разрешены все установленные TCP соединения
8. Остальное запрещено

```
ip access-list extended VLAN-80
remark Full Access For IT Department and SSH For Management
permit ip 10.10.12.0 0.0.1.255 any
permit tcp 10.10.10.128 0.0.0.127 any eq 22

remark Open DHCP, DNS, FTP Server for all
permit udp any host 10.10.10.20 range 67 68 
permit udp any host 10.10.10.10 eq 53
permit udp any host 10.10.10.30 eq 21

remark Permit ICMP from any to Admin PC
permit icmp any host 10.10.10.4 

remark Permit DMZ HTTP and HTTPS
permit tcp host 10.10.1.36 eq 80 host 10.10.10.4
permit tcp host 10.10.1.36 eq 443 host 10.10.10.4

remark permit DMZ EMAIL
permit tcp host 10.10.1.38 eq smtp host 10.10.10.4
permit tcp host 10.10.1.38 eq pop3 host 10.10.10.4

remark permit RADIUS Traffic
permit udp any host 10.10.10.40 range 1812 1813

remark Permit Estableshed Connections
permit tcp any any established 
```

Доступ на датчики дыма, камеры и СКУД разрешен лишь отделу IT Department и Testing, а также сети Management

```
ip access-list standard NO-INTERNET
remark Full Access For IT Department, Testing, Management
permit 10.10.12.0 0.0.1.255
permit 10.10.15.0 0.0.0.255
permit 10.10.10.128 0.0.0.127
```

Для контрля доступа в DMZ зону используется следующий список:
1. Разрешен полный доступ IT Department
2. Разрешен SSH доступ через сеть Management
3. Открыты порты для всех FTP, SMTP, POP3, HTTP, HTTPS
4. Разрешены установленные TCP соединения

```
ip access-list extended DMZ
remark Full Access For IT Department, Management
permit ip 10.10.12.0 0.0.1.255 any
permit tcp 10.10.10.128 0.0.0.127 any eq 22

remark Permit For All Email, FTP, WEB
permit tcp any host 10.10.1.36 eq 80 
permit tcp any host 10.10.1.36 eq 443
permit tcp any host 10.10.1.38 eq smtp 
permit tcp any host 10.10.1.38 eq pop3 
permit tcp any host 10.10.1.37 eq 21 

remark Permit Estableshed Connections
permit tcp any any established 
```

На NAT Layer маршрутизаторах есть список, запрещающий доступ датчикам дыма, камерам и СКУДу доступ в Интернет

```
ip access-list standard DENY-INTERNET
remark Deny Access To Internet
deny 10.10.1.0 0.0.0.31
deny 10.10.2.0 0.0.0.255
deny 10.10.10.32 0.0.0.31
permit any
```

Применение списков к интерфейсам на L3 коммутаторах:

```
interface vlan 80
ip access-list VLAN-80 out

interface vlan 55
ip access-list NO-INTERNET out

interface vlan 56
ip access-list NO-INTERNET out

interface vlan 125
ip access-list NO-INTERNET out

interface vlan 100
ip access-list DMZ out
```

Настройка на NAT Layer маршрутизаторах:

```
interface range fa1/0-1
ip access-list DENY-INTERNET in
```

Далее: [Настройка NAT и доступа во внешнюю сеть](./nat_settings.md)

Назад: [Настройка головного офиса](./main_office.md)