---
description: Unquoted Service Paths – Windows Privilege Escalation
---

# Unquoted Service Path

### Manual Checks

Check service without quotes:

{% code overflow="wrap" %}
```powershell
wmic service get name,displayname,startmode,pathname | findstr /i /v "C:\Windows\\" |findstr /i /v """

# or

Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName
```
{% endcode %}

Check File or Directory Permissions:

{% code overflow="wrap" %}
```powershell
icacls C:\
icacls "C:\Program Files"

# or
.\accesschk64.exe -wvud "C:\" -accepteula
.\accesschk64.exe -wvud "C:\Program Files" -accepteula
```
{% endcode %}

### PowerUp

{% code overflow="wrap" %}
```powershell
Invoke-AllChecks
Get-ServiceUnquoted
Get-ServiceDetail "ServiceVulns"
```
{% endcode %}
