---
layout: post
title: "Locate Android & iOS UI elements using Appium Desktop"
date: '2021-09-14 00:30:15'
---

![Appium Desktop image](./AppiumDesktop.png)

## Download Appium Desktop
[Appium Desktop Apps](https://github.com/appium/appium-desktop/releases/) 

## Run appium desktop
Ensure `ANDROID_HOME` and `JAVA_HOME` are set in appium desktop. To do this, hit `Edit Configuration` button, and add path to `ANDROID_HOME` and `JAVA_HOME`

![Appium Desktop Enviroment Variable](./AppiumDesktopEnvironmentVariables.png)

`4723` is the default port that being use by Appium. When using Appium Desktop, we could change to different port to avoid conflict.

![Appium Desktop Port](AppiumDesktopPort.png)

Hit `Start Server` button to start an instant of appium on port 4725 in this example.	


Now that we got appium server running. We can start inspecting connected device using `Start Inspector Session` button

![Start Inspector Session](./StartInspectorButton.png)

Using Desired Capabilies, we can specify which platform and device we want to connected too, minimum parameter is `platformName`.

Android:
![Android Desired Capability](./AndroidDesiredCapbility.png)

iOS:
![iOS Desired Capability](./iOSDesireCapability.png)

These capbilies can be saved by `Save As...` button

![Save Capability](./SavedCapability.png)

Hit `Start Session` button and mobile UI elements should be parsed as XML tree on Appium Desktop

![Appium Desktop UI inspector](./AppiumDesktopUIInspector.png)

**Note**: any UI changes (eg: you open another app or move to different tab within the app) from device screen will not be automatic updated from Appium desktop. Refresh button will need to be use when UI changes from the device happened from outside Appium Desktop.


Manual query for element can be use by tap `Search For Element` button
![Search for element](./SearchForElement.png)

![Accessibility Card Search](./AccessibilityCardSearch.png)

Select element foung will be highlighted in the UI
![Select elemetn from Search](./SelectElementFromSearch.png)


These are just some of the main features of Appium Desktop that I use, there are test recorder, tap, swip, element attribute and much more... 