## Настройка VLAN и межвлановой маршрутизации

1. Создание VLAN на коммутаторах

Настройка корневых L3 коммутаторов (**CORE-1SW** и **CORE-2SW**)

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 57 
name Guards
vlan 60 
name Reception
vlan 77 
name ITDepartment
vlan 80 
name Server
vlan 88 
name Repair
vlan 89 
name Testing 
vlan 90 
name HR
vlan 91 
name Juridical
vlan 92 
name Accounting
vlan 93 
name Administration
vlan 100
name DMZ
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 130
name VLAN0130
vlan 200
name Management
vlan 573
name Native 
```

Настройка коммутатора **ОХРАНА**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 57 
name Guards
vlan 80 
name Server
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **РЕСЕПШН**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 60 
name Reception
vlan 80 
name Server
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 130
name VLAN0130
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **СЕРВЕРНАЯ**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 57 
name Guards
vlan 60 
name Reception
vlan 77 
name ITDepartment
vlan 80 
name Server
vlan 88 
name Repair
vlan 89 
name Testing 
vlan 90 
name HR
vlan 91 
name Juridical
vlan 92 
name Accounting
vlan 93 
name Administration
vlan 100
name DMZ
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 130
name VLAN0130
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **IT-ОТДЕЛ**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 77 
name ITDepartment
vlan 80 
name Server
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **РЕМОНТНЫЙ ОТДЕЛ**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 80 
name Server
vlan 88 
name Repair
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **ТЕСТИРОВЩИКИ**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 80 
name Server
vlan 89 
name Testing 
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **HR/Юристы**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 80 
name Server
vlan 90 
name HR
vlan 91 
name Juridical
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

Настройка коммутатора **БУХГАЛРЕТИЯ/АДМИНИСТРАЦИЯ**

```
vlan 55 
name SKUD
vlan 56 
name IPCamera
vlan 80 
name Server
vlan 92 
name Accounting
vlan 93 
name Administration
vlan 111
name WiFiEmploees
vlan 125
name SmokeRadar
vlan 130
name VLAN0130
vlan 200
name Management
vlan 357
name ParkingLot
vlan 573
name Native 
```

2. Настройка межвлановой маршрутизации

Для межвлановой маршрутизации все порты коммутаторов, которые ведут на CORE-1SW и CORE-2SW, переведены в trunk, через них пропускаются нужные VLAN. Также назначен Native VLAN 573. На CORE-1SW, CORE-2SW созданы интерфейсы с номерами VLAN, на них назначены IP адреса. Так как предполагается использовать HSRP протокол, то для реализации ROAS второй адрес в подсети VLAN используется для CORE-1SW, а третий для CORE-2SW. Первый оставлен под HSRP.

Настройка CORE-1SW:

```
interface Vlan55
ip address 10.10.1.2 255.255.255.224

interface Vlan56
ip address 10.10.2.2 255.255.255.0

interface Vlan57
ip address 10.10.1.130 255.255.255.128

interface Vlan60
ip address 10.10.1.66 255.255.255.192

interface Vlan77
ip address 10.10.12.2 255.255.254.0

interface Vlan80
ip address 10.10.10.2 255.255.255.224

interface Vlan88
ip address 10.10.14.2 255.255.255.0

interface Vlan89
ip address 10.10.15.2 255.255.255.0

interface Vlan90
ip address 10.10.3.2 255.255.255.192

interface Vlan91
ip address 10.10.3.66 255.255.255.192

interface Vlan92
ip address 10.10.3.130 255.255.255.192

interface Vlan93
ip address 10.10.3.194 255.255.255.192

interface Vlan100
ip address 10.10.1.34 255.255.255.224

interface Vlan111
ip address 10.10.4.2 255.255.252.0

interface Vlan125
ip address 10.10.10.34 255.255.255.224

interface Vlan130
ip address 10.10.8.2 255.255.254.0

interface Vlan200
ip address 10.10.10.130 255.255.255.128
```

Настройка CORE-2SW:

```
interface Vlan55
ip address 10.10.1.3 255.255.255.224

interface Vlan56
ip address 10.10.2.3 255.255.255.0

interface Vlan57
ip address 10.10.1.131 255.255.255.128

interface Vlan60
ip address 10.10.1.67 255.255.255.192

interface Vlan77
ip address 10.10.12.3 255.255.254.0

interface Vlan80
ip address 10.10.10.3 255.255.255.224

interface Vlan88
ip address 10.10.14.3 255.255.255.0

interface Vlan89
ip address 10.10.15.3 255.255.255.0

interface Vlan90
ip address 10.10.3.3 255.255.255.192

interface Vlan91
ip address 10.10.3.67 255.255.255.192

interface Vlan92
ip address 10.10.3.131 255.255.255.192

interface Vlan93
ip address 10.10.3.195 255.255.255.192

interface Vlan100
ip address 10.10.1.35 255.255.255.224

interface Vlan111
ip address 10.10.4.3 255.255.252.0

interface Vlan125
ip address 10.10.10.35 255.255.255.224

interface Vlan130
ip address 10.10.8.3 255.255.254.0

interface Vlan200
ip address 10.10.10.131 255.255.255.128
```

В разделе настройки DHCP сервера для тех VLAN, где он предусмотрен, одним из параметров будет маршрут по умолчанию. В качестве такого маршрута будет указан адрес виртуального маршрутизатора для каждого VLAN, которые реализуются через протокол HSRP. После HSRP можно будет считать настройку ROAS законченной.

Далее: [Настройка HSRP](./hsrp_settings.md)

Назад: [Настройка головного офиса](./main_office.md)