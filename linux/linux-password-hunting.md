---
description: Common Password Attack on Linux Machine
---

# Linux Password Hunting

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
find /home/* -type f -name "*.txt"
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

{% code overflow="wrap" %}
```bash
grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```
{% endcode %}

### File History

{% code overflow="wrap" %}
```bash
find / -type f -name "*history" 2>/dev/null
tail -n5 /home/*/.*_history*
```
{% endcode %}

### Search Password in PHP Files

{% code overflow="wrap" %}
```bash
grep -iRl "password\|passwd" /var/www --include=*.php
```
{% endcode %}

### Cracking Linux Credentials

{% code overflow="wrap" %}
```bash
# unshadow local creds
unshadow passwd.bak shadow.bak > unshadow.txt
# Perform Dictionary Attack
hashcat -m 1800 -a 0 unshadow.txt /usr/share/wordlists/rockyou.txt -o cracked_shadow
```
{% endcode %}

Take a look at the unshadow.txt file. The field after the username (with a number or letter between two dollar signs) is the one that identifies the hash type used. It could be one of the following:

1. **$1$** is MD5
2. **$2a$** is Blowfish
3. **$2y$** is Blowfish
4. **$5$** is SHA-256
5. **$6$** is SHA-512
6. **$y$** is yescrypt

For **$y$**, for example, you can use the command:

{% code overflow="wrap" %}
```bash
john --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt unshadow.txt
```
{% endcode %}

### Tools

* [https://github.com/unode/firefox\_decrypt](https://github.com/unode/firefox\_decrypt)
* [https://github.com/huntergregal/mimipenguin](https://github.com/huntergregal/mimipenguin)
* [https://github.com/AlessandroZ/LaZagne/tree/master/Linux](https://github.com/AlessandroZ/LaZagne/tree/master/Linux)
