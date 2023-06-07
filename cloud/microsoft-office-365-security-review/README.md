# Microsoft Office 365 Security Review

## Pre-requisites

### User account

User account with ‘Global Reader’ permissions

### Guidance document

The Microsoft Technical Guide PDF can be found [here](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWBft1).

## Connecting to the tenancy via PowerShell

```
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted 
Set-ExecutionPolicy RemoteSigned 

```

```
Install-Module -Name MSOnline 
Install-Module -Name AzureADPreview 
Install-Module -Name ExchangeOnlineManagement 
Install-Module -Name Microsoft.Online.SharePoint.PowerShell 
Install-Module -Name MicrosoftTeams 
```

```
Get-Credential 
Connect-MsolService -Credential $M365credentials 
Connect-AzureAD -Credential $M365credentials 
Get-MsolCompanyInformation ( Checking its working) 
Connect-ExchangeOnline -Credential $M365credentials 
Connect-IPPSSession -Credential $M365Credentials 
$orgName = “upstarttech” 
Connect-SPOService -Url “https://$orgName-admin.sharepoint.com” -Credential 
$M365Credentials 
Connect-MicrosoftTeams -Credential $M365credentials 
```
