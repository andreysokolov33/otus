# Базовый конфиг на всех утройствах

В базовые настройки маршрутизаторов, которые не будут отображены в других разделах с целью уменьшения повторений, относятся следующие:

```bash
enable
configure terminal

hostname <HOSTNAME>
no ip domain-lookup

ip domain-name nn.ru
crypto key generate rsa general-keys modulus 1024
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3 

line console 0
 logging synchronous
 exec-timeout 0 0

line vty 0 4
 transport input ssh
 login local
 exec-timeout 15 0

interface range e0/0-3
no shutdown

do wr
```

Настройка коммутаторов:

```bash
vlan 100
name Management

interface vlan 100
 ip address 192.168.10.2 255.255.255.0
 no shutdown
```


Далее: [Распределение адресного пространства](./addresses.md)

Назад: [Оглавление](../README.md)
