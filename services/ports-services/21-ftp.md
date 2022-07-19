---
description: >-
  The File Transfer Protocol is a standard network protocol used for the
  transfer of computer files between a client and server on a computer network.
---

# 21 - FTP

\*Consider using FileZilla&#x20;

### Anonymous FTP&#x20;

username: anonymous&#x20;

password: blank

```
root@Kali:~/# ftp 10.10.10.98 
Connected to 10.10.10.98. 
220 Microsoft FTP Service 
Name (10.10.10.98:root): anonymous 
331 Anonymous access allowed, send identity (e-mail name) as password. 
Password: 
230 User logged in. 
Remote system type is Windows_NT. 
ftp> dir 
200 PORT command successful. 
125 Data connection already open; Transfer starting. 
08-23-18 08:16PM <DIR> Backups 
08-24-18 09:00PM <DIR> Engineer 
226 Transfer complete. 
ftp> exit 
221 Goodbye.
```



### Nmap&#x20;

Port scan using nmap&#x20;

```
nmap -p 21 --script ftp-* 10.10.10.98 
PORT STATE SERVICE VERSION 
21/tcp open ftp Microsoft ftpd 
| ftp-anon: Anonymous FTP login allowed (FTP code 230) 
|_Can't get directory listing: TIMEOUT 
| ftp-syst:  
|_ SYST: Windows_N 
```



### Download everything from FTP server&#x20;

```
root@Kali:~/# wget -m --no-passive 
ftp://anonymous:anonymous@10.10.10.98 

--2019-03-04 17:00:50-- 
ftp://anonymous:*password*@10.10.10.98/ 

=> ‘10.10.10.98/.listing’ 
Connecting to 10.10.10.98:21... connected. 
Logging in as anonymous ... Logged in! 
==> SYST ... done. ==> PWD ... done. 
==> TYPE I ... done. ==> CWD not needed. 
==> PORT ... done. ==> LIST ... done. 
10.10.10.98/.listing [ <=> ] 97 --.-KB/s in 0s 
==> PORT ... done. ==> LIST ... done. 
10.10.10.98/.listing [ <=> ] 97 --.-KB/s in 0s  
2019-03-04 17:00:50 (9.16 MB/s) - ‘10.10.10.98/.listing’ saved [194] 
--2019-03-04 17:00:50-- 
ftp://anonymous:*password*@10.10.10.98/Backups/ 

=> ‘10.10.10.98/Backups/.listing’ 
==> CWD (1) /Backups ... done. 
==> PORT ... done. ==> LIST ... done. 
 
```

### Connect on a non-standard port&#x20;

```
root@kali:~# ftp 
ftp> open 192.148.210.3 134 
Connected to 192.148.210.3. 
220 (vsFTPd 3.0.3) 
Name (192.148.210.3:root): anonymous 
331 Please specify the password. 
Password: 
230 Login successful. 
Remote system type is UNIX. 
Using binary mode to transfer files. 
ftp> dir 
200 PORT command successful. Consider using PASV. 
150 Here comes the directory listing. 
-rw-r--r--    1 ftp      ftp            33 Jun 26 13:57 flag 
226 Directory send OK. 
```

### Metasploit module:&#x20;

```
auxiliary/scanner/ftp/ftp_login
```

```
use auxiliary/scanner/ftp/anonymous
```

```
auxiliary/scanner/ftp/ftp_version
```

## Create FTP server

On a linux host start a FTP:

```
apt-get install python3-pyftpdlib  
python3 -m pyftpdlib -p 21 -w
```

Or use metasploit:

```
msf > use auxiliary/server/ftp
```
