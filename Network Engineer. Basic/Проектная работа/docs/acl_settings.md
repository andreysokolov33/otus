## Настройка списков ACL

Политика ACL для данной компании:

В какие сети какой VLAN имеет доступ, а также может ли устройство в данном VLAN выходить в Интернет

| VLAN | Название | В сеть какого VLAN | В Интернет |
| --- | --- | --- | --- |
| 55 | СКУД | - | Нет |
| 56 | IP камеры | - | Нет |
| 57 | Пост охраны | 60, 80, 90, 91, 92, 93 | Да |
| 60 | Ресепшн | 57, 80, 90, 91, 92, 93, 111, 130 | Да |
| 77 | IT отдел | Все VLAN | Да |
| 80 | Серверная | Все VLAN | Да |
| 88 | Ремонтный  | Все VLAN | Да |
| 89 | Тестировщики | Все VLAN | Да |
| 90 | HR | 57, 60, 80, 91, 92, 93, 111, 130 | Да |
| 91 | Юристы | 57, 60, 80, 90, 92, 93, 111, 130 | Да |
| 92 | Бухгалтерия | 57, 60, 80, 90, 91, 93, 111, 130 | Да |
| 93 | Администрация | 57, 60, 80, 90, 91, 92, 111, 130 | Да |
| 100 | DMZ | 57, 77, 80, 88, 89 | Да |
| 111 | WiFi Emploees | 57, 60, 80, 88, 89, 90, 91, 92, 93, 100 | Да |
| 125 | Датчики дыма | - | Нет |
| 130 | WiFi Guest | 60, 80, 90, 91, 92, 93 | Да |
| 200 | Management | -  | Нет |

Для запрета доступа одной подсети в другую будут использоваться стандартные ACL, которые работают по SRC IP. Списки будут назначаться на нужный VLAN на L3 коммутаторах в направлении OUT, так как это самая близкая к источнику назначения точка в топологии. Также не всем подсетям дается полный доступ в подсеть серверной и DMZ, для них открыты лишь определенный порты, например, порты DHCP сервера, FTP, WEB и тд - это уже расширенные списки.

Списки правил одинаковые для CORE1-SW и CORE2-SW.

Политика правил будет - **запрещено все, что не разрешено**.

[Адреса подсетей](./addressing.md#vlans)

Список, который разрешает лишь сетям IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления (Management), Серверной и DMZ доступ в данную сеть. Предназначен на VLAN 57, 77, 88, 89, 93, 111, 130.

```
ip access-list standard PERMIT-TECH-AND-DMZ

remark IT, Repair, Testing VLANs
permit 10.10.12.0 0.0.3.255

remark Server VLAN
permit 10.10.10.0 0.0.0.31

remark Management VLAN
permit 10.10.10.128 0.0.0.127

remark DMZ VLAN
permit 10.10.1.32 0.0.0.31


interface vlan 57
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 77
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 88
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 89
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 93
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 111
ip acces PERMIT-TECH-AND-DMZ out
interface vlan 130
ip acces PERMIT-TECH-AND-DMZ out
```

Список, который разрешает доступ IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления (Management), Серверной, а также устройствам в сети администрации доступ в данную сеть. Предназначен для VLAN 60, 90, 91, 92.

```
ip access-list standard PERMIT-TECH-DMZ-AND-ADMIN

remark IT, Repair, Testing VLANs
permit 10.10.12.0 0.0.3.255

remark Server VLAN
permit 10.10.10.0 0.0.0.31

remark Management VLAN
permit 10.10.10.128 0.0.0.127

remark Administration VLAN
permit 10.10.3.192 0.0.0.63

remark DMZ VLAN
permit 10.10.1.32 0.0.0.31



interface vlan 60
ip acces PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 90
ip acces PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 91
ip acces PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 92
ip acces PERMIT-TECH-DMZ-AND-ADMIN out
```

Список, который разрешает доступ IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления (Management) и Серверной. Предназначен для VLAN 55, 56, 125, 200.

```
ip access-list standard PERMIT-ONLY-TECH

remark IT, Repair, Testing VLANs
permit 10.10.12.0 0.0.3.255

remark Server VLAN
permit 10.10.10.0 0.0.0.31

remark Management VLAN
permit 10.10.10.128 0.0.0.127



interface vlan 55
ip acces PERMIT-ONLY-TECH out
interface vlan 56
ip acces PERMIT-ONLY-TECH out
interface vlan 125
ip acces PERMIT-ONLY-TECH out
interface vlan 200
ip acces PERMIT-ONLY-TECH out
```


Список, который разрешает полный доступ из сети IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления, Серверной. Также для всех открыты порты 67-68 (DHCP), 53 (DNS), 21 (FTP), ICMP трафик. Предназначен для VLAN 80.

```
ip access-list extended PERMIT-SERVER

remark Full Access For IT Department and Management
permit ip 10.10.12.0 0.0.1.255 10.10.10.0 0.0.0.31

remark Server VLAN
permit ip 10.10.10.0 0.0.0.31 10.10.10.0 0.0.0.31

remark Management VLAN
permit ip 10.10.10.128 0.0.0.127 10.10.10.0 0.0.0.31

remark Open DHCP Server for all
permit udp any host 10.10.10.20 range 67 68 

remark Open DNS Server for all
permit udp any host 10.10.10.10 eq 53

remark Open FTP Server for all
permit udp any host 10.10.10.30 eq 21

remark Permit ICMP
permit icmp any 10.10.1.32 0.0.0.31



interface vlan 80
ip acces PERMIT-SERVER out
```

Список, который разрешает полный доступ из сети IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления, Серверной. Также для всех открыты порты 80 и 443 (HTTp, HTTPS), 21 (FTP), SMTP и POP3, ICMP. Предназначен для VLAN 100.

```
ip access-list extended PERMIT-DMZ

remark Full Access For IT Department and Management
permit ip 10.10.12.0 0.0.1.255 10.10.1.32 0.0.0.31

remark Server VLAN
permit ip 10.10.10.0 0.0.0.31 10.10.1.32 0.0.0.31

remark Management VLAN
permit ip 10.10.10.128 0.0.0.127 10.10.1.32 0.0.0.31

remark Permit Everyone Access To WEB 443
permit tcp any host 10.10.1.36 eq 443

remark Permit Everyone Access To WEB 80
permit tcp any host 10.10.1.36 eq www

remark Permit Everyone Access To FTP
permit tcp any host 10.10.1.37 eq 21

remark Permit Everyone Access To EMAIL SMTP
permit tcp any host 10.10.1.38 eq smtp

remark Permit Everyone Access To EMAIL POP3
permit tcp any host 10.10.1.38 eq pop3

remark Permit ICMP
permit icmp any 10.10.1.32 0.0.0.31



interface vlan 100
ip acces PERMIT-DMZ out
```



Далее: [Настройка NAT и доступа во внешнюю сеть](./nat_settings.md)

Назад: [Настройка головного офиса](./main_office.md)