---
layout: post
title: "Mobile Automation with Appium in .NET - Setup and writing first test"
subtitle: "Setup Appium and writing first test for iOS & Android"
date: '2021-08-29 17:06:15'
background: '/img/bg-about.jpg'
---

![Appium .NET](./AppiumDotnet.png)

## Install Android Studio and XCode
First Android Studio and Xcode need to be installed.

## Setup enviroments variable

### Mac
Add these to your `.zprofile` (or `.bashprofile` if you use bash).
```bash
export ANDROID_HOME=~/Library/Android/sdk
export ANDROID_SDK_ROOT=~/Library/Android/sdk
export ANDROID_AVD_HOME=~/.android/avd
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools

export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
export PATH=$PATH:$JAVA_HOME/bin
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```
Note above `JAVA_HOME` is my local machine default path, might be different minor version for you, but as long as it is `jkd.1.8.xxx` then it is ok.

### Windows
[WIP], make sure those paths are added in `Enviroment Variables`

## Install Appium

### Install Appium
```bash
npm install -g appium
```
### Install Appium doctor
```bash
npm install -g appium-doctor
```


## Create Appium Dotnet Client Project
```zsh
dotnet new classlib -o AppiumDotnetClient
```

### Add nuget packages
```zsh
dotnet add package Appium.WebDriver --version 4.3.1
```
```zsh
dotnet add package NUnit --version 3.13.2
```
```zsh
dotnet add package NUnit3TestAdapter --version 4.0.0
```

Alternative paste these in `.csproj` file, then run `dotnet restore`
```csharp
  <ItemGroup>
    <PackageReference Include="Appium.WebDriver" Version="4.3.1" />
    <PackageReference Include="NUnit" Version="3.13.2" />
    <PackageReference Include="NUnit3TestAdapter" Version="4.0.0" />
  </ItemGroup>
```

## Create first android test:
Create `FirstAndroidTest.cs`
<font size=2>
```csharp
using System;
using System.IO;
using NUnit.Framework;
using OpenQA.Selenium.Appium;
using OpenQA.Selenium.Appium.Android;
using OpenQA.Selenium.Appium.Enums;

namespace AppiumDotnetClient
{
    public class FirstAndroidTest
    {
        private AppiumDriver<AndroidElement> _driver;

        [SetUp]
        public void Setup()
        {
            var currentTestDirectory = Directory.GetParent(System.AppDomain.CurrentDomain.BaseDirectory)
                                            .Parent.Parent.Parent.FullName;

            var driverOptions = new AppiumOptions();
            driverOptions.AddAdditionalCapability(MobileCapabilityType.PlatformName, "Android"); 
            driverOptions.AddAdditionalCapability(MobileCapabilityType.AutomationName, "UIAutomator2"); //(1)
            driverOptions.AddAdditionalCapability(MobileCapabilityType.DeviceName, "Pixel 3a Droid10");
            driverOptions.AddAdditionalCapability("avd", "Pixel_3a_Droid10"); //(2)
            driverOptions.AddAdditionalCapability(MobileCapabilityType.App, $"{currentTestDirectory}/apps/DemoApp.apk"); //(3)

            driverOptions.AddAdditionalCapability("appWaitActivity", "*.MainActivity"); //(4)
            _driver = new AndroidDriver<AndroidElement>(new Uri("http://localhost:4723/wd/hub"), driverOptions); //(5)
        }

        [TearDown]
        public void TearDown()
        {
            _driver?.Quit();
        }

        [Test]
        public void LaunchApp()
        {
            Assert.Pass();
        }
    }
}
```
</font>

>**(1)** Appium supports both UIAutomator2 and Espresso for Android UI Automation test, we'll be using UIAutomator2 this time.  

>**(2)** `avd` capability allows android device to be launched when test running, choose which ever emulator you have available on your machine.  

>**(3)** This is the path to your app, in this sample the app was downloaded from [here](https://github.com/webdriverio/native-demo-app/releases) 

>**(4)** This allows to wait untill the `MainActivity` started before running our test.

>**(5)** This is the defaut address of appium server, default port is `4723`, different ports can be used for parallel run.


## Run test
### Start Appium Server
Open terminal and run
```bash
appium
```
Open another terminal and run
```bash
dotnet test
```

Test should now run successfully and we should see this in our Appium server output
```Node
[HTTP] <-- POST /wd/hub/session 200 5696 ms - 1059
```

The test app launch and our test run.

## Create iOS first test
Create `FirstIOSTest.cs`
<font size=2>
```csharp
using System;
using System.IO;
using NUnit.Framework;
using OpenQA.Selenium.Appium;
using OpenQA.Selenium.Appium.Enums;
using OpenQA.Selenium.Appium.iOS;

namespace AppiumDotnetClient
{
    public class FirstIOSTest
    {
        private AppiumDriver<IOSElement> _driver;

        [SetUp]
        public void Setup()
        {
            var currentTestDirectory = Directory.GetParent(System.AppDomain.CurrentDomain.BaseDirectory)
                                            .Parent.Parent.Parent.FullName;

            var driverOptions = new AppiumOptions();
            driverOptions.AddAdditionalCapability(MobileCapabilityType.PlatformName, "iOS");
            driverOptions.AddAdditionalCapability(MobileCapabilityType.AutomationName, "XCUITest"); //(1) 
            driverOptions.AddAdditionalCapability(MobileCapabilityType.DeviceName, "iPhone 12"); // (2) 
            driverOptions.AddAdditionalCapability(MobileCapabilityType.PlatformVersion, "14.5"); // (2) 
            driverOptions.AddAdditionalCapability(MobileCapabilityType.App, $"{currentTestDirectory}/apps/DemoApp.app.zip");
            //(3)
            _driver = new IOSDriver<IOSElement>(new Uri("http://localhost:4723/wd/hub"), driverOptions);
        }

        [TearDown]
        public void TearDown()
        {
            _driver?.Quit();
        }

        [Test]
        public void LaunchApp()
        {
            Assert.Pass();
        }
    }
}

```
</font>

> **(1)** XCUITest is used for testing iOS	

> **(2)** this can be check via Xcode to see what simulator & OS version you have available, (run `xcrun simctl list`)

> **(3)**  No capability is required to start an ios simulator because appium will do it for us

Repeat above step to run test, test should now run successfully.