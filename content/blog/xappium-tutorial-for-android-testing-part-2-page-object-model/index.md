---
layout: post
title: "Xappium UI Test tutorial for mobile automation - Android Part 2 - Page object model"
subtitle: "Xappium UI Test tutorial for mobile automation - Android Part 2 - Page object model"
date: '2021-09-16 06:23:15'
background: '/img/bg-about.jpg'
---

<img src="https://xappium.com/images/xappium-bot.png" alt="img" style="zoom:50%;" />

<br/>

Checkout [Xappium docs on Page Object Model](https://xappium.com/docs/page-object-model.html) pattern.


## Inspect mobile app with appium desktop
(see [Appium Desktop](https://mauiautomation.com/appium-desktop-locate-element/) overview post if this is your first time using it)

Start Appium Desktop on port `4725`
Start an inspector session with the following capabilities:

![Appium Desktop UI inspector session](./AppiumDesktopUIInspectorSession.png)



Install [wdio demo app](https://github.com/webdriverio/native-demo-app/releases) on the device if it is not there. Android emulator installation can be done by drag and drop the `.apk` file to the emulator.

<img src="./wdioApp.png" alt="wdioApp" style="zoom:50%;" />

Start the app.

On Appium UI Inspector session Hit `Start Session` to connect to the device.

![image-20210916064500011](./InspectorSessionConnectToApp.png)



`@content-desc` is the Accessibily ID in Android. AccessibilyID can be added when buiding app UI b adding `AutomationId` in Xamarin.Forms or `testId` in React Native.

  ![Search for element home](./SearchForElementHome.png)

This can be confirm by searching for element with AccessibilityID Home. And we found one element, which is the Home tab button.



## Writing HomePage object

Add `HomePage.cs` to the `XappiumAndroidTest` folder.

