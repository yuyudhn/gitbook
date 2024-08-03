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
# Check SMB Service on subnet
netexec smb 172.16.8.139/24

# Check null auth
smbclient -L 192.168.1.2 --no-pass
smbclient //192.168.1.2/public --no-pass
```
{% endcode %}

### Enumerate Null Sessions

Check if Null Session, also known as Anonymous session, is enabled on the network. Can be very useful on a Domain Controller to enumerate users, groups, password policies, etc.

```bash
netexec smb 172.16.8.139 -u 'nonexistusers' -p ''
netexec smb 172.16.8.139 -u 'nonexistusers' -p '' --shares
```

### Enumerate Guest Logon

Using a random username and password you can check if the target accepts guest logon. If so, it means that either the domain guest account or the local guest account of the server you're targetting is enabled.

{% code overflow="wrap" %}
```bash
netexec smb 172.16.8.139 -u Guest -p ''
netexec smb 172.16.8.139 -u Guest -p '' --shares
```
{% endcode %}

### SMB Signing Not Required

Maps the network of live hosts and saves a list of only the hosts that don't require SMB signing. List format is one IP per line.

{% code overflow="wrap" %}
```bash
netexec smb 192.168.1.0/24 --gen-relay-list relay_list.txt
```
{% endcode %}

Reference:

* [https://www.netexec.wiki/smb-protocol/enumeration](https://www.netexec.wiki/smb-protocol/enumeration)

### Dictionary Attack and Password Spraying

Dictionary attack with username and password lists.

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.10.10.1 smb2
```

Password spraying with NetExec

{% code overflow="wrap" %}
```bash
netexec smb 10.10.10.1 -u /usr/share/metasploit-framework/data/wordlists/common_users.txt -p 'Passw0rd' --continue-on-success
```
{% endcode %}

### Authenticated Enumeration

Shares check with credentials

{% code overflow="wrap" %}
```bash
# Check using valid account
netexec smb 172.16.8.139 -u asuka -p 'p@ssw0rd' --shares

# check with local account (not domain account)
netexec smb 172.16.8.139 -u asuka -p 'p@ssw0rd' --shares --local-auth
```
{% endcode %}

Username enumeration on Workstation using valid credentials

{% code overflow="wrap" %}
```bash
netexec smb 10.10.100.206 -u 'Jon' -p 'alqfna22' --users
```
{% endcode %}

Enum disk

{% code overflow="wrap" %}
```bash
netexec smb 172.16.8.140 -u 'Administrator' -p 'Password123' --disk --local-auth
```
{% endcode %}

### Password Policy Check

Check password policy on Domain/Workstation

{% code overflow="wrap" %}
```bash
netexec smb 172.16.8.139 -u 'administrator' -p 'passw0rd' --pass-pol
```
{% endcode %}

Example output:

```bash
....................
SMB         172.16.8.139    445    EVANGELION-PC    [+] Dumping password info for domain: EVANGELION
SMB         172.16.8.139    445    EVANGELION-PC    Minimum password length: 7
SMB         172.16.8.139    445    EVANGELION-PC    Password history length: 24
SMB         172.16.8.139    445    EVANGELION-PC    Maximum password age: 41 days 23 hours 53 minutes 
SMB         172.16.8.139    445    EVANGELION-PC    
SMB         172.16.8.139    445    EVANGELION-PC    Password Complexity Flags: 000000
SMB         172.16.8.139    445    EVANGELION-PC        Domain Refuse Password Change: 0
SMB         172.16.8.139    445    EVANGELION-PC        Domain Password Store Cleartext: 0
SMB         172.16.8.139    445    EVANGELION-PC        Domain Password Lockout Admins: 0
SMB         172.16.8.139    445    EVANGELION-PC        Domain Password No Clear Change: 0
SMB         172.16.8.139    445    EVANGELION-PC        Domain Password No Anon Change: 0
SMB         172.16.8.139    445    EVANGELION-PC        Domain Password Complex: 0
SMB         172.16.8.139    445    EVANGELION-PC    
SMB         172.16.8.139    445    EVANGELION-PC    Minimum password age: 1 day 4 minutes 
SMB         172.16.8.139    445    EVANGELION-PC    Reset Account Lockout Counter: 30 minutes 
SMB         172.16.8.139    445    EVANGELION-PC    Locked Account Duration: 30 minutes 
SMB         172.16.8.139    445    EVANGELION-PC    Account Lockout Threshold: None
SMB         172.16.8.139    445    EVANGELION-PC    Forced Log off Time: Not Set
```

### Password Spraying

{% code overflow="wrap" %}
```bash
netexec smb 172.16.8.139 -u username-lists.txt -p 'p@ssw0rd'
```
{% endcode %}

### RID Cycling - Username Enumeration

{% code overflow="wrap" %}
```bash
# NetExec
netexec smb 172.16.8.139 -u evasvc -p 'Serviceworks1' --rid-brute

# Impacket-lookupsid
impacket-lookupsid 'evasvc':'Serviceworks1'@evangelion.lab
impacket-lookupsid 'evasvc':'Serviceworks1'@evangelion.lab | grep SidTypeUser | cut -d' ' -f2 | cut -d'\' -f2
```
{% endcode %}

### SMBClient

```bash
# no pass
smbclient //192.168.1.2/public --no-pass
# with creds
smbclient //172.16.8.139/secrets-folder -U  'username'%'p@ssw0rd' -p 445
# download all files from shares
smbclient '//10.10.11.174/support-tools' -N -c 'prompt OFF;recurse ON;mget *'
```

### Vuln Checks

{% code overflow="wrap" %}
```bash
nmap -Pn --script 'smb-vuln*' -p 139,445 10.10.100.206
netexec smb 10.10.100.206 -u '' -p '' -M ms17-010
```
{% endcode %}

Example output:

```bash
Starting Nmap 7.93 ( https://nmap.org ) at 2024-03-24 23:50 WIB
Nmap scan report for 10.10.100.206 (10.10.100.206)
Host is up (0.38s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx

Nmap done: 1 IP address (1 host up) scanned in 13.31 seconds
```

```bash
netexec smb 10.10.100.206 -u '' -p '' -M ms17-010

SMB         10.10.100.206   445    JON-PC           [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:JON-PC) (domain:Jon-PC) (signing:False) (SMBv1:True)
SMB         10.10.100.206   445    JON-PC           [+] Jon-PC\: 
MS17-010                                            [+] 10.10.100.206 is likely VULNERABLE to MS17-010! (Windows 7 Professional 7601 Service Pack 1)
```

Updated soon...
