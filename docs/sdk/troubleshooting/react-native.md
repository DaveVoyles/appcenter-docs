---
title: React Native SDK Troubleshooting
description: Troubleshooting the App Center SDK for React Native
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 02/26/2021
ms.topic: article
ms.assetid: e92ba11b-fa9f-41b0-8c7e-aa7650b3cfcc
ms.tgt_pltfrm: react-native
---

# React Native SDK Troubleshooting
[!INCLUDE [Retirement announcement for App Center](../../includes/retirement.md)]

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

## 'React/RCTDefines.h' file not found
This error appears when RN core libraries aren't referenced correctly, which can be caused by different kinds of integrating or linking issues. It often happens when you have your dependencies linked using relative path in a podfile, rather than statically in a project.

Our linking script supports only the standard way of linking pods, so to resolve the problem:

1. Replace the dependencies in your `Podfile` with relative linking paths:

    Before:

    ```ruby
    pod 'AppCenter/Analytics', '~> 1.14.0'
    pod 'AppCenter/Crashes', '~> 1.14.0'
    pod 'AppCenterReactNativeShared', '~> 1.13.0'
    ```

    After:

    ```ruby
    pod 'appcenter', path: '../node_modules/appcenter/ios'
    pod 'appcenter-analytics', path: '../node_modules/appcenter-analytics/ios'
    pod 'appcenter-crashes', path: '../node_modules/appcenter-crashes/ios'
    ```

1. Run `pod install` from `iOS` folder.

1. Inside **AppDelegate.m**, replace imports:

    Before:

    ```objective-c
    #import <AppCenterReactNative/AppCenterReactNative.h>
    #import <AppCenterReactNativeAnalytics/AppCenterReactNativeAnalytics.h>
    #import <AppCenterReactNativeCrashes/AppCenterReactNativeCrashes.h>
    ```

    After:

    ```objective-c
    #import "AppCenterReactNative.h"
    #import "AppCenterReactNativeAnalytics.h"
    #import "AppCenterReactNativeCrashes.h"
    ```

1. Remove AppCenter dependencies from the project (right-click the dependency > Remove dependency).
    * Select **<YOUR_APP>** -> **Libraries** and remove next files:
        * `AppCenterReactNative.xcodeproj`
        * `AppCenterReactNativeAnalytics.xcodeproj`
        * `AppCenterReactNativeCrashes.xcodeproj`
    * Open your project settings and under **General** tab in the **Linked Frameworks and Libraries** section remove new items referencing target libraries removed on the previous step:
        * `libAppCenterReactNative.a`
        * `libAppCenterReactNativeAnalytics.a`
        * `libAppCenterReactNativeCrashes.a`
    * Modify **Header Search Paths** to remove headers from the AppCenter React Native plugins projects. Open your project settings and under **Build Settings** tab in the **Header Search Paths** section remove the following locations for header files:
        * `$(SRCROOT)/../node_modules/appcenter/ios/AppCenterReactNative`
        * `$(SRCROOT)/../node_modules/appcenter-analytics/ios/AppCenterReactNativeAnalytics`
        * `$(SRCROOT)/../node_modules/appcenter-crashes/ios/AppCenterReactNativeCrashes`

## React Native link command unrecognized

If you have `yarn` installed on your machine, `react-native init {myapp}` will initialize your app and install dependencies using `yarn` rather than `npm`. If you see the following error message when running `react-native link`, chances are App Center dependencies are installed from `npm install {package}` instead of `yarn add {package}` so that `npm` and `yarn` are mixed during installation.

```Text
Command `link` unrecognized. Make sure that you have run `npm install` and that you are inside a react-native project.
```

In this case, run `npm install` and try `react-native link` again.

## Build error such as 'AppCenterCrashes/MSACErrorReport.h' file not found

### React-Native 0.60 and above

