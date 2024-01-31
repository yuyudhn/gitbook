---
description: List all Objection commands used for iOS mobile app penetration testing.
---

# Objection

### Explore App with Objection

{% code overflow="wrap" %}
```bash
objection -g com.example.app explore # com.example.app is example apps
```
{% endcode %}

### Run Objection Command at Spawn Apps

{% code overflow="wrap" %}
```bash
# Example command to run "ios hooking search classes jail" on spawn apps
objection -g com.example.app "ios hooking search classes jail"
```
{% endcode %}

### Basic Jailbreak Detection Bypass

```bash
ios jailbreak disable

# Example output:
com.example.app on (iPhone: 15.4.1) [usb] # 
(agent) [303462] fileExistsAtPath: check for /Applications/Cydia.app failed with: 0x0, marking it as successful.
(agent) [289052] fileExistsAtPath: check for /Applications/Cydia.app was successful with: 0x1, marking it as failed.
(agent) [289052] fileExistsAtPath: check for /bin/bash was successful with: 0x1, marking it as failed.
(agent) [289052] fopen: check for /bin/bash was successful with: 0x103404f98, marking it as failed.
..................
```

## Basic Enumeration

### Local App Paths

```bash
env

# Example output:
Name               Path
-----------------  -------------------------------------------------------------------------------------------
BundlePath         /private/var/containers/Bundle/Application/81AD95F9-3DA4-4CEB-BD50-442BD55D1D02/Example.app
CachesDirectory    /var/mobile/Containers/Data/Application/9EC4057E-FE48-4B9F-81D3-C0FB75BC2EA3/Library/Caches
DocumentDirectory  /var/mobile/Containers/Data/Application/9EC4057E-FE48-4B9F-81D3-C0FB75BC2EA3/Documents
..........
```

### List bundles of the application

```bash
ios bundles list_bundles

# Example output:
com.example.app on (iPhone: 15.4.1) [usb] # ios bundles list_bundles
Executable              Bundle                            Version   Path
----------------------  --------------------------------  --------  -------------------------------------------
AGXMetalA11             com.apple.AGXMetalA11             190.17.2  ...em/Library/Extensions/AGXMetalA11.bundle
Runner                  com.example.app                   11.4.61   ...E-38A1-4FC0-AE77-0B2D26E7BF67/Runner.app
```

### List framework used by the application

```bash
ios bundles list_frameworks

# Example output:
Executable                      Bundle                                        Version    Path
------------------------------  --------------------------------------------  ---------  -------------------------------------------
share_plus                      org.cocoapods.share-plus                      0.0.1      ...nner.app/Frameworks/share_plus.framework
webview_flutter_wkwebview       org.cocoapods.webview-flutter-wkwebview       0.0.1      ...orks/webview_flutter_wkwebview.framework
DTTJailbreakDetection           org.cocoapods.DTTJailbreakDetection           0.4.0      ...ameworks/DTTJailbreakDetection.framework
```

## Basic Hooking

### List all classes

```bash
ios hooking list classes

# Example output:
com.example.app on (iPhone: 15.4.1) [usb] # ios hooking list classes
AAAFoundationSwift.AAFTimedAnalyticsEvent
AAAFoundationSwift.BroadcastMessageSender
AAAFoundationSwift.DependencyRegistry
AAAFoundationSwift.MessageSender
AAAFoundationSwift.OSActivity
AAAFoundationSwift.OSTransaction
AAAFoundationSwift.WeakWrapper
.................
```

### Search for classes

{% code overflow="wrap" %}
```bash
# Search a class that contains a string
ios hooking search classes jailbreak

# Example output:
com.example.app on (iPhone: 15.4.1) [usb] # ios hooking search classes jailbreak
PodsDummy_DTTJailbreakDetection
DTTJailbreakDetection
PodsDummy_flutter_jailbreak_detection
flutter_jailbreak_detection.SwiftFlutterJailbreakDetectionPlugin
FlutterJailbreakDetectionPlugin
..............
```
{% endcode %}

### Search for methods

{% code overflow="wrap" %}
```bash
# Search a method that contains a string
ios hooking search methods jail

# Example output:
com.example.app on (iPhone: 15.4.1) [usb] # ios hooking search methods jail
[DTTJailbreakDetection + isJailbroken]
[UIScreen + _shouldDisableJail]
[UIScreen - _unjailedReferenceBoundsForInterfaceOrientation:]
[UIScreen - _unjailedReferenceBoundsInPixels]
..........
```
{% endcode %}

### List class methods

```bash
# List methods of a specific class
ios hooking list class_methods DTTJailbreakDetection

# Example output:
com.example.app on (iPhone: 15.4.1) [usb]
+ isJailbroken

Found 1 methods
```

### Watch class

{% code overflow="wrap" %}
```bash
# Hook all the methods of a class, dump all the initial parameters and returns
ios hooking watch class DTTJailbreakDetection

# Hook an specific method of a class dumping the parameters, backtraces and returns of the method each time it's called
ios hooking watch method "*[iRoot isJailBroken]" --dump-args --dump-return --dump-backtrace
```
{% endcode %}

### Overwrite Return Value

{% code overflow="wrap" %}
```bash
ios hooking set return_value "*[iRoot isJailBroken]" false
```
{% endcode %}

### Generate Hooking Template

```
ios hooking generate simple iRoot
```

## Extract Sensitive Information

### Dump NSUserDefaults

NSUserDefaults is a simple storage mechanism commonly used for storing small amounts of data or user preferences. However, it's not a secure place to store sensitive information like passwords because it can be accessed easily by other apps or by jailbroken devices.

```bash
ios nsuserdefaults get

# Example output:
..........
"flutter.isStartStopGps" = 0;
    "flutter.isUploadResolution" = 0;
    "flutter.logACallObjectType" = 1;
    "flutter.mPin" = 1337;
    "flutter.mPinTime" = 7;
    "flutter.userEmail" = "redacted@redacted.com";
    "flutter.userId" = 1337;
.............
```

### Dump Keychain Data

Extracts the keychain items for the current application.

```bash
ios keychain dump
```

{% hint style="info" %}
**Note**: This page is incomplete and will be regularly updated. If you have any ideas or resources that need to be added, please contact me at [yuyudhn@gmail.com](mailto:yuyudhn@gmail.com).
{% endhint %}
