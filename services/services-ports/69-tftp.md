---
description: >-
  TFTP does not provide any mechanism for users to logon to the remote system
  with a userid and password and verify which files they can access.
---

# 69 - TFTP

## Enumeration

Confirm TFTP is open:

```
nmap -sU -p 69 <IP>
```

Enumerate files:

```
nmap -sU -p 69 --script tftp-enum.nse --script-args tftp-enum.filelist=customlist.txt <IP>
```

## Connect

Can use the built in utility:

```
tftp 10.10.10.90
    tftp> get non-existing-file
    Error code 1: Could not find file 'C:\non-existing-file'.
    
    tftp> get WINDOWS\System32\drivers\etc\hosts
    Received 734 bytes in 0.1 seconds [58720 bits/sec]
```

Put files:

```
tftp 10.10.10.90
    tftp> put test.txt
    Sent 9 bytes in 0.3 seconds
```

## Brute force files

### Nmap

```
root@kali:~# nmap -Pn -sU -p69 --script tftp-enum <IP>
Starting Nmap 6.46 (http://nmap.org) at 2014-11-14 13:01 UTC
Nmap scan report for <IP>
PORT STATE SERVICE
69/udp open tftp
| tftp-enum:
| tftp-enum:
| sip.cfg
| syncinfo.xml
| SEPDefault.cnf
| SIPDefault.cnf
|_ XMLDefault.cnf.xml
```

### Metasploit

```
msf > use auxiliary/scanner/tftp/tftpbrute
```
