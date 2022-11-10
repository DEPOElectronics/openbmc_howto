# Переход со статичной ФС на UBIFS
## Для чего нужен переход
1) По-умолчанию работа с 2 микросхемами памяти - основной и резервной. И возможность загрузиться с резервной
2) Объем памяти на флешке ограничен размером флешки, а не размером раздела RO
## Включение UBI
В conf файл добавить зависимость
```
require conf/distro/include/phosphor-ubi.inc
```
## U-boot 2016
В conf файле если выбран u-boot 2019, то надо перейти назад на 2016
```
UBOOT_MACHINE = "ast_g5_phy_config"
PREFERRED_PROVIDER_virtual/bootloader = "u-boot-aspeed"
PREFERRED_PROVIDER_u-boot = "u-boot-aspeed"
PREFERRED_PROVIDER_u-boot-fw-utils = "u-boot-fw-utils-aspeed"
```
## DevTree
Поменять разметку флеш памяти в DevTree. Для этого из разметки убрать статичную разметку `include "openbmc-flash-layout-64.dtsi`, добавить раздел `obmc-ubi`. В итоге будут разделы `u-boot`, `u-boot-env`, `obmc-ubi`. Например:
```
               partitions {
                       #address-cells = < 1 >;
                       #size-cells = < 1 >;
                       compatible = "fixed-partitions";
                       u-boot@0 {
                               reg = <0x0 0xe0000>; // 896KB
                               label = "u-boot";
                       };
                       u-boot-env@e0000 {
                               reg = <0xe0000 0x20000>; // 128KB
                               label = "u-boot-env";
                       };
                       obmc-ubi@100000 {
                               reg = <0x100000 0x3F00000>; // 63MB
                               label = "obmc-ubi";
                       };
               };

```

## Тома UBI
`/proc/mtd` - соответствие тома и разделом mtd0 - bmc и т.д
`/dev/mdtX` - расположение разделов
`/dev/mtd/` - именованные ссылки на них
`dd` - копировать с флешки
`flashcp` - копировать на флешку
Копирование раздела напрямую не поддерживается поэтому нужен промежуточный файл
```
dd if=/dev/mtd/bmc of=/tmp/bmc
flashcp /tmp/bmc /dev/mtd/alt-bmc
```
## U-boot
Для того чтобы boot работал с разделами UBI нужна программа ubi в u-boot
`printenv` - отобразить переменные окружения
Для загрузки интересуют переменные
```
kernelname=kernel-7b40f281`
ubiblock=0,3
root=/dev/ubiblock0_3
```
Переменная `root` должна соответствовать переменной ubiblock
Для просмотра разделов
```
ubi part obmc-ubi
ubi info layout
```
## Задать значение переменных
Для того чтобы задать новое значение через U-BOOT
`setenv <переменная> <значение>`
сохранить `saveenv`

Для работы через систему
```
fw_printenv
fw_setenv
```
Необходимо чтобы u-boot и система хранили данные в одном месте. Настройка для системы в `/ets/fs_env.config`. Для u-boot в переменной `mtdparts`. Для корректной работы пришлось поменять!

# Overlay
Для того чтобы можно было записывать каталоги, которые по-умолчанию доступны только для чтения используется Overlay```
```
mkdir -p /tmp/persist/usr
mkdir -p /tmp/persist/work/usr
mount -t overlay -o lowerdir=/usr,upperdir=/tmp/persist/usr,workdir=/tmp/persist/work/usr overlay /usr
```

Для lib
```
mkdir -p /tmp/persist/lib
mkdir -p /tmp/persist/work/lib
mount -t overlay -o lowerdir=/lib,upperdir=/tmp/persist/lib,workdir=/tmp/persist/work/lib overlay /lib
```