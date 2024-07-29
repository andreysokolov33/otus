## Настройка HSRP на L3 коммутаторах

Для повышения надежности схемы на L3 коммутаторах настроен протокол HSRP для каждого интерфейса VLAN. В качестве адреса для виртуального маршрутизатора выбран первый IP адрес подсети.

Настройки на CORE1-SW:

```
interface Vlan55
standby version 2
standby 55 ip 10.10.1.1
standby 55 priority 120
standby 55 preempt

interface Vlan56
standby version 2
standby 56 ip 10.10.2.1
standby 56 priority 120
standby 56 preempt

interface Vlan57
standby version 2
standby 57 ip 10.10.1.129
standby 57 preempt

interface Vlan60
standby version 2
standby 60 ip 10.10.1.65
standby 60 priority 120
standby 60 preempt

interface Vlan77
standby version 2
standby 77 ip 10.10.12.1
standby 77 preempt

interface Vlan80
standby version 2
standby 80 ip 10.10.10.1
standby 80 preempt

interface Vlan88
standby version 2
standby 88 ip 10.10.14.1
standby 88 preempt

interface Vlan89
standby version 2
standby 89 ip 10.10.15.1
standby 89 preempt

interface Vlan90
standby version 2
standby 90 ip 10.10.3.1
standby 90 priority 120
standby 90 preempt

interface Vlan91
standby version 2
standby 91 ip 10.10.3.65
standby 91 priority 120
standby 91 preempt

interface Vlan92
standby version 2
standby 92 ip 10.10.3.129
standby 92 priority 120
standby 92 preempt

interface Vlan93
standby version 2
standby 93 ip 10.10.3.193
standby 93 priority 120
standby 93 preempt

interface Vlan100
standby version 2
standby 100 ip 10.10.1.33
standby 100 preempt

interface Vlan111
standby version 2
standby 111 ip 10.10.4.1
standby 111 priority 120
standby 111 preempt

interface Vlan125
standby version 2
standby 125 ip 10.10.10.33
standby 125 priority 120
standby 125 preempt

interface Vlan130
standby version 2
standby 130 ip 10.10.8.1
standby 130 priority 120
standby 130 preempt

interface Vlan200
standby version 2
standby 200 ip 10.10.10.129
standby 200 preempt
```

Настройки на CORE2-SW:

```
interface Vlan55
standby version 2
standby 55 ip 10.10.1.1
standby 55 preempt

interface Vlan56
standby version 2
standby 56 ip 10.10.2.1
standby 56 preempt

interface Vlan57
standby version 2
standby 57 ip 10.10.1.129
standby 57 priority 120

interface Vlan60
standby version 2
standby 60 ip 10.10.1.65
standby 60 preempt

interface Vlan77
standby version 2
standby 77 ip 10.10.12.1
standby 77 priority 120
standby 77 preempt

interface Vlan80
standby version 2
standby 80 ip 10.10.10.1
standby 80 priority 120
standby 80 preempt

interface Vlan88
standby version 2
standby 88 ip 10.10.14.1
standby 88 priority 120
standby 88 preempt

interface Vlan89
standby version 2
standby 89 ip 10.10.15.1
standby 89 priority 120
standby 89 preempt

interface Vlan90
standby version 2
standby 90 ip 10.10.3.1
standby 90 preempt

interface Vlan91
standby version 2
standby 91 ip 10.10.3.65
standby 91 preempt

interface Vlan92
standby version 2
standby 92 ip 10.10.3.129
standby 92 preempt

interface Vlan93
standby version 2
standby 93 ip 10.10.3.193
standby 93 preempt

interface Vlan100
standby version 2
standby 100 ip 10.10.1.33
standby 100 priority 120
standby 100 preempt

interface Vlan111
standby version 2
standby 111 ip 10.10.4.1
standby 111 preempt

interface Vlan125
standby version 2
standby 125 ip 10.10.10.33
standby 125 preempt

interface Vlan130
standby version 2
standby 130 ip 10.10.8.1
standby 130 preempt

interface Vlan200
standby version 2
standby 200 ip 10.10.10.129
standby 200 priority 120
standby 200 preempt
```

Далее: [Настройка STP](./stp_settings.md)

Назад: [Настройка головного офиса](./main_office.md)