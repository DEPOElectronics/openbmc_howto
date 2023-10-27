Для общения BIOS-BMC чаще всего используется KCS over LPC. Соответственно надо включить в ядре
```
&lpc_ctrl {
	//Enable lpc clock
	status = "okay";
};

&kcs3 {
	status = "okay";
	aspeed,lpc-io-reg = <0xCA2>;
};
```