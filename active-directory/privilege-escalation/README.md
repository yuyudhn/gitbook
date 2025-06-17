---
description: Windows Privilege Escalation Checks
---

# Privilege Escalation

Privilege escalation checks on Windows involve examining various aspects of the system and its configuration to identify potential vulnerabilities or misconfigurations that could be exploited to elevate privileges.&#x20;

### Labs

* [**THM: Windows PrivEsc Arena**](https://tryhackme.com/r/room/windowsprivescarena)
* [**THM: Windows PrivEsc**](https://tryhackme.com/r/room/windows10privesc)
* [**THM: Bypassing UAC**](https://tryhackme.com/r/room/bypassinguac)

### Tools and Checklists

Here are some common tools and checks used for privilege escalation on Windows systems:

* [x] WinPEAS.exe
* [x] PrivescCheck.ps1
* [x] beRoot.exe
* [x] Watson.exe / Invoke-Watson.ps1
* [x] Moriarty.exe
* [x] PowerUp.ps1
* [x] UAC Bypass
* [x] Windows Exploit Suggester - Next Generation (WES-NG)
* [x] GodPotato

### WinPEAS

Windows Privilege Escalation Awesome Scripts

* [https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)
* [Invoke-WinPEAS](https://raw.githubusercontent.com/BC-SECURITY/Empire/main/empire/server/data/module_source/privesc/Invoke-winPEAS.ps1)

### Unattended Windows Installations

When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:

* C:\Unattend.xml
* C:\Windows\Panther\Unattend.xml
* C:\Windows\Panther\Unattend\Unattend.xml
* C:\Windows\system32\sysprep.inf
* C:\Windows\system32\sysprep\sysprep.xml

### UACME

UAC Bypass

* [https://github.com/yuyudhn/UACME-bin](https://github.com/yuyudhn/UACME-bin)
* [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)

### PowerUp

PowerUp.ps1 is a program that enables a user to perform quick checks against a Windows machine for any privilege escalation opportunities. It is not a comprehensive check against all known privilege escalation techniques, but it is often a good place to start when you are attempting to escalate local privileges.

* [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)
* [Advanced PowerUp.ps1 Usage](https://rootrecipe.medium.com/advanced-powerup-ps1-usage-ad0f6d713a9f)
* [HackTricks: PowerUp](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/powerup)

```powershell
powershell -ep bypass
. .\PowerUp.ps1
Invoke-AllChecks
```

### PrivescCheck

This script aims to identify Local Privilege Escalation (LPE) vulnerabilities that are usually due to Windows configuration issues, or bad practices. It can also gather useful information for some exploitation and post-exploitation tasks.

* [https://github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck)

{% code overflow="wrap" %}
```powershell
# Basic checks
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Risky"
# Extended checks
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,HTML"
# All checks
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck -Extended -Audit -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,HTML"
```
{% endcode %}

{% hint style="info" %}
Note: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at yuyudhn@gmail.com.
{% endhint %}
