# Finding Domain Controllers and DHCP servers

```
nmap -sP <IP>/24 >> hosts.txt
nmap -p 389 <IP> 
cat /var/lib/dhcp/*
cat /etc/resolve.conf
Route -n
```

### Using Wireshark

If you find that your task is completely black boxed and a client won't give you a static IP to use then you can get on by stealing a valid users mac to then be in the mac pool and obtain a DHCP IP

Start Wireshark\
Search for a valid Mac (chose responsibly)\
Change your Mac to match\
Then disconnect and reconnect to the network, you should be given an IP
