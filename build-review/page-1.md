# Page 1

Checklist



### Prerequisites

* Local administrator and basic user account
* PowerShell we need to be enabled on windows builds
* Port 445 will need to be enabled for authentication scan

Ensure that you make the followihng registry edit in order to be able to perform the remote authenticated nussus scan:

> regedit (run as admin) > HKEY\_LOCAL\_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Policies > System. Right click > New > DWORD (32-bit) Value. LocalAccountTokenFilterPolicy > Right click > Modify > Value data: 1

### Checklist

![](<../.gitbook/assets/image (4).png>)

### Useful commands

Unquoted service paths

```
cmd /c wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
```

### Benching

* NCSC ([https://www.ncsc.gov.uk/collection/end-user-device-security/platform-specific-guidance/](https://www.ncsc.gov.uk/collection/end-user-device-security/platform-specific-guidance/))&#x20;
* &#x20;Compare the output
  * Resultant Set of Policy&#x20;
  * Gpresult /H buldreview.html
* CIS&#x20;
  * Level 1&#x20;
  * Run in Nessus

### Download for the file tests

{% file src="../.gitbook/assets/Build-review.zip" %}
