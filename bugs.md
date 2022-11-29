# Частые проблемы 
## Не удалось получить IP
```
rm /etc/systemd/network/00-bmc-eth0.network
```

## Заново прописать MAC
```
rm /var/lib/first-boot-set-mac/eth0
systemctl restart update_mac
```