# ЛР 7. ISIS

## 1. Цели работы

1. Настроите IS-IS в ISP Триада.
2. R23 и R25 находятся в зоне 2222.
3. R24 находится в зоне 24.
4. R26 находится в зоне 26.

## 2. Топология сети

![Alt text](./topology.png)

Рисунок 1. Топология сети

## 3. Настройка IS-IS

R23 и R24 в зоне 2222. Между ними будет соседства уровня L1. R23 с R25 будет в соседстве L2, R24 с R26 в соседстве L2 и R25 с R26 тоже в соседстве L2.

### Настрйока зоны 2222

NET R23: 49.2222.1921.6820.0001.00
NET R25: 49.2222.1921.6820.0002.00

R23

```bash
router isis 1
 net 49.2222.1921.6820.0001.00
 passive-interface e0/0

interface loopback 1
 ip address 192.168.200.1 255.255.255.255

interface Ethernet0/0
 ip address 192.0.2.10 255.255.255.252
 ip router isis 1

interface Ethernet0/1
 ip address 172.20.0.1 255.255.255.252
 ip router isis 1
 isis circuit-type level-1

interface Ethernet0/2
 ip address 172.20.0.5 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only
```

R25

```bash
router isis 1
 net 49.2222.1921.6820.0002.00
 passive-interface default
 no passive-interface Ethernet0/0
 no passive-interface Ethernet0/2

interface loopback 1
 ip address 192.168.200.2 255.255.255.255
 ip router isis 1

interface Ethernet0/1
 ip address 203.0.113.17 255.255.255.252
 ip router isis 1

interface Ethernet0/0
 ip address 172.20.0.2 255.255.255.252
 ip router isis 1
 isis circuit-type level-1

interface Ethernet0/2
 ip address 172.20.0.13 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only

interface Ethernet0/3
 ip address 203.0.113.13 255.255.255.252
 ip router isis 1
```

Результат команды `show isis neighbor` на R23 и R25

![Alt text](./r23-neigh-1.png)

![Alt text](./r25-neigh-1.png)

Соседство уровня L1 в UP

Полученные по протоколу ISIS маршруты на R23

![Alt text](./r23-ip-route.png)

И R25

![Alt text](./r25-ip-route.png)

### Настрйока зоны 24

NET R24: 49.0024.1921.6820.0003.00

R24

```bash
router isis 1
 net 49.0024.1921.6820.0003.00
 passive-interface default
 no passive-interface Ethernet0/0
 no passive-interface Ethernet0/1
 no passive-interface Ethernet0/2
 no passive-interface Ethernet0/3
 no passive-interface Loopback1

interface loopback 1
 ip address 192.168.200.3 255.255.255.255
 ip router isis 1

interface Ethernet0/0
 ip address 198.51.100.6 255.255.255.252
 ip router isis 1

interface Ethernet0/1
 ip address 172.20.0.9 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only

interface Ethernet0/2
 ip address 172.20.0.6 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only

interface Ethernet0/3
 ip address 203.0.113.1 255.255.255.252
 ip router isis 1
```

Результат команды `show isis neighbor` на R24

![Alt text](./r24-isis-neigh.png)

Полученные по протоколу ISIS маршруты на R24

![Alt text](./r24-ip-route.png)

### Настрйока зоны 26

NET R26: 49.0026.1921.6820.0004.00

R26

```bash
router isis 1
 net 49.0026.1921.6820.0004.00
 passive-interface default
 no passive-interface Ethernet0/0
 no passive-interface Ethernet0/2
 no passive-interface Loopback1

interface Loopback1
 ip address 192.168.200.4 255.255.255.255
 ip router isis 1

interface Ethernet0/0
 ip address 172.20.0.10 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only

interface Ethernet0/1
 ip address 203.0.113.9 255.255.255.252

interface Ethernet0/2
 ip address 172.20.0.14 255.255.255.252
 ip router isis 1
 isis circuit-type level-2-only

interface Ethernet0/3
 ip address 203.0.113.5 255.255.255.252
```

Результат команды `show isis neighbor` на R26

![Alt text](./r26-isis-neigh.png)

Полученные по протоколу ISIS маршруты на R26

![Alt text](./r26-ip-route.png)

## Тест

В таблицах маршрутизации роутеров появились маршруты, переданные протоколом ISIS. Для теста с R23 пущен пинг до интерфейса e0/1 R26. Пинг прошел успешно, связанность в сети "Триада" имеется.

![Alt text](./r23-ping-r26.png)
