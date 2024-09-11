---
description: Basic Enumeration using CMD and Powershell
---

# Enumeration

### User Enumeration

Local user checks

{% code overflow="wrap" %}
```powershell
# check local user
net user
# check local admin member
net localgroup administrators
# check user "asuka"
net user asuka
```
{% endcode %}

Check local user privileges

{% code overflow="wrap" %}
```powershell
# check priv
whoami /priv
whoami /all
# check group information of current user
whoami /groups
whoami /user /groups
```
{% endcode %}

Enumerate domain user

```powershell
# check domain user
net user /domain
# check domain user "asuka-domain"
net user asuka-domain /domain
# check domain group
net group /domain
# Check Domain Admins member
net group "Domain Admins" /domain
```

### Architecture Checks

What the Windows version installed? What aarch of windows running? x64? x86?

**Why**: Knowing this information will make the decision about the exploit or the crafted payload better.

{% code overflow="wrap" %}
```powershell
ver
wmic os get osarchitecture
```
{% endcode %}

### Hostname Checks

What hostname of this computer? Is this computer join AD?

**Why**: Knowing this information will make the attack path decision clear.

{% code overflow="wrap" %}
```powershell
hostname
whoami
systeminfo | findstr /B "Domain"
set userdomain
```
{% endcode %}

### Network Checks

**Why**: Knowing this information will improve the mapping of the internal network pentest.

ipconfig to displays all the networking information of the current PC your connected to.

{% code overflow="wrap" %}
```powershell
ipconfig
ipconfig /all
route PRINT
```
{% endcode %}

netstat is a command-line network utility that displays network connections for Transmission Control Protocol, routing tables, and a number of network interface and network protocol.

{% code overflow="wrap" %}
```powershell
netstat
netstat -ano
```
{% endcode %}

### Firewall Checks

Windows Firewall checks using netsh in CMD.

**Why**: Knowing this information will improve the decision-making process for data transfer.

```powershell
netsh advfirewall show currentprofile
netsh advfirewall show allprofiles
netsh firewall show state
Get-Service -Name mpssvc # powershell
```

### Defender Checks

**Why**: Knowing this information will help determine whether AV evasion is necessary.

{% code overflow="wrap" %}
```powershell
sc query windefend # cmd
Get-Service -Name windefend # powershell
```
{% endcode %}

### OS Checks

Find OS version, arch used, and OS name.

**Why**: Knowing this information is necessary for searching public exploits against the installed version.

```powershell
systeminfo
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

### Running Proccess

Enumerate the running proccess.

**Why**: Knowing this information is necessary for searching public exploits against the running service or proccess.

```
tasklist /SVC
```

### Installed Patches

What patches are installed?

Why: Knowing this information is necessary for searching public exploits against the installed patches.

```powershell
wmic qfe
```

### Installed Apps

What app installed in this computer?

**Why**: Knowing this information is necessary for searching public exploits against the installed apps.

{% code overflow="wrap" %}
```powershell
wmic product get name, version, vendor
```
{% endcode %}

### Search cleartext creds

{% code overflow="wrap" %}
```powershell
findstr /si password *.txt
findstr /si password *.xml
findstr /si password *.ini

# Find all those strings in config files.
dir /s *pass* == *cred* == *vnc* == *.config*

# Find all passwords in all files.
findstr /spin "password" *.*
findstr /spin "password" *.*
```
{% endcode %}

### Scheduled Task Checks

{% code overflow="wrap" %}
```powershell
Schtasks /query /fo LIST /v
```
{% endcode %}

### Document Metadata

Read metadata of document/files in Powershell

{% code overflow="wrap" %}
```powershell
 Get-Item "C:\Users\Nakano\Documents\docs.docx" | Select-Object -Property *
```
{% endcode %}

### Check Powershell History

{% code overflow="wrap" %}
```powershell
(Get-PSReadlineOption).HistorySavePath
```
{% endcode %}

Or, the default location for the PowerShell command history:

```
%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHo
st_history.txt
```

i.e

```
C:\Users\student\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\Console
Host_history.txt
```

### Check PowerShell Transcript

When a user starts a PS transcript the command log file is generated. The default location for the PowerShell Transcript is: **C:\Users%username%\Documents i.e C:\Users\student\Documents**.

{% code overflow="wrap" %}
```powershell
cd C:\Users\student\Documents
ls
cat PowerShell_transcript.PRIV-ESC.uFAz6NOs.20201029121705.txt | Select-String -Pattern
"pass*"
```
{% endcode %}

### Check Registry Autologon

{% code overflow="wrap" %}
```powershell
reg query 'HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon' /v
DefaultUserName
reg query 'HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon' /v
DefaultPassword
reg query 'HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon' /v
AutoAdminLogon
```
{% endcode %}

### Resources

* [**Windows - Privilege Escalation**](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/)
* [**Windows - Persistence**](https://swisskyrepo.github.io/InternalAllTheThings/redteam/persistence/windows-persistence/)

{% hint style="warning" %}
**Note**: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at [yuyudhn@gmail.com](mailto:yuyudhn@gmail.com).
{% endhint %}
