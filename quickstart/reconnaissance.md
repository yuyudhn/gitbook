---
description: >-
  Common reconnaissance phase steps on HackTheBox machines or during penetration
  testing.
icon: face-viewfinder
---

# Reconnaissance

## NmapAutomator

Well, I like to use NmapAutomator in my daily pentest activity. I just run NmapAutomator, light a cigarette, and wait for this awesome recon tool to do the job.

{% code overflow="wrap" %}
```bash
nmapAutomator.sh --host 10.10.11.11 --output nmapautomator_res --type Recon
```
{% endcode %}

## Port Scanning

When obtaining the IP address of a HackTheBox machine, one essential task is to perform port scanning. The most powerful tool for conducting port scanning is nmap.

### TCP ports

```bash
rustscan -a 10.10.11.33 -- -Pn -sVC -oN 10.10.11.33_scan
```

### **UDP Ports**

```bash
sudo nmap -Pn -sU --top-ports 1000 -sV -sC -vv --min-rate=5000 \
-oA udpinitial target.htb
```

## Directory Scanning

Directory scanning involves scouring the targeted directories or files on a web server to uncover security vulnerabilities. A web server typically allows access to specific directories and files. However, if these directories or files are not managed carefully by web administrators, attackers can exploit these exposed areas to gain access to sensitive information.

Directory scanning with Feroxbuster.

{% code overflow="wrap" %}
```bash
feroxbuster -C 404 --random-agent --auto-tune -k --wordlist /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt -x php --threads 100 -u http://testphp.vulnweb.com
```
{% endcode %}

## Subdomain / Virtualhost Brute

During the testing reconnaissance phase, testers spend time on virtual host enumeration, which is the process of discovering all the virtual hosts associated with a particular IP address or domain. This helps them find hidden or undocumented assets that might be vulnerable or misconfigured.

### FFuF

Vhost Brute with FFuF (CTF style)

{% code overflow="wrap" %}
```bash
ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://target.htb -H "Host: FUZZ.target.htb"
```
{% endcode %}

### PureDNS

Subdomain Bruteforce with PureDNS.

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

Subdomain Enumeration with Subfinder

{% code overflow="wrap" %}
```bash
# https://github.com/projectdiscovery/subfinder
subfinder -d linuxsec.org
```
{% endcode %}

## Technology Detection

Technology detection is a crucial aspect of penetration testing, providing insights into the software, frameworks, and platforms utilized by the target system. By employing various tools and techniques, penetration testers can identify the underlying technologies, versions, and configurations present, facilitating a deeper understanding of potential vulnerabilities and attack vectors. Amazing tool to perform technology detection is Browser add-ons Wappalyzer, or cli tool [**webanalyze**](https://github.com/rverton/webanalyze).

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

## WAF Detection

In web pentesting, knowing your target, including their WAF, is an important step because you need a different approach when your target is protected by a WAF.

{% code overflow="wrap" %}
```bash
# wafw00f
wafw00f https://blog.linuxsec.org
# nuclei
nuclei -t ./http/fuzzing/waf-fuzz.yaml -itags fuzz -silent -u https://blog.linuxsec.org/
```
{% endcode %}

## JavaScript Analyze

If you’re pentesting web applications, you certainly come across a lot of JavaScript. Nearly every web application nowadays is using it. Frameworks like Angular, React and Vue.js place a lot of functionality and business logic of web applications into the front end. Thus, to thoroughly pentest web applications, you have to analyze their client-side JavaScript.

### **LinkFinder**

LinkFinder is a python script written to discover endpoints and their parameters in JavaScript files.

* [https://github.com/GerbenJavado/LinkFinder](https://github.com/GerbenJavado/LinkFinder)

{% code overflow="wrap" %}
```bash
python3 linkfinder.py -i http://testphp.vulnweb.com -d --output cli
```
{% endcode %}

### **SecretFinder**

SecretFinder is a python script based on LinkFinder, written to discover sensitive data like apikeys, accesstoken, authorizations, jwt,..etc in JavaScript files.

* [https://github.com/m4ll0k/SecretFinder](https://github.com/m4ll0k/SecretFinder)

{% code overflow="wrap" %}
```bash
python3 SecretFinder.py -i http://testhtml5.vulnweb.com -e --output cli
```
{% endcode %}

## Web Crawling

Another approach to finding endpoints on your targets. Some people like using a command-line spider for gathering endpoints. Katana is one of these security focused spiders.

[**katana**](https://github.com/projectdiscovery/katana) by Projectdiscovery.

{% code overflow="wrap" fullWidth="false" %}
```bash
katana -u http://testphp.vulnweb.com/ -headless -js-crawl -jsluice -display-out-scope -ef png,css,jpg
```
{% endcode %}

**Tips**

When using katana:

1. use "-headless" as modern CDN WAFs block many command-line spiders.
2. use "-js-crawl" to enable javascript parsing.
3. use "-jsluice" to enable syntax-tree (better) javascript parsing.
4. use "-display-out-scope" to know when the spider find links to other domains that might be related to your target.

Reference: [https://x.com/Jhaddix/status/1802537881694544192](https://x.com/Jhaddix/status/1802537881694544192)

## Vulnerability Assessment

Quick shot to find some 'low-hanging fruit' findings.

{% code overflow="wrap" %}
```bash
# wapiti
wapiti --help
wapiti --url http://testphp.vulnweb.com
```
{% endcode %}

Nuclei + Katana

{% code overflow="wrap" %}
```bash
katana -u http://testphp.vulnweb.com/ -headless -js-crawl -jsluice -ef png,css,jpg -silent | qsreplace -a | nuclei -t dast/vulnerabilities -dast -silent
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiiqti-EEd7fS3ENngTk7wjCqb5jR504HRxBXwZylS1F-MQeuiMfhVrvrshtaB2kiLQv4yVrb23XzJYA6M3dK-kjgSmfe8bl4HazfMaP_mRta63ESKC32CJ6wGkMBxJBTlhVE17gGWxRMix6QAPYf6RStt_Rl8Kj9HNeUwolwfDBFYQCO9c0soBhp3n5qQ/s800" alt=""><figcaption><p>Nuclei + Katana</p></figcaption></figure>

{% hint style="info" %}
**Note**: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at [yuyudhn@gmail.com](mailto:yuyudhn@gmail.com).
{% endhint %}
