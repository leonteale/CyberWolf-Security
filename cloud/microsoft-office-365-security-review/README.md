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

## Review Guidance

The following supporting material provides additional information relating to the associated finding. This section reviews 7KBW’s O365 security settings against the technical guide “Office 365 UK Blueprint - Secure Configuration Alignment – Prepared for UK Government, 4/9/2021, Version 2 Final”, comparing with xxxxxx configuration.

## GOOD

### Verify that Microsoft 365 Audit logging is enabled

When audit log search in the compliance centre is turned on, user and admin activity from your organisation is recorded in the audit log and retained for 90 days, and up to one year depending on the license assigned to users.

```
Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
```

A value of _**True**_ for the UnifiedAuditLogIngestionEnabled property indicates that auditing is turned on. A value of _**False**_ indicates that auditing is not turned on.

[https://compliance.microsoft.com/auditlogsearch](https://compliance.microsoft.com/auditlogsearch)\
\
Reference:\
[https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-enable-disable?view=o365-worldwide](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-enable-disable?view=o365-worldwide)\
