---
description: >-
  VNC is a graphical desktop-sharing system that uses the Remote Frame Buffer
  protocol (RFB) to remotely control another computer. VNC usually uses ports
  5800 or 5801 or 5900 or 5901.
---

# VNC

## Enumeration

```bash
nmap -sV --script vnc-info,realvnc-auth-bypass,vnc-title -p <PORT> <IP>
msf> use auxiliary/scanner/vnc/vnc_none_auth
```

## Connect to VNC using Kali

```bash
vncviewer [-passwd passwd.txt] <IP>::5901
```

## Decrypting VNC password

Default **password is stored** in: \~/.vnc/passwd

If you have the VNC password and it looks encrypted (a few bytes, like if it could be and encrypted password). It is probably ciphered with 3des. You can get the clear text password using [https://github.com/jeroennijhof/vncpwd](https://github.com/jeroennijhof/vncpwd)

```bash
make
vncpwd <vnc password file>
```

You can do this because the password used inside 3des to encrypt the plain-text VNC passwords was reversed years ago.\
For **Windows** you can also use this tool: [https://www.raymond.cc/blog/download/did/232/](https://www.raymond.cc/blog/download/did/232/)\
I save the tool here also for ease of access:

{% file src="../.gitbook/assets/vncpwd.zip" %}

## Shodan

* `port:5900 RFB`
