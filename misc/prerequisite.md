---
description: Some things that need to be prepared to play Hack The Box machines.
---

# Prerequisite

Before starting the work on HTB machines, always add the IP address to /etc/hosts on our machine. Example:

```
nino@nakano:~$ cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       nakano
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

#HackTheBox
10.10.11.105    horizontall.htb api-prod.horizontall.htb
.....

```

### Common tools

Here are some commonly used tools for working on HackTheBox machines.

* [**nmap**](https://github.com/nmap/nmap) - for port scanning
* [**ffuf**](https://github.com/ffuf/ffuf) - directory scanning & vhost discovery

[Ffuf](https://www.linuxsec.org/2020/08/web-fuzzing-menggunakan-ffuf.html) can be used for performing directory scanning as well as vhost bruteforce (subdomain enumeration). That's why I prefer using Ffuf over other tools because this single tool can be used for multiple purposes.

* [**SecLists**](https://github.com/danielmiessler/SecLists) - Wordlist

Seclists is a collection of wordlists that will be very useful when performing fuzzing.

### Reverse Shell

* [https://reverse-shell.sh](https://reverse-shell.sh/)
* [https://www.revshells.com](https://www.revshells.com/)

### TTY Shell

"Magic trick" for achieving a stable Full TTY shell (works on bash):&#x20;

```
user@remote-server:~$ python3 -c "import pty; pty.spawn('/bin/bash')" 
```

Then press **CTRL+Z** to pause the shell process. Next, execute the following command to disable input buffering and echo, making the reverse shell more responsive.

```bash
user@local:~$ stty raw -echo 
```

After that, run the following command to bring the shell process to the foreground.

```bash
user@local:~$ fg
```

Lastly, execute the following command to set the TERM environment variable to xterm, ensuring smooth operation of the interactive shell.

```bash
user@remote-server:~$ export TERM=xterm
```

* [Mendapatkan Akses TTY Shell setelah Back Connect](https://www.linuxsec.org/2019/07/spawn-tty-shell.html)

## Useful Resources

* [exploitdb-bin-sploits // Exploit-Database's pre-compiled binary exploits](https://gitlab.com/exploit-database/exploitdb-bin-sploits)
* [Ippsec](https://ippsec.rocks/?)
* [kashz jewels](https://kashz.gitbook.io/kashz-jewels/)
* [HackTricks](https://book.hacktricks.xyz/)

Alright, that's it for this update. I'll provide more updates later.
