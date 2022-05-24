# Отладка
[SDK проекта](sdk.md)
[Сборка](build.md) программы для OpenBmc
Для очистки программы при сборке в Bitbake `bitabke {program} -c cleanall`

Просмотр параметров ядра:
```
zcat /proc/config.gz | grep 
```