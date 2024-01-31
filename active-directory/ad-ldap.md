---
description: LDAP Enumeration and Exploitation
---

# AD: LDAP

### **ldapsearch query**

Get domain infomation

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

[https://github.com/dirkjanm/ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump)

```
ldapdomaindump -u 'support\ldap' -p 'p@ssw0rd' dc.support.htb
```
