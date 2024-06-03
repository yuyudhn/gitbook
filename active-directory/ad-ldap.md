---
description: LDAP Enumeration and Exploitation
---

# LDAP

### **ldapsearch**

Get domain infomation (anonymous bind)

```bash
ldapsearch -H ldap://192.168.1.123 -x -s base namingcontexts
```

Query with credentials

```bash
ldapsearch -x -H 192.168.12.134 -D 'DOMAIN\user' -w 'password' -b "DC=target,DC=htb"
```

Username Enumeration

```bash
ldapsearch -H ldap://192.168.1.123 -x -b "DC=target,DC=htb" "(objectClass=person)" | \
grep "sAMAccountName:"
```

### **ldapdump**

* [https://github.com/dirkjanm/ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump)

{% code overflow="wrap" %}
```bash
ldapdomaindump -u 'support\ldap' -p 'p@ssw0rd' dc.support.htb

# Parse Computer Lists
cat domain_computers.json | jq -r .[].attributes.dNSHostName[]

# Parse Domain Users
cat domain_users.json | jq -r .[].attributes.sAMAccountName[]
```
{% endcode %}

### windapsearch

* [https://github.com/ropnop/go-windapsearch](https://github.com/ropnop/go-windapsearch)

{% code overflow="wrap" %}
```bash
windapsearch --dc 172.16.8.139 --module users # anonymous bind
windapsearch --dc 172.16.8.139 -d evangelion.lab -u 'asuka' -p 'P@ssw0rd2033' -m users # authenticated
windapsearch --dc 172.16.8.139 -d evangelion.lab -u 'asuka' -p 'P@ssw0rd2033' -m members -g 'CN=EvaDriver,OU=EVA,DC=EVANGELION,DC=lab'
```
{% endcode %}
