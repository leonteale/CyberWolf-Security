# Password Cracking

## Cracking MSCACH / LSA

### Using hashcat:

```
 .\hashcat.exe -m 2100 .\tocrack\hashes.txt .\rockyou.txt
```

example hash format

```
$DCC2$10240#user12345#ea373dca60f838378e952b9cd337c0d3
$DCC2$10240#testuser#0b8378e952b978df5378e952b955eb3d2
```
