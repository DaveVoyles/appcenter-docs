---
title: Get Started with Unity
description: Get started with Unity
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 10/13/2021
ms.topic: article
ms.assetid: 9d6f5d86-f53f-43d1-bbaf-a6f01a74bdeb
ms.tgt_pltfrm: unity
---

# Get Started with Unity

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [iOS Extensions](ios-extensions.md)
> * [React Native](react-native.md)
> * [Xamarin/MAUI](xamarin.md)
> * [UWP](uwp.md)
> * [WPF/WinForms](wpf-winforms.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [macOS Extensions](macos-extensions.md)
> * [tvOS](tvos.md)
> * [Cordova](cordova.md)

The App Center SDK uses a modular architecture so you can use any or all of the services.

Let's get started and set up the App Center Unity SDK to use App Center Analytics and Crashes. To add App Center Distribute to your app, see the [documentation for App Center Distribute](~/sdk/distribute/unity.md).

## 1. Prerequisites

Before you begin, make sure that your project is set up in Unity 2018.1 or later.

The App Center SDK for Unity supports the following platforms:

* iOS (9.0 or later)
* Android (5.0/API 21 or later)
* UWP (Build 16299 or later)

> [!NOTE]
> .NET 3.5 scripting runtime version and .NET scripting backend are no longer supported on the UWP platform.

To learn how to use coroutines/callbacks instead of `async/await` with `.NET 3.5` backend, refer to the [Asynchronous APIs in the Unity SDK](../../sdk/other-apis/unity.md) section of the documentation.

Also, the App Center SDK for Unity is only available in C#.

## 2. Create your app in the App Center Portal to acquire the App Secret

If you've already created your app in the App Center portal, you can skip this step.
1. Head over to [appcenter.ms](https://appcenter.ms).
2. Sign up or sign in to App Center.
3. Click the blue button on the top-right corner of the portal that says **Add new** and select **Add new app** from the dropdown menu.
4. Enter a name and an optional description for your app.
5. Select the appropriate OS and platform depending on your project as described above.
6. Click the **Add new app** button in the bottom-right of the page.

Once you've created an app, you can obtain its **App Secret** on the **Settings** page on the App Center Portal. At the top right hand corner of the **Settings** page, click on the **triple vertical dots** and select `Copy app secret` to get your App Secret.

## 3. Add the App Center SDK to your project

### 3a Install the Extension Editor

The App Center SDK is integrated by importing Unity Packages into your project. To do so, first install the [App Center Unity Editor Extensions](https://github.com/Microsoft/AppCenter-SDK-Unity-Extension) plugin. Open the Editor Extensions via the Unity menu: Window > App Center > Editor Extensions and select "Install App Center SDK".

> [!NOTE]
> In the Unity versions 2019.1.2f1 and newer, there could be errors in logs during the installation such as `Coroutine continue failure`. It's a known Unity issue and you should ignore it.

### 3.1b Install Individual Packages

As an alternative, each of the individual Unity packages can be downloaded and imported. They're found on the [App Center Unity SDK GitHub page](https://github.com/Microsoft/AppCenter-SDK-Unity/releases). Download the latest release of the package(s) that you want to use. Its name should be in the format **AppCenter{Analytics/Crashes/Distribute}-v{version}.unitypackage**.

### 3.2b Import the package

Open your Unity project, then double-click the package you downloaded. A pop-up window should appear in your Unity project containing a list of files. Select **Import**, and the SDK will be added to your project. Repeat this step for each package you downloaded and plan to use in your project.

## 4. Enable the SDK

### 4.1 Create an empty Game Object

App Center works as a component that you attach to a game object in the scene that your game launches into. Navigate to this scene and add an empty game object. Give it a descriptive name, such as "App Center".

### 4.2 Attach the App Center script

In the **Project** window, navigate to the "AppCenter" folder that was added to your project. Locate the script, named *AppCenterBehavior*, and drag it onto your newly created game object in the **Hierarchy** window.

> [!NOTE]
> You don't need to add App Center to every scene in which you wish to use it. Adding it to the first loaded scene is enough.

### 4.3 Configure App Center settings

Click on this new "App Center" object and add your app secrets to the corresponding fields in the **Inspector** window. Make sure to also check the "Use {service}" boxes for each App Center service you intend to use

![App Center script configuration](images/unity_configuration.png "App Center configuration")

[!INCLUDE [app secret warning](../includes/app-secret-warning.md)]

> [!NOTE]
> If your project doesn't support one of the three platforms listed in the settings, leave the app secret field as-is; it will have no effect. If your project supports platforms that App Center doesn't support, the APIs and configuration will have no effect for those platforms.

---
Great, you're all set to visualize Analytics and Diagnostics (Crash and Error) data in the portal that the SDK will automatically collect.

Look at the documentation for [App Center Analytics](~/sdk/analytics/unity.md) and [App Center Crashes](~/sdk/crashes/unity.md) to learn how to use more advanced functionality with both services.

To learn how to get started with in-app updates, read the documentation for [App Center Distribute](~/sdk/distribute/unity.md).

## 5. Backup rules (Android only)

> [!NOTE]
> Apps that target Android 6.0 (API level 23) or higher have Auto Backup automatically enabled.

> [!NOTE]
> If you already have a custom file with backup rule, switch to the third step.

> [!NOTE]
> If you don't already have your own **AndroidManifest.xml** file, create it in the **Assets/Plugins/Android** folder. This manifest will be merged with the default Unity-created one at the time of build.

If you use auto-backup to avoid getting incorrect information about device, follow the next steps:

### 5.1. For Android 11 (API level 30) or lower.

1. Create **appcenter_backup_rule.xml** file in the **Assets/Plugins/Android/res/xml** folder.

[!INCLUDE [android backup rules](includes/android-backup-rules-android.md)]

### 5.2. For Android 12 (API level 31) or higher.

1. Create **appcenter_backup_rule.xml** file in the **Assets/Plugins/Android/res/xml** folder.

[!INCLUDE [android backup rules](includes/android-backup-rules-android-12.md)]
  