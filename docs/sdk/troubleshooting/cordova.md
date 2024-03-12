---
title: Cordova SDK Troubleshooting
description: Troubleshooting the App Center SDK for Cordova
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 06/08/2020
ms.topic: article
ms.assetid: e1ef1165-dbc6-4e16-8438-c12060d529db
ms.tgt_pltfrm: cordova
---

# Cordova SDK Troubleshooting
[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [macOS](macos.md)
> * [tvOS](tvOS.md)
> * [React Native](react-native.md)
> * [Cordova](cordova.md)
> * [Unity](unity.md)
> * [Windows](uwp.md)
> * [MAUI/Xamarin](xamarin.md)

## Failed to install plugin - pod command failed with exit code 1

After updating to the latest plugin version, chances are you'll see the following error message during `cordova plugin add cordova-plugin-appcenter-<module>`:

```Text
Installing "cordova-plugin-appcenter-shared" for ios
Failed to install 'cordova-plugin-appcenter-shared': Error: pod: Command failed with exit code 1
```

In `0.3.0`, we dropped iOS 8 support. The issue with this is that the `4.5.5` version of `cordova-ios` targeted iOS `9.0` by default, but used iOS `8.0` in the podfile. The problem is fixed with `cordova-ios 5.0.0`.

There are two options:
1. `cordova platform add ios@5.0.0`
2. If you want to stay on an older cordova version, go to `platforms` > `ios`, open `Podfile` and change `platform :ios, '8.0'` to `platform :ios, '9.0'`. Then run `pod install`.

## Failed to install 'cordova-plugin-appcenter-shared': undefined

One possible cause of this error is when running `cordova plugin add cordova-plugin-appcenter-<module>` without **CocoaPods** installed.

In this case, the error usually includes this line:
```Text
Error: CocoaPods was not found. Please install version 1.0.1 or greater from https://cocoapods.org/
```

If you see this line, run:

```shell
sudo gem install cocoapods
pod setup
```

## CocoaPods could not find compatible versions for pod "AppCenter"

If you see the following error message during `cordova plugin add cordova-plugin-appcenter-<module>`, chances are your local clone (`~/.cocoapods/repos` on your system) of the [CocoaPods spec repo](https://github.com/CocoaPods/Specs) isn't up to date.

```Text
CocoaPods could not find compatible versions for pod "AppCenter":
 In Podfile:
   AppCenter (~> 1.12.0)
```

Run `pod repo update` to update the spec repo, and try adding the plugin again. For more information on CocoaPods commands, see the [CocoaPods command line reference](https://guides.cocoapods.org/terminal/commands.html#pod_repo_update).

## Module 'AppCenter' not found iOS build error

If you see the following error message during `cordova build ios`, it means that something went wrong with **CocoaPods** installation:

```Text
.../cordova-plugin-appcenter-shared/AppCenterShared.h:3:9: fatal error: module 'AppCenter' not found
@import AppCenter;
~~~~~~~^~~~~~~~~
1 error generated.
```

One possible cause is that your local clone (`~/.cocoapods/repos` on your machine) of the [CocoaPods spec repo](https://github.com/CocoaPods/Specs) isn't up to date.

Run `pod repo update` to update the spec repo, and try adding plugin again. For more information on CocoaPods commands, see the [CocoaPods command line reference](https://guides.cocoapods.org/terminal/commands.html#pod_repo_update).

## Archive not found at path iOS build error

If you see the following error message during `cordova build ios`, you must switch to the legacy build system.

```Text
error: archive not found at path '.../platforms/ios/MyApp.xcarchive'
** EXPORT FAILED **
```

To resolve this issue, specify `--buildFlag="-UseModernBuildSystem=0"` when running build. The solution is also described in [this StackOverflow thread](https://stackoverflow.com/a/52400072/7453375).

## Could not find method google() for arguments [] on repository container Android build error

If you see the following error message during `cordova build android`, chances are your `cordova-android` version is too low.

```Text
FAILURE: Build failed with an exception.

* Where:
Script '.../cordova/platforms/android/cordova-plugin-appcenter-*/*.gradle' line: 3

* What went wrong:
A problem occurred evaluating script.
> Could not find method google() for arguments [] on repository container.
```

Update `cordova-android` to at least `6.4.0`. You can run `cordova platform update android` to update the platform to the latest version or `cordova platform add android@6.4.0` to use the exact version.

## Analytics not showing up in the portal

Make sure you've added this line to your configuration file:

`<preference name="APPCENTER_ANALYTICS_ENABLE_IN_JS" value="true" />`

By default this value is `false`.

## Protect the App Center secret value

[!INCLUDE [app secret secure](../includes/app-secret-secure.md)]
