# Table of contents

* [About](README.md)

## 🚉 QUICKSTART

* [Prerequisite](quickstart/prerequisite.md)
* [Reconnaissance](quickstart/reconnaissance.md)
* [Exploitation](quickstart/exploitation.md)
* [Post Exploitation](quickstart/post-exploitation.md)
* [⛈️ Misc](quickstart/misc.md)

## 🪟 Active Directory

* [Basic Command](active-directory/basic-command.md)
* [Enumeration](active-directory/enumeration/README.md)
  * [PowerView](active-directory/enumeration/powerview.md)
* [Service Exploitation](active-directory/service/README.md)
  * [LDAP](active-directory/service/ldap.md)
  * [SMB](active-directory/service/smb/README.md)
    * [MS17-010](active-directory/service/smb/ms17-010.md)
  * [MSSQL](active-directory/service/mssql.md)
* [Privilege Escalation](active-directory/privilege-escalation/README.md)
  * [Unquoted Service Path](active-directory/privilege-escalation/unquoted-service-path.md)
  * [UAC Bypass](active-directory/privilege-escalation/uac-bypass.md)
  * [Token Abuse](active-directory/privilege-escalation/token-abuse.md)
* [Post Exploitation](active-directory/post-exploitation/README.md)
  * [Tunneling with Ligolo-ng](active-directory/post-exploitation/tunneling-with-ligolo-ng.md)
* [Credential Hunting](active-directory/credential-hunting/README.md)
  * [Group Policy Preferences](active-directory/credential-hunting/group-policy-preferences.md)
  * [DPAPI](active-directory/credential-hunting/dpapi.md)

## MITRE ATT\&CK <a href="#mitre" id="mitre"></a>

* [Defense Evasion](mitre/defense-evasion/README.md)
  * [Physical Attack: Remove EDR](mitre/defense-evasion/physical-attack-remove-edr.md)
  * [AMSI Bypass](mitre/defense-evasion/amsi-bypass.md)
* [Credential Access](mitre/credential-access/README.md)
  * [Dump SAM Hashes via Registry](mitre/credential-access/dump-sam-hashes-via-registry.md)

## 🐧 Linux

* [Misc](linux/misc.md)
* [Linux Post Exploitation](linux/linux-post-exploitation.md)
* [Linux Password Hunting](linux/linux-password-hunting.md)

## 🐚 Backdoor Stuff

* [Simple PHP Webshell](backdoor-stuff/php-webshell.md)
* [MSFvenom Generate Payload](backdoor-stuff/generate-payload.md)

## 📳 Mobile Pentest: iOS <a href="#ios" id="ios"></a>

* [iOS Penetration Testing](ios/ios-checklist.md)
* [Objection](ios/objection.md)

## 🕸️ Web Application

* [Common Applications](web-application/common-applications/README.md)
  * [Tomcat](web-application/common-applications/tomcat.md)
  * [Joomla](web-application/common-applications/joomla.md)
* [SSTI](web-application/ssti.md)
* [File Inclusion](web-application/file-inclusion.md)
* [XSS](web-application/xss.md)
* [Misc](web-application/misc.md)

## 🖊️ Machine Writeup

* [HackTheBox](machine-writeup/htb/README.md)
  * [Perfection](https://www.linuxsec.org/2024/06/hackthebox-perfection-writeup.html)
  * [Pilgrimage](machine-writeup/htb/pilgrimage.md)
  * [PC](machine-writeup/htb/pc.md)
  * [Shoppy](machine-writeup/htb/shoppy.md)
  * [GoodGames](machine-writeup/htb/goodgames.md)
  * [Photobomb](machine-writeup/htb/photobomb.md)
  * [Support](machine-writeup/htb/support.md)
