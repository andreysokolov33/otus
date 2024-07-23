## Настройка списков ACL

Политика ACL для данной компании:

В какие сети какой VLAN имеет доступ, а также может ли устройство в данном VLAN выходить в Интернет

| VLAN | Название | Из какого VLAN ессть доступ | В Интернет |
| --- | --- | --- | --- |
| 55 | СКУД | 77, 80, 88, 89, 200 | Нет |
| 56 | IP камеры | 77, 80, 88, 89, 200 | Нет |
| 57 | Пост охраны | 77, 80, 88, 89, 100, 200 | Да |
| 60 | Ресепшн | 77, 80, 88, 89, 93, 100, 200 | Да |
| 77 | IT отдел | 77, 80, 88, 89, 100, 200 | Да |
| 80 | Серверная | <div>Полный доступа: 77, 88, 89</div> <div>Для всех порты: 67, 68, 53, 21</div> | Да |
| 88 | Ремонтный  | 77, 80, 88, 89, 100, 200 | Да |
| 89 | Тестировщики | 77, 80, 88, 89, 100, 200 | Да |
| 90 | HR | 77, 80, 88, 89, 93, 100, 200 | Да |
| 91 | Юристы | 77, 80, 88, 89, 93, 100, 200 | Да |
| 92 | Бухгалтерия | 77, 80, 88, 89, 93, 100, 200 | Да |
| 93 | Администрация | 77, 80, 88, 89, 100, 200 | Да |
| 100 | DMZ | <div>Полный доступа: 77, 80, 88, 89</div> <div>Для всех порты: 21, 80, 443, SMTP, POP3, ICMP</div> | Да |
| 111 | WiFi Emploees | 77, 80, 88, 89, 100, 200 | Да |
| 125 | Датчики дыма | 77, 80, 88, 89, 200 | Нет |
| 130 | WiFi Guest | 77, 80, 88, 89, 100, 200 | Да |
| 200 | Management | 77, 80, 88, 89, 200 | Да |

Для запрета доступа одной подсети в другую будут использоваться стандартные ACL, которые работают по SRC IP. Списки будут назначаться на нужный VLAN на L3 коммутаторах в направлении OUT, так как это самая близкая к источнику назначения точка в топологии. Также не всем подсетям дается полный доступ в подсеть серверной и DMZ, для них открыты лишь определенный порты, например, порты DHCP сервера, FTP, WEB и тд - это уже расширенные списки.

Списки правил одинаковые для CORE1-SW и CORE2-SW.

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

remark Deny Others VLANs
deny 10.10.0.0 0.0.15.255 

remark Permit Internet Resourses
permit any 



interface vlan 57
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 77
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 88
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 89
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 93
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 111
ip access-group PERMIT-TECH-AND-DMZ out
interface vlan 130
ip access-group PERMIT-TECH-AND-DMZ out
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

remark Deny Others VLANs
deny 10.10.0.0 0.0.15.255 

remark Permit Internet Resourses
permit any 




interface vlan 60
ip access-group PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 90
ip access-group PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 91
ip access-group PERMIT-TECH-DMZ-AND-ADMIN out
interface vlan 92
ip access-group PERMIT-TECH-DMZ-AND-ADMIN out
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
ip access-group PERMIT-ONLY-TECH out
interface vlan 56
ip access-group PERMIT-ONLY-TECH out
interface vlan 125
ip access-group PERMIT-ONLY-TECH out
interface vlan 200
ip access-group PERMIT-ONLY-TECH out
```


Список, который разрешает полный доступ из сети IT специалистов (IT отдел, ремонтный отдел и тестировщики), сети управления, Серверной. Также для всех открыты порты 67-68 (DHCP), 53 (DNS), 21 (FTP). Предназначен для VLAN 80. Последним правилом идет доступ разрешающее для всех не из офисной сети. Нужно для ICMP трафика во внешнюю сеть, для прочих запросов.

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

remark Deny Others VLANs
deny ip 10.10.0.0 0.0.15.255 10.10.10.0 0.0.0.31

remark Permit Internet Resourses
permit ip any 10.10.10.0 0.0.0.31



interface vlan 80
ip access-group PERMIT-SERVER out
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
ip access-group PERMIT-DMZ out
```

ACL для внешних интерфейсов L3 комутаторов, чтобы контролировать доступ сетям в Интернет. Это стандартный ACL, который назначается на физические порты CORE1-SW и CORE2-SW Gi1/0/4-5. Чтобы лист был проще и быстрее обрабатывался, в Интернет могут выходить все сети, кроме тех, что запрещены:

```
ip access-list standard PERMIT-INTERNET
remark Deny SCUD
deny 10.10.1.0 0.0.0.31
remark Deny IP Cameras
deny 10.10.2.0 0.0.0.255
remark Deny SmokeRadar
deny 10.10.10.32 0.0.0.31
remark Permit Other Traffic
permit any



interfacerange Gi1/0/4-5
ip access-group PERMIT-INTERNET out
```



Далее: [Настройка NAT и доступа во внешнюю сеть](./nat_settings.md)

Назад: [Настройка головного офиса](./main_office.md)