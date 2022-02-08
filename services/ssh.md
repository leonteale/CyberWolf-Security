# SSH

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
