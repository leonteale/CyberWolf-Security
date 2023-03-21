# Port Scanning

### Rust Scan

{% embed url="https://github.com/RustScan/RustScan" %}

```
wget https://github.com/RustScan/RustScan/releases/download/2.0.1/rustscan_2.0.1_amd64.deb
dpkg -i rustscan_2.0.1_amd64.deb

rustscan -a shibboleth -u 3000
```

### Powershell portscan 1 liner

```
$ip = "192.168.140.4"; $Ports = [ordered]@{http=80; ftp=21; ssl=443; smb=445; telnet=23; ssh=22}; foreach ($port in $Ports.Values) { try { $Socket = New-Object System.Net.Sockets.TcpClient; $Socket.Connect($ip, $port); Write-Host "$port Open on $IP" -BackgroundColor Black -ForegroundColor Green; Start-Sleep 2 } catch { Write-Host "Couldn't connect to Port #$port On $IP " -BackgroundColor Black -ForegroundColor Red } } 
```
