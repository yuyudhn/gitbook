---
description: Active Directory Enumeration Checklists
---

# AD: Enumeration

## Using PowerView

PowerView is a PowerShell tool to gain network situational awareness on Windows domains. It contains a set of pure-PowerShell replacements for various windows "net \*" commands, which utilize PowerShell AD hooks and underlying Win32 API functions to perform useful Windows domain functionality.

Import PowerView

```powershell
. C:\AD\Tools\PowerView.ps1
```

### User Enumeration

{% code overflow="wrap" %}
```powershell
# Enumerate Domain User
Get-DomainUser 
Get-DomainUser -Identity studentX
Get-DomainUser -Properties samaccountname,logonCount

# Search for a particular string in a user's attributes
Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```
{% endcode %}

### Computer Enumeration

{% code overflow="wrap" %}
```powershell
# Get a list of computers in the current domain 
Get-DomainComputer | select Name,serviceprincipalname
Get-DomainComputer | select -ExpandProperty dnshostname
Get-DomainComputer -OperatingSystem "*Server 2022*" 
Get-DomainComputer -Ping
```
{% endcode %}

### Domain Group Enumeration

```powershell
Get-DomainGroup | select Name
Get-DomainGroup -Identity "Domain Admins"
Get-DomainGroup -Domain moneycorp.local
```

### Domain Group Member Enumeration

{% code overflow="wrap" %}
```powershell
# Get all the members of the Domain Admins group
Get-DomainGroupMember -Identity "Domain Admins" -Recurse

# Get all the members of the Enterprise Admins group
Get-DomainGroupMember -Identity "Enterprise Admins" -Domain moneycorp.local

# Get the group membership for a user
Get-DomainGroup -UserName "studentX"
```
{% endcode %}

### Enumerate OUs

{% code overflow="wrap" %}
```powershell
# Get OUs in a domain
Get-DomainOU
Get-DomainOU | select -ExpandProperty name
Get-DomainOU -Identity StudentMachines

# List all computers in StudentsMachines OU
(Get-DomainOU -Identity StudentMachines).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name
```
{% endcode %}

### Enumerate GPOs

{% code overflow="wrap" %}
```powershell
# Get list of GPO in current domain
Get-DomainGPO 
Get-DomainGPO -ComputerIdentity DCORP-STD_X

# Enumerate GPO applied on StudentMachines OU
(Get-DomainOU -Identity StudentMachines).gplink
Get-DomainGPO -Identity '{7478F170-6A0C-490C-B355-9E4618BC785D}'
```
{% endcode %}

### Enumerate ACLs of Domain Admins

{% code overflow="wrap" %}
```powershell
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -Verbose
```
{% endcode %}

### Check modify perm for RDPUsers

{% code overflow="wrap" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}
```
{% endcode %}

### Enumerate all domains in current forest

{% code overflow="wrap" %}
```powershell
Get-ForestDomain -Verbose
```
{% endcode %}

### Enumerate all trust in current domain

{% code overflow="wrap" %}
```powershell
Get-DomainTrust
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local
```
{% endcode %}

### Find-PSRemotingLocalAdminAccess

Finding computers on which **current user** has Local Administrator privileges.

{% code overflow="wrap" %}
```powershell
. .\Find-PSRemotingLocalAdminAccess.ps1
Find-PSRemotingLocalAdminAccess -Verbose
```
{% endcode %}

## References

* [https://powersploit.readthedocs.io/en/latest/Recon/](https://powersploit.readthedocs.io/en/latest/Recon/)
