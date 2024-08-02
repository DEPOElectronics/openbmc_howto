# Установка MAC адреса
Для того чтобы MAC адрес задавался автоматически при старте системы необходимо его прописать в окружение UBOOT переменные `ethaddr` для MAC eth0 и `eth1addr` для MAC eth1
Это можно сделать командой
```
fw_setenv ethaddr "AA:AA:AA:AA:AA:AA"
```
Посмотреть текущее окружение можно с с помощью команды
```
fw_printenv
```

# Установка адреса из DevTree
Если mac адрес записан в eeprom, то
```
	/* FRU eeprom */
	eeprom@50 {
		compatible = "microchip,24c32", "atmel,24c32";
		reg = <0x50>;
		pagesize = <32>;
		wp-gpios = <&gpio0 ASPEED_GPIO(Y, 2) GPIO_ACTIVE_HIGH>;

		eth1_macaddress: macaddress@0e0a {
			reg = <0x0e0a 6>;
		};
	};

&mac1 {
	status = "okay";
	no-hw-checksum;
	phy-mode = "rgmii-rxid";
	phy-handle = <&ethphy1>;
	pinctrl-names = "default";
	pinctrl-0 = <&pinctrl_rgmii2_default>;
	resets = <&syscon 0x0c>;

	nvmem-cells = <&eth1_macaddress>;
	nvmem-cell-names = "mac-address";
};
```