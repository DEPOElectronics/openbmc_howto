На данный момент хорошего способа работы с шиной i3c не нашел. Список шин можно увидеть с помощью `i2cdetect -l`. Но устройства на шине не увидел
[Пример](https://github.com/AspeedTech-BMC/linux/blob/24cbe56dfcf97618e7bee12218f4f33374b6bbf6/arch/arm/boot/dts/aspeed/aspeed-ast2600-evb.dts#L904) описания DevTree. После добавления DevTree, увидел какие-то устройства (0-0, 2-0)  с помощью драйвера i3cdev.
С помощью программы i3ctransfer смог считать какие-то данные, но не понял какие.
[i3c-tools](https://github.com/AspeedTech-BMC/i3c-tools) 
Считать 15 байт с 0 смещения
```
i3ctransfer -d /dev/bus/i3c/0-0 -w 0 -r 15
```