# Работа с SPD eeprom
SPD модули DIMM DDR4 и более ранние видны по i2c. До DIMM4 c ними можно работать как с обычной eeprom at24 на 256кбит. Для DIMM4 используется 512кбит eeprom c 2 страницами по 256кбит. Переключение страниц осуществляется с помощью записи команды 0 на адрес i2c 0x36 (1 страница) или  0x37 (2 страница) вне зависимости от адреса DIMM! Для чтения такой SPD можно использовать драйвер ee1004. Он не поддерживает запись. В случае необходимости записать spd (если запись еще не заблокирована) надо воспользоваться драйвером at2402 и переключать страницы вручную.

## SPD для DDR5
В отличии от предыдущих DDR доступ к SPD осуществляется по шине I3C. Получить доступ так и не удалось.
## Подготовка файла SPD
Обычно файлы spd передают в текстовой-шестнадцатеричной таблице виде и расширением .spd
Например
```
 23 12 0C 01 86 29 00 08 00 60 00 03 01 0B 80 00 
 00 00 05 0D F8 FF 2F 00 6E 6E 6E 11 00 6E F0 0A 
 20 08 00 05 00 A8 14 28 28 00 78 00 14 3C 00 00 
```
Перевести в бинарный вид можно с помощью команды 
`xxd -r -p spd_full.spd > spd_full.bin`
Разбить бинарный файл на 2 страницы можно с помощью dd
```
dd if=./spd_full.bin of=./spd_1_of_2.spd bs=256 count=1
dd if=./spd_full.bin of=./spd_2_of_2.spd bs=256 count=1 skip=1
```
## Запись SPD
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
## Просмотр информации в spd
Для получения инфы из spd файла  можно использовать decode-dimms
```
hexdump -C spd_full.bin > spd_full.hex
decode-dimms -x spd_full.hex
```
# Manufacturer ID
Идектификатор производителя записан в стандарте JEDEC в документе JEP106. Для DEPO - `0x89 0x19`