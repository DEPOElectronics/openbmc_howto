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

# phosphor-ipmi-host
Программа отделена от для команд которые надо регистрировать либо внутри программы, либо через специальный интерфейс. Сама программа ipmid, а команды хранятся в библиотеках /usr/lib/ipmi-providers
# Настройка
Должен быть заполнен dev_id.json он отвечает за ответ еа запрос Get Device ID Command (`ipmitool raw 0x06 0x01`)
## FRU
Для того чтобы появлися fru с ID=0 нужно чтобы chassis type у него было либо main server либо rack
