# Metasploit

## Start Required Services <a href="#start-required-services" id="start-required-services"></a>

* Metasploit uses PostgreSQL as its database so it needs to be launched first.

```bash
$ sudo service postgresql start
```

### Initialise the Metasploit PostgreSQL Database <a href="#initialise-the-metasploit-postgresql-database" id="initialise-the-metasploit-postgresql-database"></a>

* With PostgreSQL up and running, we next need to create and initialize the msf database.

```bash
$ sudo msfdb init
```

### Launch msfconsole in Kali <a href="#launch-msfconsole-in-kali" id="launch-msfconsole-in-kali"></a>

```bash
$ sudo msfconsole
msf > db_status
[*] postgresql connected to msf3
```

### Connect MSF to the database if needed

```
db_connect -y /usr/share/metasploit-framework/config/database.yml
```

### Fix Metasploit Cache Issue <a href="#fix-metasploit-cache-issue" id="fix-metasploit-cache-issue"></a>

```bash
msf > search wordpress
[!] Database not connected or cache not built, using slow search

# Rebuid Cache
# It takes some time for the cache to be rebuild
msf> db_rebuild_cache
```

### Set RHOSTS from msfconsole search

```
services -S 445 -R
```

### Set RHOSTS globally

### Export Database

```
 db_export -f xml /root/msfu/Exported.xml
```
