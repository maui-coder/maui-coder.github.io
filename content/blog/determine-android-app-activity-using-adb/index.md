---
layout: post
title: "Determine android app activity name using adb"
date: '2021-09-04 00:30:15'
---

![Android Debug Bridge](./AndroidDebugBridgeImage.png)

When setup Appium driver options for Android, we need to specify app activity. Something like this:

<font size=2>

```csharp
_driverOptions.AddAdditionalCapability("appActivity", "*.MainActivity");
```
</font>

Or if app has a `SplashActivity` then we need to wait for the `MainActivity`

<font size=2>

```csharp
_driverOptions.AddAdditionalCapability("appWaitActivity", "*.MainActivity");
```
</font>

It is quite common for app main activity to be `com.example.app.mainActivity`. However, this is not always the case. Sometimes, main activity can be named differently.


One example of this is apps that built using xamarin, by defaut, if no activity name is specified by developer. The main activity will be appended with a hash to help avoiding namespace conflict. More details can be read [here](https://docs.microsoft.com/en-au/xamarin/android/platform/android-manifest).

Without the correct activity name specified, Appium will throw this error:
```bash
 Error: Error occured while starting App. Original error: Activity used to start app doesn't exist or cannot be launched! Make sure it exists and is a launchable activit
```

<br/>

## Determine running activity using `adb`

### Install adb
On Mac 
```bash
brew install android-platform-tools
```


### Find activity name using `adb`

First, determine device id using `adb devices`
```bash
adb devices

Output:

List of devices attached
e1418fa6	device
```

Next, open device shell
```bash
 adb -s e1418fa6 shell
```

Launch the app we want to find activity name

Then run this command
```bash
dumpsys window windows | grep -E 'mCurrentFocus'    
```

This will output the name of the current focus activity.  

Example output:
<font size=2>

```bash
OnePlus3T:/ $ dumpsys window windows | grep -E 'mCurrentFocus' 
  mCurrentFocus=Window{844329b u0 com.whatsapp/com.whatsapp.registration.EULA}
OnePlus3T:/ $ 
```
</font>



