# IPMI
Подключение по IPMI
```
ipmitool -C 17 -H "$BMC_IP" -I lanplus -U "$BMC_USER" -P "$BMC_PASSWD" power
```