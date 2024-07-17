## Настройка VLAN и межвлановой маршрутизации

1. Создание VLAN на коммутаторах

Настройка корневых L3 коммутаторов (CORE-1SW и CORE-2SW)

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

```

Настройка CORE-2SW:

```

```



Назад: [Настройка головного офиса](./main_office.md)