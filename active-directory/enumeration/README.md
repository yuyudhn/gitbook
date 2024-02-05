---
description: Basic Enumeration using CMD and Powershell
---

# AD: Enumeration

### Architecture Checks

What arch of windows running? x64? x86?

{% code overflow="wrap" %}
```powershell
ver
wmic os get osarchitecture
```
{% endcode %}

### Hostname Checks

What hostname of this computer?

{% code overflow="wrap" %}
```powershell
hostname
# print the domain and username
whoami
```
{% endcode %}

### Network Checks

ipconfig to displays all the networking information of the current PC your connected to.

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

Windows Firewall checks using netsh in CMD.

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

Find OS version, arch used, and OS name.

```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

### Check if the computer is join to domain

Is this computer join to domain?

{% code overflow="wrap" %}
```powershell
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

### Read Document Metadata

Read metadata of document/files in Powershell

{% code overflow="wrap" %}
```powershell
 Get-Item "C:\Users\Nakano Nino\Documents\docs.docx" | Select-Object -Property *
```
{% endcode %}

### TCP Port Scanner (Powershell)

{% code overflow="wrap" %}
```powershell
1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("172.16.8.1",$_)) "Port $_ is open!"} 2>$null
```
{% endcode %}

### Ping Sweep (CMD)

{% code overflow="wrap" %}
```powershell
for /l %i in (1,1,254) do @ping -n 1 -w 100 172.16.8.%i > nul && echo 172.16.8.%i is up.
```
{% endcode %}
