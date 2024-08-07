# Пост-коды
## Получение кодов
Для получения POST кодов из хоста надо в [device tree](dev_tree) прописать
```
&lpc_snoop {
status = "okay";
snoop-ports = <0x80>;
};
```
## Отображение кодов
Для того чтобы коды отображались на дисплее или светодиодах необходимо устройство в `/dev/` которое будет на вход принимать трехзначные шестнадцатеричные? коды и отображать их.
Есть проект для отображения через gpio - [post_disp](drivers/post_disp)
Для того чтобы на устройства записывались коды необходимо в файле `recipes-phosphor/host/phosphor-host-postd_%.bbappend` прописать следующее
```
PACKAGECONFIG:append = " 7seg"
POSTCODE_SEVENSEG_DEVICE = "post_disp"
```
где `POSTCODE_SEVENSEG_DEVICE` - название устройства