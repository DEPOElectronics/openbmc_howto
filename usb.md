# Подключение USB устройств к BMC
Для того чтобы в BMC определялись физические USB устройства необходимо в конфигурации [ядра](kernel) добавить необходимые записи. ~~Какие?~~
# Подключение к хосту как USB устройство вручную
```
#!/bin/sh

cd /sys/kernel/config/usb_gadget
mkdir hid_keyboard
cd hid_keyboard;

echo 0x0100 > bcdDevice
echo 0x0200 > bcdUSB
echo 0x0104 > idProduct         # Multifunction Composite Gadget
echo 0x1d6b > idVendor          # Linux Foundation

# create English locale
mkdir strings/0x409

echo "OpenBMC" > strings/0x409/manufacturer
echo "virtual_input" > strings/0x409/product
echo "OBMC0001" > strings/0x409/serialnumber

# Create HID keyboard function
mkdir functions/hid.0

echo 1 > functions/hid.0/protocol       # 1: keyboard
echo 8 > functions/hid.0/report_length
echo 1 > functions/hid.0/subclass

echo -ne '\x05\x01\x09\x06\xa1\x01\x05\x07\x19\xe0\x29\xe7\x15\x00\x25\x01\x75\x01\x95\x08\x81\x02\x95\x01\x75\x08\x81\x03\x95\x05\x75\x01\x05\x08\x19\x01\x29\x05\x91\x02\x95\x01\x75\x03\x91\x03\x95\x06\x75\x08\x15\x00\x25\x65\x05\x07\x19\x00\x29\x65\x81\x00\xc0' > functions/hid.0/report_desc


# Create configuration
mkdir configs/c.1
mkdir configs/c.1/strings/0x409

echo 0x80 > configs/c.1/bmAttributes
echo 200 > configs/c.1/MaxPower
echo "" > configs/c.1/strings/0x409/configuration

# Link HID functions to configuration
ln -s functions/hid.0 configs/c.1
echo "1e6a0000.usb-vhub:p4" > UDC
```
Скрипт взят с [https://github.com/AspeedTech-BMC/openbmc/releases/download/v08.01/SDK_User_Guide_v08.01.pdf](https://github.com/AspeedTech-BMC/openbmc/releases/download/v08.01/SDK_User_Guide_v08.01.pdf "https://github.com/AspeedTech-BMC/openbmc/releases/download/v08.01/SDK_User_Guide_v08.01.pdf") . ch6.2.35 Драйвер контроллера виртуального концентратора USB2.0

Я считаю, что он скопирован с [https://github.com/openbmc/obmc-ikvm/blob/master/create_usbhid.sh#L6.](https://github.com/openbmc/obmc-ikvm/blob/master/create_usbhid.sh#L6 "https://github.com/openbmc/obmc-ikvm/blob/master/create_usbhid.sh#L6")
Если вы можете видеть `a`на хосте, выполнив следующую команду на bmc, тогда ваше USB-соединение работает. Я понятия не имею, почему клавиатура не работает на obmc-ikvm.

```
echo -ne "\0\0\x4\0\0\0\0\0" > /dev/hidg2
echo -ne "\0\0\0\0\0\0\0\0" > /dev/hidg2
```