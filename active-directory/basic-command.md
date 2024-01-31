---
description: The basic of powershell and CMD
---

# Basic Command

### Import script to Powershell

```powershell
powershell -ep bypass
Import-Module .\PowerView.ps1
```

### Basic Enumeration Command

Retrieving local user

```powershell
Get-LocalUser
net user
```

Retrieving the ADUsers List

```powershell
Get-ADUser -Filter * -Properties *
net user /domain
```

To get the specific user accounts details

```powershell
Get-ADUser -Identity support -Properties *
```

Check privileges

```
whoami /all
whoami /priv
```

Check Group List

```powershell
Get-LocalGroup
net localgroup
Get-ADGroup -filter * -properties * |select SAMAccountName, Description
```

Check group member

{% code overflow="wrap" %}
```powershell
Get-LocalGroupMember -Group "Administrators"
Get-ADGroupMember -Identity Administrators | Select-Object name, objectClass,distinguishedName
```
{% endcode %}

### Restart and Reboot

```powershell
 # Shutdown now
 shutdown /s /t 0

 # Restart
 shutdown /r /t 0
```

### Convert to Base64

{% code overflow="wrap" %}
```powershell
[convert]::ToBase64String((Get-Content -path "20230908145747_BloodHound.zip" -Encoding byte))
```
{% endcode %}
