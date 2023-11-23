---
description: https://www.infosecmatter.com/powershell-commands-for-pentesters/
---

# Powershell commands

[https://www.infosecmatter.com/powershell-commands-for-pentesters/](https://www.infosecmatter.com/powershell-commands-for-pentesters/)\
\


#### List installed antivirus (AV) products <a href="#list-installed-antivirus-av-products" id="list-installed-antivirus-av-products"></a>

Here’s a simple PowerShell command to query Security Center and identify all installed Antivirus products on this computer:

{% code overflow="wrap" %}
```powershell
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntiVirusProduct
Get-CimInstance -Namespace root/SecurityCenter -ClassName AntiVirusProduct
```
{% endcode %}
