# Workstation

## Prerequisites

* Local administrator and basic user account
* PowerShell we need to be enabled on windows builds
* Port 445 will need to be enabled for authentication scan

Ensure that you make the followihng registry edit in order to be able to perform the remote authenticated nussus scan:

> regedit (run as admin) > HKEY\_LOCAL\_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Policies > System. Right click > New > DWORD (32-bit) Value. LocalAccountTokenFilterPolicy > Right click > Modify > Value data: 1

Also enable/start the 'remote registry' service in services.msc

## Checklist

![](<../.gitbook/assets/image (4) (2).png>)

| Catagory | Checks | Result |
| -------- | ------ | ------ |
|          |        |        |
|          |        |        |
|          |        |        |

## Useful commands

### Unquoted service paths

```
cmd /c wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
```

### Search the running processes

```
Tasklist | findstr <query>
```

### Check if LAPS is installed (PowerShell)

```
Get-ChildItem 'C:\Program Files\LAPS\CSE\Admpwd.dll'
Get-ChildItem 'C:\Program Files (x86)\LAPS\CSE\Admpwd.dll'
```

## Vulnerability and patching checks

{% embed url="https://support.microsoft.com/en-gb/topic/a-new-version-of-the-windows-update-offline-scan-file-wsusscn2-cab-is-available-for-advanced-users-fe433f4d-44f4-28e3-88c5-5b22329c0a08" %}

### Using Microsoft Baseline Security Analyzer (MBA)

[https://docs.microsoft.com/en-us/previous-versions/cc184924(v=msdn.10)?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/previous-versions/cc184924\(v=msdn.10\)?redirectedfrom=MSDN)

{% embed url="https://files02.tchspt.com/temp/MBSASetup-x64-EN.msi" %}



## Benching

* NCSC ([https://www.ncsc.gov.uk/collection/end-user-device-security/platform-specific-guidance/](https://www.ncsc.gov.uk/collection/end-user-device-security/platform-specific-guidance/))&#x20;
* &#x20;Compare the output
  * Resultant Set of Policy&#x20;
  * Gpresult /H buldreview.html
* CIS&#x20;
  * Level 1&#x20;
  * Run in Nessus

## Download for the file tests

{% file src="../.gitbook/assets/Build-review.zip" %}
