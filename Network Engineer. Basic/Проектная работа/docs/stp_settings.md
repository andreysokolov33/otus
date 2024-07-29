## Настройка STP

Работа в сети ведется по протоколу Rapid-PVST. Согласно [заданию](zadanie.md#3-настроить-балансировку-нагрузки-через-stp) трафик определенных VLAN надо пускать на определенный L3 коммутатор с целью балансировки нагрузки. Для этого вручную для каждого VLAN прописан первичный и вторичный ROOT коммутатор.

Настройка CORE1-SW:

```
spanning-tree mode rapid-pvst
spanning-tree vlan 55-56,60,90-93,111,125,130 root primary 
spanning-tree vlan 57,77,80,88-89,100,200 root secondary 
```

Настройка CORE2-SW:

```
spanning-tree mode rapid-pvst
spanning-tree vlan 57,77,80,88-89,100,200 root primary
spanning-tree vlan 55-56,60,90-93,111,125,130 root secondary
```

На всех коммутаторах уровня распределения на все ACCESS порты включен **portfast** и **bpduguard**:

```
spanning-tree mode rapid-pvst
spanning-tree portfast
spanning-tree bpduguard enable
```


Далее: [Настройка DHCP](./dhcp_settings.md)

Назад: [Настройка головного офиса](./main_office.md)