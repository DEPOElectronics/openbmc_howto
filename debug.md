# Отладка
[SDK проекта](sdk.md)
[Сборка](build.md) программы для OpenBmc
Для очистки программы при сборке в Bitbake `bitabke {program} -c cleanall`

Просмотр параметров ядра:
```
zcat /proc/config.gz | grep 
```

##  Измененные файлы
`/run/initramfs/rw/cow`

## Создание патча
см. [Создать патч](bitbake.md)

# Просмотр исходников
Зачастую исходники удобно смотреть через https://grok.openbmc.org

# Пример
```
#include <iostream>
std::cout<<"Hello World" <<std::endl;
```
# Debug для определенной программы
Для некоторых программ (например U-boot) для включения отладки для определенного файла нужно в этот файл до всех `#INCLUDE` написать `#define DEBUG 1
