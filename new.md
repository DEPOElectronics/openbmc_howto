Создание образа под свою плату:
Я использовал в качестве основы слой из ветки minimum

1.	Скопировать  meta-sample/ast2500-min
2.	сonf
	1.	`bblayers.conf.sample` переименовать слой
    2.	`BBFILE_COLLECTIONS += "mylayer"`
    3.	`BBFILE_PATTERN_dazn-layer = "^${LAYERDIR}/"`
    4.	`LAYERSERIES_COMPAT_dazn-layer = "honister"`
3. local.conf.sample переименовать MACHINE    
