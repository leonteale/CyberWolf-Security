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



## Disable hierarchical search

If you have found that Metasploit does this:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Then run the below to turn it off. dont forget to run the 'save' command after if you want it persistant on next boot.&#x20;

{% code overflow="wrap" %}
```bash
features set hierarchical_search_table false
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
