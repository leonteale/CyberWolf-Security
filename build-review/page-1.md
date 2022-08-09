# Page 1

Checklist



### Prerequisites

* Local administrator and basic user account
* PowerShell we need to be enabled on windows builds
* Port 445 will need to be enabled for authentication scan

Category Checks Result Pre-boot checks Can you get to boot menu yes Is there a BIOS password set no Hard Drive Encryption Is Hard drive encryption used yes Antivirus Checks Antivirus Software installed Is it up to date Can it be stopped or disabled by user or admin Can you download and execute files Removable Media Can you read and write to a USB Data Mining for sensitive files Unattened.xml Management tools Powershell yes CMD yes MMC yes FTP yes Restricted Access C:\ drive read or write yes Is UAC enabled / password required yes

Execute file types BAT yes EXE Powershell Python Authenticated Vulnerability scan Nessus scan Build Review Details Operating System and Version Windows 10 enterprise x64 Accessible Services Missing Patches Miscellaneous vulnerabilities LAPS

Benching

* NCSC (https://www.ncsc.gov.uk/collection/end-user-device-security/platform-specific-guidance/) o Compare the output  Resultant Set of Policy  Gpresult /H buldreview.html
* CIS o Level 1 o Run in Nessus

### Download for the file tests

{% file src="../.gitbook/assets/Build-review.zip" %}
