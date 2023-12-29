# АЦП ADC датчики
АЦП включаются в [дереве устройств](dev_tree) в разделе adc. В простейшем случаее
```
&adc {
	status = "okay";
};
```
Для того чтобы работать с ними с помощью [Entity-Manager](entity-manager) требуется драйвер iio-hwmon
```
	iio-hwmon {
		compatible = "iio-hwmon";
		io-channels = <&adc 0>, <&adc 1>, <&adc 2>, <&adc 3>, <&adc 4>,
			<&adc 5>, <&adc 6>, <&adc 7>, <&adc 8>, <&adc 9>,
			<&adc 10>, <&adc 11>, <&adc 12>, <&adc 13>, <&adc 14>;
	};

```
После этого можно перечислять в конфигурации
```
        {
            "Index": 10,
            "Name": "VOLT_PCH_1.8V",
            "ScaleFactor": 0.7519,
            "Type": "ADC"
        },
```
ScaleFactor обычно можно вычислить воспользовавшись [калькулятором делителя напряжения](https://cxem.net/calc/divider_calc.php?ysclid=lqqajrhtax407656489) (в данном случае 1/value)
Для измерения напряжения батареи используется дополнительный gpio,  для того чтобы батарея быстро не садилась от частого измерения. Для этого линия [gpio](gpio) должна быть названа. В конфигураторе Entity-manager для данного АЦП добавляется
```
            "BridgeGpio": [
                {
                    "Name": "BAT_HWM_EN",
                    "Polarity": "High"
                }
            ],
```