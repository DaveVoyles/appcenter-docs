---
title: Get Started with macOS Extensions
description: Get started with macOS extensions
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 10/22/2020
ms.topic: article
ms.assetid: bf7b3627-3ee0-41a8-9023-98a08a3ad4c1
ms.tgt_pltfrm: macos
dev_langs:  
 - swift
 - objc
---

# Get Started with macOS extensions
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

Extensions only support crash reporting.

To learn how to add App Center SDK to your container app, refer to the documentation for [App Center macOS Getting Started](./macos.md).

Let's get started with setting up App Center macOS SDK in your app extension to use App Center Crashes.

## 1. Prerequisites

The following requirements must be met to use App Center SDK:

* Your macOS project is set up in Xcode 11 or later on macOS version 10.14.4 or later.
* You're targeting devices running on macOS 10.13 or later.
* You're not using any other library that provides Crash Reporting functionality.

## 2. Create your app in the App Center Portal to obtain the App Secret

If you've already created your app in the App Center portal, you can skip this step.

1. Head over to [appcenter.ms](https://appcenter.ms).
2. Sign up or log in and hit the blue button on the top right corner of the portal that says **Add new** and select **Add new app** from the dropdown menu.
3. Enter a name and an optional description for your app.
4. Select **macOS** as the OS and **Objective-C/Swift** as a platform.
5. Hit the button at the bottom right that says **Add new app**.

Once you've created an app, you can obtain its **App Secret** on the **Settings** page on the App Center Portal. At the top right hand corner of the **Settings** page, click on the **triple vertical dots** and select `Copy app secret` to get your App Secret.

> [!NOTE]
> Using an existing App Secret from another macOS project has side effects. App Center doesn't support filtering by project on the same App Center application. For instance listing crashes for a particular version will list all the crashes from both projects for this version without distinction. It can be confusing.

> [!NOTE]
> In the `4.0.0` version of App Center breaking changes were introduced. Follow the [Migrate to App Center SDK 4.0.0 and higher](../getting-started/migration/apple-sdk-update.md) section to migrate App Center from previous versions.

[!INCLUDE [ios macos extension](includes/ios-macos-extensions.md)]

Look at the documentation for [App Center Crashes](~/sdk/crashes/macos.md) to learn how to customize your app extension and use more advanced functionalities.
