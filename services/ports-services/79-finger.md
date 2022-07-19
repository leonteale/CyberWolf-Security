---
description: Finger is a program you can use to find information about computer users.
---

# 79 - Finger

### **Usage**

**Finger Enumeration**&#x20;

```
finger @TARGET-IP
```

**Finger a Specific Username**&#x20;

```
finger admin@192.186.218.3 
Login: admin                            Name: Jason L. Nawrocki 
Directory: /home/admin                  Shell: /bin/bash 
Office: 5877, 989-905-2731              Home Phone: 978-272-5420 
Never logged in. 
No mail. 
No Plan. 
```

### **Solaris**

**Solaris bug that shows all logged in users:**&#x20;

```
finger 0@host   
SunOS: RPC services allow user enum: 

$ rusers # users logged onto LAN 
finger 'a b c d e f g h'@sunhost  
```

### Metasploit

Modules:

```
msf5 > search finger 

Matching Modules 
================ 

#  Name                                           Disclosure Date  Rank    Check  Description 
   -  ----                                           ---------------  ----    -----  ----------- 
   1  auxiliary/gather/mybb_db_fingerprint           2014-02-13       normal  Yes    MyBB Database Fingerprint 
   2  auxiliary/scanner/finger/finger_users                           normal  Yes    Finger Service User Enumerator 
   3  auxiliary/scanner/oracle/isqlplus_login                         normal  Yes    Oracle iSQL*Plus Login Utility 
   4  auxiliary/scanner/oracle/isqlplus_sidbrute                      normal  Yes    Oracle iSQLPlus SID Check 
   5  auxiliary/scanner/vmware/esx_fingerprint                        normal  Yes    VMWare ESX/ESXi Fingerprint Scanner 
   6  auxiliary/server/browser_autopwn                                normal  No     HTTP Client Automatic Exploiter 
   7  exploit/bsd/finger/morris_fingerd_bof          1988-11-02       normal  Yes    Morris Worm fingerd Stack Buffer Overflow 
   8  exploit/windows/http/bea_weblogic_post_bof     2008-07-17       great   Yes    Oracle Weblogic Apache Connector POST Request Buffer Overflow 
   9  post/windows/gather/enum_putty_saved_sessions                   normal  No     PuTTY Saved Sessions Enumeration Module 
```

Find users:

```
msf5 > use auxiliary/scanner/finger/finger_users 
msf5 auxiliary(scanner/finger/finger_users) > show options  

Module options (auxiliary/scanner/finger/finger_users): 

Name        Current Setting                                                Required  Description 
   ----        ---------------                                                --------  ----------- 
   RHOSTS                                                                     yes       The target address range or CIDR identifier 
   RPORT       79                                                             yes       The target port (TCP) 
   THREADS     1                                                              yes       The number of concurrent threads 
   USERS_FILE  /usr/share/metasploit-framework/data/wordlists/unix_users.txt  yes       The file that contains a list of default UNIX accounts. 

msf5 auxiliary(scanner/finger/finger_users) > setg rhosts 192.186.218.3 
rhosts => 192.186.218.3 
msf5 auxiliary(scanner/finger/finger_users) > run 

[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: admin 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: administrator 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: backup 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: bin 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: daemon 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: dbadmin 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: diag 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: games 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: gnats 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: gopher 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: irc 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: list 
[+] 192.186.218.3:79      - 192.186.218.3:79 - Found user: lp 
[+] 192.186.218.3:79      - 192.186.218.3:79 Users found: admin, administrator, backup, bin, daemon, dbadmin, diag, games, gnats, gopher, irc, list, lp, mail, man, news, nobody, proxy, root, saned, sync, sys, systemd-bus-proxy, udadmin, uucp, webmaster, www-data 
[*] 192.186.218.3:79      - Scanned 1 of 1 hosts (100% complete) 
[*] Auxiliary module execution completed 
```

### finger-user-enum

Download script and run it with a wordlist: &#x20;

