# Directory Brute forcing

### GoBuster

```
gobuster dir -u http://horizontall.htb/ -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt    1 ⨯


===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://horizontall.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /home/kali/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/01/14 08:34:17 Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 194] [--> http://horizontall.htb/img/]
/css                  (Status: 301) [Size: 194] [--> http://horizontall.htb/css/]
/js                   (Status: 301) [Size: 194] [--> http://horizontall.htb/js/] 
~
~
~
```

More intense scan

```
gobuster dir -u http://backdoor.htb/ -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 100 -e -x php,txt,html,bak,zip
```

