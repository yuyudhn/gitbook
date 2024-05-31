---
description: Active Directory Enumeration Checklists with PowerView
---

# PowerView

## Using PowerView

PowerView is a PowerShell tool to gain network situational awareness on Windows domains. It contains a set of pure-PowerShell replacements for various windows "net \*" commands, which utilize PowerShell AD hooks and underlying Win32 API functions to perform useful Windows domain functionality.

Tools:

* [**PowerView.ps1**](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1)

### Import PowerView

{% code overflow="wrap" %}
```powershell
. C:\AD\Tools\PowerView.ps1
```
{% endcode %}

### Get-DomainController

Enumerates the domain controllers for the current or specified domain. By default built in .NET methods are used. The -LDAP switch uses Get-DomainComputer to search for domain controllers.

{% code overflow="wrap" %}
```powershell
Get-DomainController
Get-DomainController -Domain domain.lab
Get-DomainController -Domain domain.lab -LDAP
```
{% endcode %}

### Get-DomainUser

Builds a directory searcher object using Get-DomainSearcher, builds a custom LDAP filter based on targeting/filter parameters, and searches for all objects matching the criteria.

{% code overflow="wrap" %}
```powershell
# Enumerate Domain User
Get-DomainUser
Get-DomainUser -Domain domain.lab 
Get-DomainUser -Identity "Asuka-Soryu"
Get-DomainUser -Properties samaccountname,logonCount

# Search for a particular string in a user's attributes
Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```
{% endcode %}

### Get-DomainComputer

Return all computers or specific computer objects in AD.

{% code overflow="wrap" %}
```powershell
# Get a list of computers in the current domain 
Get-DomainComputer | select Name,serviceprincipalname
Get-DomainComputer | select -ExpandProperty dnshostname
Get-DomainComputer -OperatingSystem "*Server 2022*" 
Get-DomainComputer -Ping

# Check Constrained Delegation
Get-DomainComputer -TrustedToAuth

# Return computer objects that have unconstrained delegation
Get-DomainComputer -Unconstrained
```
{% endcode %}

### Get-DomainGroup

Return all groups or specific group objects in AD.

```powershell
Get-DomainGroup | select Name
Get-DomainGroup -Identity "Domain Admins"
Get-DomainGroup -Domain domain.lab
```

### Get-DomainGroupMember

Return the members of a specific domain group.

{% code overflow="wrap" %}
```powershell
# Get all the members of the Domain Admins group
Get-DomainGroupMember -Identity "Domain Admins" -Recurse

# Get all the members of the Enterprise Admins group
Get-DomainGroupMember -Identity "Enterprise Admins" -Domain domain.lab

# Get the group membership for a user
Get-DomainGroup -UserName "Asuka-Soryu"
```
{% endcode %}

### Get-DomainOU

Search for all organization units (OUs) or specific OU objects in AD.

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

### Get-DomainObjectAcl

Returns the ACLs associated with a specific active directory object.

{% code overflow="wrap" %}
```powershell
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -Verbose
```
{% endcode %}

### Find-InterestingDomainAcl

Finds object ACLs in the current (or specified) domain with modification rights set to non-built in objects.

{% code overflow="wrap" %}
```powershell
Find-InterestingDomainAcl -ResolveGUIDs
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}

# Quick shot to find RBCD, etc
Find-InterestingDomainAcl -ResolveGUIDs | Where-Object {$_.SamAccountName -ne "Domain Admins" -and $_.SamAccountName -ne "Account Operators" -and $_.SamAccountName -ne "Enterprise Admins" -and $_.SamAccountName -ne "Administrators" -and $_.SamAccountName -ne "DnsAdmins" -and $_.SamAccountName -ne "Schema Admins" -and $_.SamAccountName -ne "Key Admins" -and $_.SamAccountName -ne "Enterprise Key Admins" -and $_.SamAccountName -ne "Storage Replica Administrators"  -and $_.IdentityReferenceName -ne "DnsAdmins"} | ?{($_.ObjectType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ActiveDirectoryRights -match 'WriteDacl') -or ($_.ActiveDirectoryRights -match 'WriteProperty')}
```
{% endcode %}

### Get-ForestDomain

Return all domains for the current (or specified) forest.

{% code overflow="wrap" %}
```powershell
Get-ForestDomain -Verbose
Get-ForestDomain -Forest domain.lab
```
{% endcode %}

### Get-DomainTrust

{% code overflow="wrap" %}
```powershell
Get-DomainTrust
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local
```
{% endcode %}

### Get-DomainSID

Returns the SID for the current domain or the specified domain.

{% code overflow="wrap" %}
```powershell
Get-DomainSID
Get-DomainSID -Domain domain.lab
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
