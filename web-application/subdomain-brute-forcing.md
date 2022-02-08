# Subdomain brute forcing

### GoBuster

```
sudo gobuster vhost -u http://horizontall.htb -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt  1 ⨯


===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:          http://horizontall.htb
[+] Method:       GET
[+] Threads:      10
[+] Wordlist:     /home/kali/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:   gobuster/3.1.0
[+] Timeout:      10s
===============================================================
2022/01/14 08:40:24 Starting gobuster in VHOST enumeration mode
===============================================================
~
~
~
```

### WFUZZ

```
wfuzz -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://shibboleth.htb/ -v -x -H "Host:FUZZ.shibboleth.htb" --hw 26
```

