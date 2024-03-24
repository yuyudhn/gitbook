---
description: The basic of powershell and CMD
---

# Basic Command

This note was created when I was not very familiar with the Windows environment. I didn't know how to restart the machine from the command line, copy files, or import PowerShell scripts, etc.

### Import script to Powershell

```powershell
powershell -ep bypass
Import-Module .\PowerView.ps1
```

### Copy command

```powershell
# copy files
copy payload.exe "C:\Program Files\Service.exe"

# copy directory name Tools to new directory "MyTools"
xcopy Tools "C:\Users\Administrator\Desktop\MyTools" /E /I
```

### Rename command

```powershell
# Rename Payload.exe to Service.exe
ren Payload.exe Service.exe
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