The likely cause is the conflict between [major](https://semver.org/) package versions because of a [breaking change](https://github.com/microsoft/appcenter-sdk-apple/releases/tag/4.0.0) in our Apple SDK.

1. Make sure that all `appcenter` packages use the same major version. Update them with `npm install` or `yarn` if needed.

1. Go to **ios** folder of your project and remove **Podfile.lock** and the **Pods** folder.
1. Run `pod install --repo-update` in your **ios** folder.
1. Verify that **Podfile.lock** contains `appcenter` dependencies of the same major version.

### React Native lower than 0.60

One possible cause is when running `react-native link` without **CocoaPods** installed.

To confirm the cause, execute `react-native link`, and in the logs, check for the following line:

```Text
Could not configure AppCenter for iOS. Error Reason - spawn pod ENOENT
```

This line is located in the following section of the logs:

```Text
Added code to initialize iOS AppCenter SDK in ios/TestApp/AppDelegate.m
Installing Cocoapods dependencies...
Could not configure AppCenter for iOS. Error Reason - spawn pod ENOENT
rnpm-install info Platform 'ios' module appcenter-analytics is already linked
rnpm-install info Platform 'android' module appcenter-analytics is already linked
```

If you see that error, make sure the `pod` command from **CocoaPods** is available in your system's **PATH** environment variable.

After you fix your **CocoaPods** installation, run `pod install` in the **ios** folder to fix your project.

## CocoaPods unable to find a specification for AppCenterReactNativeShared during react-native link

If you see the following CocoaPods error message during `react-native link`, chances are your local clone (`~/.cocoapods/repos` on your machine) of the [CocoaPods spec repo](https://github.com/CocoaPods/Specs) isn't up to date.

```Text
Analyzing dependencies [!] Unable to find a specification for AppCenterReactNativeShared (~> {version})
```

Run `pod repo update` to update the spec repo, and try `react-native link` again. For more information of CocoaPods commands, see [CocoaPods command line reference](https://guides.cocoapods.org/terminal/commands.html#pod_repo_update).

## How to upgrade app using App Center SDK from React Native 0.59 to 0.60

For upgrading React Native to the version 0.60.0, use the following command:

```shell
react-native upgrade 0.60.0
```

> [!NOTE]
> If during an upgrade you get this error `Command failed: git status -s fatal: not a git repository (or any of the parent directories): .git`, perform the next steps:
>   ```shell
>   git init
>   git add .
>   git commit -m "Upgrade react-native"
>   ```
> After you finish upgrading you may remove the `.git` directory.

#### Update React Native iOS

1. Open **Podfile** and replace App Center dependencies on the following line:

    ```ruby
    use_native_modules!
    ```

2. Run the following command:

    ```shell
    pod repo update
    ```

## How to update React Native SDK to the latest version

If you've already integrated our SDK into your application and want to upgrade to a newer version of the SDK, follow the following steps to upgrade:

1. `npm uninstall --save appcenter appcenter-analytics appcenter-crashes` to uninstall old App Center packages.

2. `npm install --save-exact appcenter appcenter-analytics appcenter-crashes` to install latest App Center packages.

3. `pod repo update` to make sure your CocoaPods spec repo is up to date.

4. `react-native link`.

> [!TIP]
> If you see error message similar to the following CocoaPods error message during `react-native link`, delete the auto-generated **Podfile.lock** in your **ios** folder and run `react-native link` again.

> ```Text
> [!] Unable to satisfy the following requirements:
>
> - `AppCenter/Core (= 1.0.0)` required by `Podfile.lock`
> - `AppCenter/Core (= 1.0.0)` required by `AppCenter/Crashes (1.0.0)`
> - `AppCenter/Core (= 1.0.0)` required by `AppCenter/Analytics (1.0.0)`
> - `AppCenter/Core (= 1.0.1)` required by `AppCenterReactNativeShared (1.0.1)`
> ```

## Migration from jCenter to Maven Central

Due to [termination of jCenter support](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) all our assemblies were moved to the Maven Central repository. 
To use App Center, you need to add `mavenCentral()` to the repository in your Gradle file like below:

   ```groovy
   repositories {
      google()
      mavenCentral()
   }
   ```

## Protect the App Center secret value

[!INCLUDE [app secret secure](../includes/app-secret-secure.md)]
