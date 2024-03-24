---
description: Basic SMB Enumeration and Exploitation
---

# SMB

### Basic Enumeration

{% code overflow="wrap" %}
```bash
# List all NetExec modules
netexec smb 172.16.8.139 -L

# Check SMB version
netexec smb 172.16.8.139

# Check if guest is not disabled
netexec smb 172.16.8.139 -u guest -p '' --shares

# Check using valid account
netexec smb 172.16.8.139 -u asuka -p 'p@ssw0rd' --shares

# check with local account (not domain account)
netexec smb 172.16.8.139 -u asuka -p 'p@ssw0rd' --shares --local-auth

# Check SMB Service on subnet
netexec smb 172.16.8.139/24

# Check null auth
smbclient -L 192.168.1.2 --no-pass
smbclient //192.168.1.2/public --no-pass
```
{% endcode %}

### Password Spraying

{% code overflow="wrap" %}
```bash
netexec smb 172.16.8.139 -u username-lists.txt -p 'p@ssw0rd'
```
{% endcode %}

### RID Cycling - Username Enumeration

{% code overflow="wrap" %}
```bash
# RID Cycling with Guest 
netexec smb 172.16.8.139 -u guest -p '' --rid-brute

# RID Cycling with valid account
netexec smb 172.16.8.139 -u evasvc -p 'Serviceworks1' --rid-brute

# or with impacket-lookupsid
impacket-lookupsid 'evasvc':'Serviceworks1'@evangelion.lab

```
{% endcode %}

### SMBClient

```bash
# no pass
smbclient //192.168.1.2/public --no-pass

# with creds
smbclient //172.16.8.139/SYSVOL -U  'evasvc'%'Serviceworks1' -p 445

# download all files from shares
smbclient '//10.10.11.174/support-tools' -N -c 'prompt OFF;recurse ON;mget *'

```

### Vuln Checks

{% code overflow="wrap" %}
```bash
nmap -Pn --script 'smb-vuln*' -p 139,445 172.16.8.140
```
{% endcode %}

Updated soon...
