Для генерации использую [frugen](https://github.com/ipmitool/frugen)
Обычно создаю вспомогательный скрипт который вызывает frugen c предустановленными параметрами. Часто в эту же FRU зашивается MAC адрес.
Пример
```
#!/bin/bash
# ./genfru_p62-v $serialNumber $MAC1 $MAC2
SN=$1
MAC0=$2
MAC1=$3

FRUNAME=fru_$SN\_$MAC0\_$MAC1.bin
echo "file= "$FRUNAME
echo SN=$SN
echo MAC0=${MAC0:0:2}:${MAC0:2:2}:${MAC0:4:2}:${MAC0:6:2}:${MAC0:8:2}:${MAC0:10:2}
echo MAC1=${MAC1:0:2}:${MAC1:2:2}:${MAC1:4:2}:${MAC1:6:2}:${MAC1:8:2}:${MAC1:10:2}

./frugen \
               -I \
               --board-mfg "Ltd." \
               --board-pn "DN.43525.03" \
	       --board-pname "P62-V" \
	       --chassis-type 0x17 \
	       --prod-atag "Server" \
	       --prod-mfg "Electronics, Ltd." \
	       --prod-name "Sm 370R" \
	       --prod-serial "$SN" \
	       --prod-version "01" \
	       $FRUNAME
	       
dd if=/dev/zero ibs=1k count=16 | tr "\000" "\377" >> $FRUNAME
truncate -s 16384 $FRUNAME

echo "3f80: $MAC0" | xxd -r - $FRUNAME
echo "3f88: $MAC1" | xxd -r - $FRUNAME
echo "fru file: "$FRUNAME
```