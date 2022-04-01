---
title: Get Started with Apache Cordova
description: Get Started (Cordova)
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 11/10/2020
ms.topic: article
ms.assetid: 9DBB5B10-4F1F-4A93-9797-BC2ECAE62903
ms.tgt_pltfrm: cordova
---

# Get Started with Apache Cordova

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [iOS Extensions](ios-extensions.md)
> * [React Native](react-native.md)
> * [Xamarin](xamarin.md)
> * [UWP](uwp.md)
> * [WPF/WinForms](wpf-winforms.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [macOS Extensions](macos-extensions.md)
> * [tvOS](tvos.md)
> * [Cordova](cordova.md)

> [!NOTE] 
> Support for Cordova Apps has ended in April 2022. Find more information in the [App Center blog](https://devblogs.microsoft.com/appcenter/announcing-apache-cordova-retirement/).

The App Center SDK uses a modular architecture so you can easily use one, several, or all of the App Center services in your Apache Cordova application.

In this article, you'll learn how to add the App Center SDK to your Apache Cordova applications, then configure App Center Analytics and App Center Crashes capabilities in an application.

## 1. Prerequisites

Before you begin, ensure that your Apache Cordova application project meets the following minimum requirements:

* Cordova CLI 6.4.0 or later
* `cordova-android` engine 5.0.0 or later
* `cordova-ios` engine 4.3.0 or later

To determine your Cordova CLI version, open a terminal window or Windows command prompt and execute the following command:

```shell
cordova -v
```

To determine the `cordova-android` and `cordova-ios` versions, open the project's **config.xml** file, you'll find the platform engines defined in the `engine` elements shown below:

```xml
<engine name="android" spec="~6.2.3" />
<engine name="ios" spec="~4.4.0" />
```

You also must have [CocoaPods](https://cocoapods.org) installed. For this, run
`sudo gem install cocoapods`
`pod setup`

## 2. Getting the App Center App Secret

In order for the App Center SDK to connect with your application project in App Center, the SDK needs your App Center project's **App Secret**. You'll learn later how to configure the App Center Apache Cordova SDK with this value, but for now, lets see where to find it. 

If you have an existing App Center application project, access the [App Center Dashboard](https://appcenter.ms), and open your project. You can also the app secret on the application project's **Settings** page. Access the menu in the page's upper right corner, then select **Copy app secret** to copy the app secret to the clipboard.

If you don't have an application created in the App Center Dashboard, complete the following steps.

1. Register for an App Center account, or Login to an existing App Center account at [https://appcenter.ms](https://appcenter.ms).
2. In the App Center Dashboard, click the **Add new** drop-down in the upper-right corner of the page. Select **Add new app** from the menu.
3. Enter a name for your application project, adding an optional description as needed.
4. Select the appropriate OS for your application project (Android or iOS only), then select the **Cordova** platform option.
5. Click the **Add new app** button on the bottom-right corner of the page.
6. Locate the app secret on the application project's **Settings** page. Access the menu in the page's upper right corner, then select the **Copy app secret** item to copy the app secret to the clipboard.

## 3. Add the App Center SDK to the project

### Installing the SDK

For Apache Cordova projects, the SDK is distributed through standard Apache Cordova plugins. Add the App Center SDK to your Cordova project using the instructions provided in this section. Start by opening a Terminal window or Windows command prompt, then navigate to your Cordova project's root folder.

To add support for **App Center Analytics** to your project, execute the following command:

```shell 
cordova plugin add cordova-plugin-appcenter-analytics
```

To add support for **App Center Crashes** to your project, execute the following command:

```shell 
cordova plugin add cordova-plugin-appcenter-crashes
```

To add plugins for all of the App Center capabilities to your project with one command, execute the following command:

```shell 
cordova plugin add cordova-plugin-appcenter-analytics cordova-plugin-appcenter-crashes
```

> [!NOTE]
> You should add **cordova-plugin-appcenter-analytics** and **cordova-plugin-appcenter-crashes** most every app that uses App Center; they provide useful information for developers without any additional setup or coding.

### Configuring the plugins

#### App Secret

You must configure the project with your App Center project app secret before you can use the App Center SDK in your Apache Cordova project.

Open the Apache Cordova project's **config.xml** file; for each of your Apache Cordova project's target `platform` elements (only Android and iOS today), add a child `preference` element in the following format:

```xml
<preference name="APP_SECRET" value="0000-0000-0000-0000-000000000000" />
```
In this example, you're adding an element with a `name` attribute with a value of `APP_SECRET`, and a `value` attribute with the value shown in the example. Here, the `0000-0000-0000-0000-000000000000` shown in the example is an arbitrary representation of a App Center project app secret. Replace the app secret shown in the example with the actual app secret for your App Center project.

As a complete example, for a Apache Cordova project that supports both Android and iOS targets, you'll have separate app project definitions in App Center, and so, different app secret values for each target platform. The relevant section of the project's **config.xml** file will look like the following:

```xml
<platform name="android">
   <preference name="APP_SECRET" value="0000-0000-0000-0000-000000000001" />
</platform>
<platform name="ios">
   <preference name="APP_SECRET" value="0000-0000-0000-0000-000000000002" />
</platform>
```
> [!Note]
> The example doesn't show actual app secret values; these are merely mock-ups, you'll need to grab your App Center project's app secrets and use them here. The app secrets are different because you'll have unique secrets for your Android and iOS application projects in App Center.

#### Analytics Preferences

If you're using App Center Analytics in your app, there's some additional configuration steps to perform. Open the Apache Cordova project's **config.xml** file in an editor, and add one or more of the following `preferences` elements to the file:

- `APPCENTER_ANALYTICS_ENABLE_IN_JS` - *(optional, default is false)* Controls whether Analytics is enabled automatically. When `false`, Analytics is enabled by default. When `true`, the application must call `AppCenter.Analytics.setEnabled(true)` (in its JavaScript code) to enable Analytics before sending data to App Center. This preference is useful when you want to ask users whether they want to share analytics information before enabling it within the application. [Learn more about sending user events manually.](~/sdk/analytics/cordova.md#wait-for-js-to-enable-app-center-analytics)

  Example:

  ```xml
  <preference name="APPCENTER_ANALYTICS_ENABLE_IN_JS" value="true" />
  ```

- `APPCENTER_CRASHES_ALWAYS_SEND` - *(optional, default is true)* Specifies whether crash reports are automatically sent to App Center when the app crashes. When sending crash reports automatically, every crash is reported, potentially overwhelming the engineering and support teams. When sending crash reports manually (setting `APPCENTER_CRASHES_ALWAYS_SEND` to `false`), code within your application can triage crashes, and decide when crash reports are sent to App Center, what data is included in the reports, and so on. This means more work for the developer, but it also provides greater control over user privacy and allows you to attach a message with crash report. [Learn more about processing on crash reports in JS](~/sdk/crashes/cordova.md).

  Example:

  ```xml
  <preference name="APPCENTER_CRASHES_ALWAYS_SEND" value="false" />
  ```
  
  #### If you use auto-backup to avoid getting incorrect information about devices, follow the next steps:

> [!NOTE]
> Apps that target Android 6.0 (API level 23) or higher have Auto Backup automatically enabled. 

> [!NOTE]
> If you already have a custom file with backup rule, switch to the third step.

  a. Create **appcenter_backup_rule.xml** file in the **res/xml** folder.

   ```xml
   <resource-file src="appcenter_backup_rule.xml" target="res/xml/appcenter_backup_rule.xml" />
   ```

  b. In order for `android:fullBackupContent` attribute to be added to the `<application>` element inside **AndroidManifest.xml** file, add the following lines to the app's **config.xml** file:

   ```xml
   <edit-config file="app/src/main/AndroidManifest.xml" mode="merge" target="/manifest/application">
        <application android:fullBackupContent="@xml/appcenter_backup_rule" />
   </edit-config>
    ```

  c. Add the following backup rules to the **appcenter_backup_rule.xml** file:

  ```xml
  <full-backup-content xmlns:tools="http://schemas.android.com/tools">
      <exclude domain="sharedpref" path="AppCenter.xml"/>
      <exclude domain="database" path="com.microsoft.appcenter.persistence"/>
      <exclude domain="database" path="com.microsoft.appcenter.persistence-journal"/>
      <exclude domain="file" path="error" tools:ignore="FullBackupContent"/>
      <exclude domain="file" path="appcenter" tools:ignore="FullBackupContent"/>
  </full-backup-content>
  ```

  ## 4. Where to go next?

Great, you're all set to visualize Analytics and Crashes data collected automatically by the SDK on the portal. There isn't any additional setup required. Look at [Analytics](~/sdk/analytics/cordova.md) and [Crashes](~/sdk/crashes/cordova.md) sections for APIs guides and walkthroughs to learn what App Center can do.
