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
