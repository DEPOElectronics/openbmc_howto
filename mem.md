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