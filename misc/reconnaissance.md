---
description: >-
  Common reconnaissance phase steps on HackTheBox machines (or during
  penetration testing).
---

# Reconnaissance

## Port Scanning

When obtaining the IP address of a HackTheBox machine, one essential task is to perform port scanning. The most powerful tool for conducting port scanning is nmap.

#### TCP ports

```bash
sudo nmap -sV -sT -sC -oA nmap_initial target.htb
sudo nmap -Pn  -p- target.htb -T4 -oA nmap_full
```

#### **UDP Ports**

```bash
sudo nmap -Pn -sU --top-ports 1000 -sV -sC -vv --min-rate=5000 \
-oA udpinitial target.htb
```

## Directory Scanning

### Directory Scanning with FFuF

{% code overflow="wrap" %}
```bash
ffuf -recursion-depth 3 -t 100 -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt \
-u http://target.htb/FUZZ
```
{% endcode %}

## Subdomain / Virtualhost Brute

### Vhost Brute with FFuF

{% code overflow="wrap" %}
```bash
ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt \
-u http://target.htb -H "Host: FUZZ.target.htb"
# or
ffuf -c -r -w "/usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt" -u "http://FUZZ.linuxsec.org"
```
{% endcode %}

### Subdomain Bruteforce with PureDNS

{% code overflow="wrap" %}
```bash
# https://github.com/blechschmidt/massdns
git clone https://github.com/blechschmidt/massdns
cd massdns
make
sudo make install

# https://github.com/d3mondev/puredns
➜  tmp cat resolver.txt
1.1.1.1
8.8.8.8

puredns bruteforce --resolvers resolver.txt /usr/share/seclists/Discovery/DNS/dns-Jhaddix.txt linuxsec.org
```
{% endcode %}

### Subfinder

{% code overflow="wrap" %}
```bash
# https://github.com/projectdiscovery/subfinder
subfinder -d linuxsec.org
```
{% endcode %}

## Technology Detection

### WhatWeb

```bash
whatweb https://blog.linuxsec.org -a 3 -v

# Example output:
Status    : 200 OK
Title     : LinuxSec Blog &#8212; Linux Tutorial for Beginners
IP        : 104.21.95.121
Country   : UNITED STATES, US
......
```

### wafw00f

{% code overflow="wrap" %}
```bash
wafw00f https://blog.linuxsec.org

# Example output:
.........                                                                                                                                                                                                                                           
[*] Checking https://blog.linuxsec.org
[+] The site https://blog.linuxsec.org is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2
```
{% endcode %}

### webanalyze

* [https://github.com/rverton/webanalyze](https://github.com/rverton/webanalyze)

{% code overflow="wrap" %}
```bash
webanalyze -host blog.linuxsec.org

# Example output:
..........
http://blog.linuxsec.org (2.3s):
    AMP,  (JavaScript frameworks)
    Cloudflare,  (CDN)
    Yoast SEO,  (SEO, WordPress plugins)
    WordPress,  (CMS, Blogs)
    Redis Object Cache,  (Caching)
    Redis,  (Databases)
    WordPress,  (CMS, Blogs)
    HTTP/3,  (Miscellaneous)
    HSTS,  (Security)
    WordPress, 6.2.3 (CMS, Blogs)
    PHP,  (Programming languages)
    MySQL,  (Databases)
```
{% endcode %}

## JavaScript Analyze

### **LinkFinder**

[https://github.com/GerbenJavado/LinkFinder](https://github.com/GerbenJavado/LinkFinder) - LinkFinder is a python script written to discover endpoints and their parameters in JavaScript files.

{% code overflow="wrap" %}
```bash
python3 linkfinder.py -i http://testphp.vulnweb.com -d --output cli
```
{% endcode %}

### **SecretFinder**

[https://github.com/m4ll0k/SecretFinder](https://github.com/m4ll0k/SecretFinder) - SecretFinder is a python script based on LinkFinder, written to discover sensitive data like apikeys, accesstoken, authorizations, jwt,..etc in JavaScript files.

{% code overflow="wrap" %}
```bash
python3 SecretFinder.py -i http://testhtml5.vulnweb.com -e --output cli
```
{% endcode %}

### GoLinkFinder

[https://github.com/0xsha/GoLinkFinder](https://github.com/0xsha/GoLinkFinder) - A minimal JS endpoint extractor

{% code overflow="wrap" %}
```bash
GoLinkFinder -d http://testphp.vulnweb.com
```
{% endcode %}

## Web Crawling

Use Katana - [https://github.com/projectdiscovery/katana/](https://github.com/projectdiscovery/katana/)

{% code overflow="wrap" %}
```bash
katana -u https://linuxsec.org
```
{% endcode %}

## Vulnerability Assessment

### Wapiti

{% code overflow="wrap" %}
```bash
wapiti --help
wapiti --url http://testphp.vulnweb.com
```
{% endcode %}

### Nikto

{% code overflow="wrap" %}
```bash
nikto -Help
nikto -h http://testphp.vulnweb.com
```
{% endcode %}

{% hint style="info" %}
**Note**: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at [yuyudhn@gmail.com](mailto:yuyudhn@gmail.com).
{% endhint %}
