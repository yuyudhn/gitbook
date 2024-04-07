---
description: Windows Privilege Escalation Checks
---

# Privilege Escalation

Privilege escalation checks on Windows involve examining various aspects of the system and its configuration to identify potential vulnerabilities or misconfigurations that could be exploited to elevate privileges.&#x20;

### Labs

* [https://tryhackme.com/r/room/windowsprivescarena](https://tryhackme.com/r/room/windowsprivescarena)
* [https://tryhackme.com/r/room/windows10privesc](https://tryhackme.com/r/room/windows10privesc)

### Tools and Checklists

Here are some common tools and checks used for privilege escalation on Windows systems:

* [x] WinPEAS.exe
* [x] beRoot.exe
* [x] Watson.exe / Invoke-Watson.ps1
* [x] Moriarty.exe
* [x] PowerUp.ps1
* [x] UAC Bypass
* [x] Windows Exploit Suggester - Next Generation (WES-NG)

### WinPEAS

Windows Privilege Escalation Awesome Scripts

* [https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)
* [Invoke-WinPEAS](https://raw.githubusercontent.com/BC-SECURITY/Empire/main/empire/server/data/module\_source/privesc/Invoke-winPEAS.ps1)

### UACME

UAC Bypass

* [https://github.com/yuyudhn/UACME-bin](https://github.com/yuyudhn/UACME-bin)
* [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)

### PowerUp

PowerUp.ps1 is a program that enables a user to perform quick checks against a Windows machine for any privilege escalation opportunities. It is not a comprehensive check against all known privilege escalation techniques, but it is often a good place to start when you are attempting to escalate local privileges.

* [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)
* [Advanced PowerUp.ps1 Usage](https://rootrecipe.medium.com/advanced-powerup-ps1-usage-ad0f6d713a9f)
* [HackTricks: PowerUp](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/powerup)

{% hint style="info" %}
Note: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at yuyudhn@gmail.com.
{% endhint %}
