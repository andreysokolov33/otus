# ЛР 6. OSPF

## 1. Цели работы

- Настроить OSPF офисе Москва
- Разделить сеть на зоны
- Настроить фильтрацию между зонами

1. Маршрутизаторы R12-R13 находятся в зоне 0 - backbone.
2. Маршрутизаторы R14-R15 находятся в зоне 10. Тип NSSA
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.

## 2. Топология сети

![Alt text](./topology.png)

Рисунок 1. Топология сети

## 3. Настройка OSPF

Так как схему немного переделал на этапе планирования сети, то backbone зона теперь на R12-R13. Из зоны 10 на R14-R15 приходят маршруты в Интернет

Area 101 - ей нужен только маршрут по умолчанию - Tatally Stub Area
Area 102 - Stub Area, но с фильтрацией маршрута до Area 101

### Настройка Backbone Area ( Area 0)

R12

```bash
route ospf 1

interface range e0/2-3
 ip ospf 1 area 0
 ip ospf network point-to-point
```

R13

```bash
route ospf 1

interface range e0/2-3
 ip ospf 1 area 0
 ip ospf network point-to-point
```

R14

```bash
route ospf 1

interface range e0/0-1
 ip ospf 1 area 0
 ip ospf network point-to-point
```

R15

```bash
route ospf 1

interface range e0/0-1
 ip ospf 1 area 0
 ip ospf network point-to-point
```

После этого все маршрутизаторы R12-R15 установили соседство. Тип сети "point-to-point", поэтому нет выбора DR и BDR на данных линках:

![Alt text](./r14-ip-ospf-neighbor.png)

Маршруты OSPF в таблице маршрутизации R14

![Alt text](./r14-ip-route.png)

Есть все линковые маршруты других интерфейсов

Также L3 коммутаторы SW2 и SW3 входят в Area 0

SW2

```bash
router ospf 1
 passive-interface default
 no passive-interface GigabitEthernet1/0
 no passive-interface GigabitEthernet1/1
 network 10.4.16.0 0.0.3.255 area 0
 network 10.4.18.0 0.0.3.255 area 0

interface range gi1/0-1
 ip ospf 1 area 0
 ip ospf network point-to-point
```

SW3

```bash
router ospf 1
 passive-interface default
 no passive-interface GigabitEthernet1/0
 no passive-interface GigabitEthernet1/1
 network 10.4.16.0 0.0.3.255 area 0
 network 10.4.18.0 0.0.3.255 area 0

interface range gi1/0-1
 ip ospf 1 area 0
 ip ospf network point-to-point
```

SW2 и SW3 получили маршруты OSPF

![Alt text](./sw2-show-ip-route.png)

Теперь клиенты с VPC могут пинговать любое устройство в данной OSPF сети

![Alt text](./PC7-ping-to-r15.png)

### Настройка NSSA ( Area 10)

До этого в предыдущих работах просто настраивались адреса на линках с провайдером. Теперь на R14 и R15 прописаны маршруты по умолчанию через провайдера. Они же будут распространяться в OSPF, сделав R14 и R15 ASBR для всей сети.

R14

```bash
ip route 0.0.0.0 0.0.0.0 192.0.2.1

router ospf 1
 area 10 nssa
 default-information originate
```

R15

```bash
ip route 0.0.0.0 0.0.0.0 198.51.100.1

router ospf 1
 area 10 nssa
 default-information originate
```

Таблица маршрутизации на R12

![Alt text](./r12-show-ip-route.png)

Таблица маршрутизации на R20

![Alt text](./r20-show-ip-route-1.png)

На роутерах появился маршрут по умолчанию типа Е2. На R12 выполняется ECMP, так как стоимость маршрутов до кадого ASBR одинаковая.

### Настройка Totally Stub Area ( Area 101)

R19

```bash
router ospf 1
 area 101 stub
 passive-interface Ethernet0/1
 passive-interface Ethernet0/2
 passive-interface Ethernet0/3

interface Ethernet0/0
 ip ospf 1 area 101
```

R12

```bash
router ospf 1
 area 101 stub no-summary

interface Ethernet0/1
 ip ospf 1 area 101
```

Таблица маршрутизации на R19. Есть только 1 маршрут, ведущий через R12

![Alt text](./r19-ip-route.png)

### Настройка Stub Area ( Area 102)

Для фильтрации нужных префиксов на R13 надо будет создать префикс лист и указать, что он применяется для нужной area 102. Данная area будет stub.

R13

```bash
ip prefix-list area_102 seq 5 deny 10.4.20.0/24
ip prefix-list area_102 seq 10 deny 10.4.21.0/24
ip prefix-list area_102 seq 15 deny 10.4.22.0/24
ip prefix-list area_102 seq 20 permit 0.0.0.0/0 le 32

router ospf 1
 area 102 stub
 area 102 filter-list prefix area_102 in

interface e0/0
 ip ospf 1 area 102
 ip ospf network point-to-point
```

R20

```bash
router ospf 1
 area 102 stub
 passive-interface Ethernet0/1
 passive-interface Ethernet0/2
 passive-interface Ethernet0/3

interface e0/0
 ip ospf 1 area 102
 ip ospf network point-to-point
```

Префиксы на R19

![Alt text](./r19-show-ip-ospf-int.png)

Маршруты на R20

![Alt text](./r20-show-ip-route.png)

Нужные префиксы отфильтрованы.
