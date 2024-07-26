## Настройка клиента

Данный пункт нужен лишь для тестирования Port Farwarding, поэтому настройка примитивная.

```
interface GigabitEthernet0/0
ip address 89.216.2.10 255.255.255.252
ip nat outside

interface GigabitEthernet0/1
ip address 192.168.1.1 255.255.255.0
ip nat inside

ip access-list standard WAN
permit 192.168.1.0 0.0.0.255

ip nat inside source list 1 interface GigabitEthernet0/0 overload
ip nat inside source list WAN interface GigabitEthernet0/0 overload

ip route 0.0.0.0 0.0.0.0 89.216.2.9 
```

На PC прописан статический адрес, настроен DNS 8.8.8.8 и указан маршрут по усолчанию 192.168.1.1.

Далее: [Тестирование сети](./testings.md)

Предыдущая страница: [Главная страница](../README.md)

