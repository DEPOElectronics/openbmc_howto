# Чтение из памяти
По умолчанию устройство /dev/mem отсутствует, для того чтобы к нему обратиться надо в аргументы запуска добавить `mem.devmem=y`
Например
```
fw_setenv bootargs $(fw_printenv -n bootargs) mem.devmem=y && systemctl reboot
```
Для чтения используется программа devmem
```
# devmem  0x1e6e207c
0x04030303
```
