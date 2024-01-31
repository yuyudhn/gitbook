---
description: iOS Pentest Checklist
---

# iOS Penetration Testing

## Jailbreak

### palera1n

{% code overflow="wrap" %}
```bash
wget https://github.com/palera1n/palera1n/releases/download/v2.0.0-beta.5/palera1n-linux-x86_64
chmod +x palera1n-linux-x86_64
sudo mv palera1n-linux-x86_64 /usr/local/bin/palera1n
sudo chown root: /usr/local/bin/palera1n

# Rootful Jailbreak
sudo palera1n -fc

# After iPhone restart, run this command
sudo palera1n -f
```
{% endcode %}

### checkra1n

{% code overflow="wrap" %}
```bash
wget -O - https://assets.checkra.in/debian/archive.key | gpg --dearmor | sudo tee /usr/share/keyrings/checkra1n.gpg >/dev/null
echo 'deb [signed-by=/usr/share/keyrings/checkra1n.gpg] https://assets.checkra.in/debian /' | sudo tee /etc/apt/sources.list.d/checkra1n.list
sudo apt-get update
sudo apt-get install checkra1n

# jb
sudo checkra1n
```
{% endcode %}

## Repository

Here is a list of repositories that need to be added to Cydia or Sileo after iOS is jailbroken. Not all repositories need to be added; it depends on your needs.

* [https://cydia.akemi.ai/](https://cydia.akemi.ai/) - **Karen's** Repository
* [https://julioverne.github.io/](https://julioverne.github.io/) - **julioverne's** Repository
* [https://build.frida.re/](https://build.frida.re/) - **Frida** Repository
* [http://apt.thebigboss.org/repofiles/cydia/](http://apt.thebigboss.org/repofiles/cydia/) - **TheBigBoss** Repository
* [https://uckermark.github.io/repo/](https://uckermark.github.io/repo/) - **Uckermark's** Repository
* [https://repo.co.kr/](https://repo.co.kr/) - **Merona** Repository
* [https://ios.jjolano.me/](https://ios.jjolano.me/) - **jjolano**'s Repository
* [https://havoc.app/](https://havoc.app/) - **Havoc** Repository
* [https://ryleyangus.com/repo/](https://ryleyangus.com/repo/) - **Ryley Angus** Repository
* [https://repo.misty.moe/apt/](https://repo.misty.moe/apt/) - **Misty's** Repository

## Applications

Here is a list of apps or tools that need to be installed after iOS is jailbroken.

### For Jailbreak Detection Bypass

| Apps             | Repo Source     |
| ---------------- | --------------- |
| **Shadow**       | **jjolano**     |
| **A-Bypass**     | **Merona**      |
| **HideJB**       | **BigBoss**     |
| **Not a bypass** | **Uckermark**   |
| **Hestia**       | **Havoc**       |
| **Liberty Lite** | **Ryley Angus** |

{% hint style="warning" %}
There's no need to enable all above tools when performing a jailbreak bypass. For instance, some apps can be bypassed using Hestia, while others can only be bypassed using Shadow, and so on.
{% endhint %}

### SSL Pinning Bypass

| Apps              | Repo Source               |
| ----------------- | ------------------------- |
| SSL Kill Switch 3 | **Misty's** Repository    |
| SSL Kill Switch 2 | **julioverne** repository |

{% hint style="danger" %}
**Note**: You can only choose between SSL Killswitch 2 or 3; you can't install all apps together.
{% endhint %}

### Utilities

<table data-full-width="false"><thead><tr><th>App Name</th><th>Description</th></tr></thead><tbody><tr><td><strong>appinst</strong></td><td>Used to install .ipa files via the terminal. Can be installed through the <strong>akemi</strong> repository.</td></tr><tr><td><strong>AppSync Unified</strong></td><td>A tweak that allows users to install ad-hoc signed, fakesigned, or unsigned IPAs. Can be installed via the <strong>akemi</strong> repository. </td></tr><tr><td><strong>openssh</strong></td><td>Used to connect to an iPhone device via the SSH protocol. It should be installed when jailbreaking using checkra1n or palera1n. The default SSH credentials are username: <strong>root</strong> and password: <strong>alpine</strong>. We recommend changing the default password immediately. </td></tr><tr><td><strong>Filza File Manager</strong></td><td>A GUI application used to explore the internal system and install .IPA files. Can be installed through the <strong>BigBoss</strong> repository or the palera1n repository for its 64-bit version.</td></tr><tr><td><strong>frida (server)</strong></td><td>Used for hooking and monitoring API calls. In some cases, if <strong>SSL Kill Switch</strong> fails to bypass SSL Pinning in an application, a custom script may be required, necessitating the installation of the frida server on the iPhone device.</td></tr></tbody></table>

## Tools

These tools are utilized for conducting penetration testing on iOS applications.

* **Frida -** Dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers. Learn more at frida.re. **Install**: `pip3 install frida-tools`.
* **Objection -** Runtime mobile exploration toolkit, powered by Frida, built to help you assess the security posture of your mobile applications, without needing a jailbreak. **Install**: `pip3 install objection`.
* [**IPAtool**](https://github.com/majd/ipatool) **-** Command-line tool that allows searching and downloading app packages (known as ipa files) from the iOS App Store
* [**Grapefruit**](https://github.com/ChiChou/grapefruit) **-** Runtime Application Instruments for iOS. Install: `npm install -g igf`.
* [**frida-ios-dump**](https://github.com/AloneMonkey/frida-ios-dump) **-** pull decrypted ipa from jailbreak device
* [**MobSF**](https://github.com/MobSF/Mobile-Security-Framework-MobSF) **-** Mobile Security Framework (MobSF) is an automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis and security assessment framework capable of performing static and dynamic analysis.
* [**reFlutter**](https://github.com/Impact-I/reFlutter) **-** Flutter Reverse Engineering Framework

### Frida Scripts

This is a list of Frida scripts I have tested and that worked to bypass root detection or SSL pinning on iOS apps.

* [Jailbreak/Root Detection Bypass in Flutter by CyberCX-STA](https://github.com/CyberCX-STA/flutter-jailbreak-root-detection-bypass)
* [A Frida script that disables Flutter's TLS verification by @TheDauntless](https://codeshare.frida.re/@TheDauntless/disable-flutter-tls-v1/)
* [Bypass Flutter SSL Pinning by @Zionspike](https://codeshare.frida.re/@zionspike/bypass-flutter-pinning-ios/)
* [iOS Jailbreak Detection Bypass from by @@liangxiaoyi1024](https://codeshare.frida.re/@liangxiaoyi1024/ios-jailbreak-detection-bypass/)

{% hint style="info" %}
**Note**: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at [yuyudhn@gmail.com](mailto:yuyudhn@gmail.com).
{% endhint %}