[http://pentestmonkey.net/tools/finger-user-enum/finger-user-enum-1.0.tar.gz](http://pentestmonkey.net/tools/finger-user-enum/finger-user-enum-1.0.tar.gz)&#x20;

**Overview**&#x20;

finger-user-enum is a tool for enumerating OS-level user accounts via the finger service. As of release v1.0 it is known to work against the default Solaris daemon. It may not yet work against all daemons since there is no defined format for the data returned by the finger service.&#x20;

**Installation**&#x20;

finger-user-enum is just a stand alone PERL script, so installation is as simple as copying it to your path (e.g. /usr/local/bin). It has only been tested under Linux so far.&#x20;

It depends on the following PERL modules which you may need to install first:&#x20;

* Socket&#x20;
* IO::Handle&#x20;
* IO::Select&#x20;
* IO::Socket::INET&#x20;
* Getopt::Std&#x20;

If you have PERL installed, you should be able to install the modules from CPAN:&#x20;

```
# perl -MCPAN -e shell 
cpan> install Getopt::Std 
```

&#x20;**Usage**&#x20;

finger-user-enum simply needs to be passed a list of users and at least one target running an finger service.&#x20;

&#x20;finger-user-enum v1.0 ( [http://pentestmonkey.net/tools/finger-user-enum](http://pentestmonkey.net/tools/finger-user-enum) ) &#x20;

Usage: finger-user-enum.pl \[options] (-u username|-U users.txt) (-t host|-T ips.txt) &#x20;

options are: \
&#x20;        \-m n     Maximum number of resolver processes (default: 5) \
&#x20;        \-u user  Check if user exists on remote system \
&#x20;        \-U file  File of usernames to check via finger service \
&#x20;        \-t host  Server host running finger service \
&#x20;        \-T file  File of hostnames running the finger service \
&#x20;        \-r host  Relay.  Intermediate server which allows relaying of finger requests. \
&#x20;        \-p port  TCP port on which finger service runs (default: 79) \
&#x20;        \-d       Debugging output \
&#x20;        \-s n     Wait a maximum of n seconds for reply (default: 5) \
&#x20;        \-v       Verbose \
&#x20;        \-h       This help message&#x20;

Some Examples&#x20;

For the examples below we need a list of potential usernames. The following output demostrates the format for this list:&#x20;

```
$ head users.txt 
root 
bin 
daemon 
adm 
lp 
sync 
shutdown 
halt 
mail 
news
```

**Normal Usage**&#x20;

The output below shows how the finger daemon responds differently to valid and invalid usernames:&#x20;

```
 $ telnet 10.0.0.1 79 
Trying 10.0.0.1... 
Connected to 10.0.0.1. 
Escape character is '^]'. 
root 
Login       Name               TTY         Idle    When    Where 
root     Super-User            console     2:05 Wed 07:23 
Connection closed by foreign host.  
$ telnet 10.0.0.1 79 
Trying 10.0.0.1... 
Connected to 10.0.0.1. 
Escape character is '^]'. 
blah 
Login       Name               TTY         Idle    When    Where 
blah                  ??? 
Connection closed by foreign host. 
```

finger-user-enum attempts to automatically parse the results returned by the finger daemon and report only users which exist.&#x20;

Note: If you ever need to modify the pattern-matching within finger-user-enum (e.g. to support a different finger daemon), you’ll need to base the patterns on positive and negative result like those found above.&#x20;

Here’s an example of the most common usage of the tool:&#x20;

```
 $ ./finger-user-enum.pl -U users.txt -t 10.0.0.1 
Starting finger-user-enum v1.0 ( 
http://pentestmonkey.net/tools/finger-user-enum
 )  
---------------------------------------------------------- 
|                   Scan Information                       | 
  ----------------------------------------------------------  
Worker Processes ......... 5 
Usernames file ........... users.txt 
Target count ............. 1 
Username count ........... 47 
Target TCP port .......... 79 
Query timeout ............ 5 secs 
Relay Server ............. Not used  
######## Scan started at Sun Jan 21 19:44:22 2007 ######### 
root@10.0.0.1: root     Super-User            console     2:03 Wed 07:23 .. 
bin@10.0.0.1: bin             ???            pts/1        <Dec 21 13:04> 10.0.0.99 
daemon@10.0.0.1: daemon          ???                         < .  .  .  . >.. 
adm@10.0.0.1: adm      Admin                              < .  .  .  . >.. 
lp@10.0.0.1: lp       Line Printer Admin                 < .  .  .  . >.. 
uucp@10.0.0.1: uucp Admin                         < .  .  .  . >.. 
nobody@10.0.0.1: nobody4  SunOS 4.x Nobody                   < .  .  .  . >.. 
ftp@10.0.0.1: ftp      Anonymous FTPUser     674          <Aug 11 14:22> 10.0.0.99 
######## Scan completed at Sun Jan 21 19:44:23 2007 ######### 
8 results.  
47 queries in 1 seconds (47.0 queries / sec)
```
