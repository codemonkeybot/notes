# NTP Ubunutu 18

### Set NTP 

Edit ```nano /etc/systemd/timesyncd.conf``` and replace 127.0.0.1 with appropriate ip address:

```
[Time]
NTP=127.0.0.1
FallbackNTP=127.0.0.1
```

Restart service
```
sudo systemctl restart systemd-timesyncd
```

Allow a seconds and the check NTP synchronized: yes
```
timedatectl
```
