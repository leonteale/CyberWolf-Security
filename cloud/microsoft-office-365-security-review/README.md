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

The following supporting material provides additional information relating to the associated finding. This section reviews an example's O365 security settings against the technical guide “Office 365 UK Blueprint - Secure Configuration Alignment – Prepared for UK Government, 4/9/2021, Version 2 Final”, comparing with xxxxxx configuration.

## GOOD

### Verify that Microsoft 365 Audit logging is enabled

When audit log search in the compliance centre is turned on, user and admin activity from your organisation is recorded in the audit log and retained for 90 days, and up to one year depending on the license assigned to users.

```
Get-AdminAuditLogConfig | FL UnifiedAuditLogIngestionEnabled
```

A value of _**True**_ for the UnifiedAuditLogIngestionEnabled property indicates that auditing is turned on. A value of _**False**_ indicates that auditing is not turned on.\
\
[https://compliance.microsoft.com/auditlogsearch](https://compliance.microsoft.com/auditlogsearch)

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Reference:\
[https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-enable-disable?view=o365-worldwide](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-enable-disable?view=o365-worldwide)

### Enable mailbox auditing for all users

Certain actions performed by mailbox owners, delegates, and admins are automatically logged, and the corresponding mailbox audit records will be available when you search for them in the mailbox audit log.

```
Get-OrganizationConfig | Format-List AuditDisabled 
```

A value of _**False**_ indicates that mailbox auditing on by default is enabled for your organisation.

Reference:\
[http://www.thatlazyadmin.com/2019/03/26/verify-mailbox-auditing-enabled-default-office-365-tenant/](http://www.thatlazyadmin.com/2019/03/26/verify-mailbox-auditing-enabled-default-office-365-tenant/)

### Use of Microsoft Secure Score service

Regularly reviewing Secure Score allows organisations to monitor for changes to their security posture and re-evaluate as new controls are made available. Microsoft Secure Score is a measurement of an organisation’s security posture, with a higher number indicating more improvement actions taken\
\
[https://compliance.microsoft.com/compliancemanager?viewid=overview](https://compliance.microsoft.com/compliancemanager?viewid=overview)\
\


<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Implement Cloud authentication model for Office 365

Azure Active Directory benefits from an extensive monitoring and security infrastructure. Using machine learning and human intelligence that looks across worldwide traffic can rapidly detect attacks and allow you to reconfigure in near-real-time. With this being considered _**it is recommended that Azure AD password hash synchronization is deployed**_ as the authentication method for Office 365 services.

```
get-adsyncscheduler
```

[https://aad.portal.azure.com/#blade/Microsoft\_AAD\_IAM/DirectoriesA](https://aad.portal.azure.com/#blade/Microsoft\_AAD\_IAM/DirectoriesA)\
\


<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

[https://aad.portal.azure.com/#blade/Microsoft\_AAD\_IAM/UsersManag](https://aad.portal.azure.com/#blade/Microsoft\_AAD\_IAM/UsersManag)\


<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Configure Admin Consent for OAUTH Apps

Configure Azure Active Directory User consent for applications to _**Do not allow user consent.**_

```
Get-AzureADMSAuthorizationPolicy 
```

Azure Active Directory > Enterprise applications > Consent and permissions > User consent settings.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

