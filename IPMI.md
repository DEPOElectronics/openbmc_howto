# IPMI
Подключение по IPMI
```
ipmitool -C 17 -H "$BMC_IP" -I lanplus -U "$BMC_USER" -P "$BMC_PASSWD" power
```
То что будет выводится по запросу ipmitool sensor описывается в yaml.config. Либо можно положиться на автоматику и не настраивать ipmi

## Получить список пользователей
```
ipmitool user list 1
```

## Задать пароль пользователя
```
ipmitool user set password <userid> christester
```