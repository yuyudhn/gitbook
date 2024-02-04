---
description: Basic Enumeration using CMD and Powershell
---

# AD: Enumeration

### Architecture Checks

{% code overflow="wrap" %}
```powershell
ver
wmic os get osarchitecture
```
{% endcode %}

### Hostname Checks

{% code overflow="wrap" %}
```powershell
hostname
# print the domain and username
whoami
```
{% endcode %}

### Network Checks

{% code overflow="wrap" %}
```powershell
ipconfig
ipconfig /all

# print routing table
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

```powershell
netsh advfirewall show currentprofile
netsh firewall show state
```

### Windows Defender Checks

{% code overflow="wrap" %}
```
sc query windefend
```
{% endcode %}

### OS Checks

```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

### Check if the computer is join to domain

{% code overflow="wrap" %}
```
set userdomain
```
{% endcode %}

### Check running proccess

```
tasklist /SVC
```

### What patches are installed?

```
wmic qfe
```

### Check installed app

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
