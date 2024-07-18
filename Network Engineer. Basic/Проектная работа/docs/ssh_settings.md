## Настройка SSH доступа на устройства

На данном этапе на каждом сетевом устройстве настраивается SSHv2, а также 1 админский пользователь с параметрами:

| Логин | Пароль |
| ---   | ---    |
| admin | cisco |

```
ip domain-name company.ru
username admin privilege 15 password cisco

crypto key generate rsa general-keys modulus 1024
ip ssh version 2

line vty 0 4
login local
```

Далее: [Настройка политик ACL](./acl_settings.md)

Назад: [Настройка головного офиса](./main_office.md)