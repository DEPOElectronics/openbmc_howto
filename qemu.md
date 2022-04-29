Скачать новый эмулятор, запустить
```
/home/glukhov/projects/qemu-system-arm -m 512 -M ast2500-evb,fmc-model=n25q512a -nographic -drive file=$1,format=raw,if=mtd -net nic -net user,hostfwd=:127.0.0.1:2222-:22,hostfwd=:127.0.0.1:2443-:443,hostfwd=udp:127.0.0.1:2623-:623,hostname=qemu
```