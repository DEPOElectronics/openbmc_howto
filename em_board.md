# Создание платы в [EntityManager](entity-manager)
Создание платы необходимо для отображения датчиков и других объектов. В том числе информации о [блоках питания](em_psu)
Для добавления лпаты необходимо создать конфигурационный файл
```
MyBoard.json
{
    "Exposes": [],
    "Name": "My Baseboard",
    "Probe": "TRUE",
    "Type": "Board", 
    "xyz.openbmc_project.Inventory.Item.System": {},   
    "xyz.openbmc_project.Inventory.Decorator.Asset": {
        "Manufacturer": "ManufactuRER",
        "Model": "MODEL",
        "PartNumber": "PARTNUM",
        "SerialNumber": "SERIAL"
    }
}
```

Строка `"xyz.openbmc_project.Inventory.Item.System": {}` необходима для добавления данной информации в раздел "System" и  "Информация о сервере"