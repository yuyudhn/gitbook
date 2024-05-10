---
description: Attacking Joomla CMS
---

# Joomla

### Enumeration

We can using **joomscan** to enumerate Joomla CMS.

{% code overflow="wrap" %}
```bash
joomscan  --url http://target.com -ec -r
```
{% endcode %}

Example output:

```bash
   ____  _____  _____  __  __  ___   ___    __    _  _ 
   (_  _)(  _  )(  _  )(  \/  )/ __) / __)  /__\  ( \( )
  .-_)(   )(_)(  )(_)(  )    ( \__ \( (__  /(__)\  )  ( 
  \____) (_____)(_____)(_/\/\_)(___/ \___)(__)(__)(_)\_)
                        (1337.today)
   
    --=[OWASP JoomScan
    +---++---==[Version : 0.0.7
    +---++---==[Update Date : [2018/09/23]
    +---++---==[Authors : Mohammad Reza Espargham , Ali Razmjoo
    --=[Code name : Self Challenge
    @OWASP_JoomScan , @rezesp , @Ali_Razmjo0 , @OWASP

Processing http://target.com ...


[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.10.0

[+] Core Joomla Vulnerability
[++] PHPMailer Remote Code Execution Vulnerability
CVE : CVE-2016-10033
https://www.rapid7.com/db/modules/exploit/multi/http/phpmailer_arg_injection
https://github.com/opsxcq/exploit-CVE-2016-10033
EDB : https://www.exploit-db.com/exploits/40969/

PPHPMailer Incomplete Fix Remote Code Execution Vulnerability
CVE : CVE-2016-10045
https://www.rapid7.com/db/modules/exploit/multi/http/phpmailer_arg_injection
EDB : https://www.exploit-db.com/exploits/40969/


[+] Checking Directory Listing
[++] directory has directory listing : 
http://target.com/administrator/components
http://target.com/administrator/modules
http://target.com/administrator/templates
http://target.com/images/banners
......
```

### Brute Force/Dictionary Attack

* [https://github.com/ajnik/joomla-bruteforce](https://github.com/ajnik/joomla-bruteforce)

{% code overflow="wrap" %}
```bash
sudo python3 joomla-brute.py -u http://target.com -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```
{% endcode %}

### Exploit Search

{% code overflow="wrap" %}
```bash
searchsploit "Joomla" -www
```
{% endcode %}

### Backdooring Joomla

Login with admin credentials. And go to Templates.

{% code overflow="wrap" %}
```
http://target.com/administrator/index.php?option=com_templates&view=templates
```
{% endcode %}

Edit one of the template file. Example: `error.php` at `beez3`.

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjfERNq0H6zfD_9ggzmQBbV9Naan88yGjRB6pr4ZVTfKsfV8Td2liTainV0RfpJRq3lkjgEPxPR4_6Gl9xeBoOcVXHp6toSFEhCjSiFx7SstWJUxyvJMjeuVHxsInU6FFIiJQ5NRgHCM_g4A-WL178rEt1Kjk1PAfGsqVlSzv-MuFeAJQg58GlqBYNVy5U/s1452/edit-joomla-template.png" alt=""><figcaption><p>Edit Joomla Templates</p></figcaption></figure>

And then, go to `target.com/templates/beez3/error.php` to access your backdoor.

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgqIx5AFm2pFhgMUw70EZloHTmymdrvwU0YXsiRTPV5c_9vYbAsRv76IlRRVQAVO7ic1YMV7uChfbda2VwDfmGsRX9QEZPVHs9DiTcj2ReND38Mc7gKYIjiNO_VJIMHFb_7tNN907yrdBKg_HkYaolo80QZ1S73GZPN57-qTg9HhJ0UKRvRGqE7o7PSPbI/s1457/backdoor-joomla.png" alt=""><figcaption><p>Joomla Backdoor</p></figcaption></figure>

Update soon....
