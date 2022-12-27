# Информация о блоке питания от EM
Надо добавить конфигурационный файл. Для каждого типа БП отдельный файл. Пример файла можно взять в примерах конфигурации. В веб интерфейсе начинает отображаться только после добавления шасси или? [платы](em_board)
Простой конфиг:
```
[{
	"Exposes": [
		{
		"Address": "$ADDRESS % 4 + 88",
		"Bus": "$bus",
		"Name": "PSU$ADDRESS % 4 + 1",
		"Thresholds": [],
		"Type": "pmbus"
	}
	],
	"Name": "PSU$ADDRESS % 4 + 1",
	"Probe": "xyz.openbmc_project.FruDevice({'PRODUCT_PRODUCT_NAME': '1136-1300WNA '})",
	"Type": "PowerSupply",
	"xyz.openbmc_project.Inventory.Decorator.Asset": {
		"Manufacturer": "$PRODUCT_MANUFACTURER",
		"Model": "$PRODUCT_PRODUCT_NAME",
		"PartNumber": "$PRODUCT_PART_NUMBER",
		"SerialNumber": "$PRODUCT_SERIAL_NUMBER"
	}
}]
```
Обязательно должен быть тип "pmbus"!