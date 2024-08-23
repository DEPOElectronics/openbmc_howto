Для доступа к регистрам phy через OpenBmc можно воспользоваться программой phytool
 
# Компиляция
```
bitbake phytool
```
 
# Использование
## Чтение регистров
Чтение производится командами `read` и `print`. `read` выдает только значение регистра, `print` - с декодированием значений
```
# phytool read eth0/0/0x1f
0x8100

# phytool print eth0/0/0x1f
ieee-phy: reg:0x1f val:0x8100

# phytool print eth0/0/1
ieee-phy: reg:BMSR(0x01) val:0x7849
   capabilities:   -100-b4 +100-f +100-h +10-f +10-h -100-t2-f -100-t2-h
   flags:          -ext-status -aneg-complete -remote-fault +aneg-capable -link -jabber +ext-register
```
## Запись регистров
```
phytool write eth0/0/0x1f 0x8900
```