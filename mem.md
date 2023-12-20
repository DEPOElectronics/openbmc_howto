# Работа с памятью
По умолчанию устройство /dev/mem отсутствует, для того чтобы к нему обратиться надо в аргументы запуска добавить `mem.devmem=y`
Например
```
fw_setenv bootargs $(fw_printenv -n bootargs) mem.devmem=y && systemctl reboot
```
https://shenki.github.io/openbmc-dev-mem/
## Чтение
Для чтения используется программа devmem
```
# devmem 0x1E6E2070
0xF131D24E
```
При работе из u-boot
```
md.w 0x1E6E2070
```
## Запись 
```
devmem 0x1E6E207C 32 0x100000
```
При работе из u-boot
```

```
# Работа с SPD eeprom
SPD модули DIMM видны по i2c. До DIMM4 c ними можно работать как с обычной eeprom at24 на 256кбит. Для DIMM4 используется 512кбит eeprom c 2 страницами по 256кбит. Переключение страниц осуществляется с помощью записи команды 0 на адрес i2c 0x36 (1 страница) или  0x37 (2 страница) вне зависимости от адреса DIMM! Для чтения такой SPD можно использовать драйвер ee1004. Он не поддерживает запись. В случае необходимости записать spd (если запись еще не заблокирована) надо воспользоваться драйвером at2402 и переключать страницы вручную.
```
echo 24c02 0x52 > /sys/bus/i2c/devices/i2c-6/new_device
i2cset -y 6 0x36 0
cat /tmp/dimm_1_of_2.spd > /sys/bus/i2c/devices/6-0052/eeprom
i2cset -y 6 0x37 0
cat /tmp/dimm_2_of_2.spd > /sys/bus/i2c/devices/6-0052/eeprom
echo 0x52 > /sys/bus/i2c/devices/i2c-6/delete_device

echo ee1004 0x52 > /sys/bus/i2c/devices/i2c-6/new_device
hexdump -C /sys/bus/i2c/devices/6-0052/eeprom
echo 0x52 > /sys/bus/i2c/devices/i2c-6/delete_device
```