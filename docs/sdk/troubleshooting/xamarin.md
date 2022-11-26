---
title: Xamarin SDK Troubleshooting
description: Troubleshooting the App Center SDK for Xamarin
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 08/16/2021
ms.topic: article
ms.assetid: 74801c55-8394-4f86-b7e1-8a8b4cbf4b37
ms.tgt_pltfrm: xamarin
---

# Xamarin/MAUI SDK Troubleshooting

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [macOS](macos.md)
> * [tvOS](tvOS.md)
> * [React Native](react-native.md)
> * [Cordova](cordova.md)
> * [Unity](unity.md)
> * [Windows](uwp.md)
> * [Xamarin](xamarin.md)

## Collecting verbose logs

To get instructions on how to adjust the log level to `VERBOSE`, refer to [Other APIs](../other-apis/xamarin.md#adjust-the-log-level) section.  
You can find the verbose logs in the **Debug** > **Windows** > **Output** window.

## Issues during setup

1. For a Xamarin.Forms app, make sure the packages are installed in all the projects that reference any libraries. Otherwise you'll see errors.
2. If you see this error when building for Xamarin.iOS: `MTOUCH: Error MT3001: Could not AOT the assembly 'obj/**/Build/Microsoft.AppCenter.**.iOS.Bindings.dll' (MT3001)` you need to update the Xamarin.iOS component to version **10.4.0.128** or later.
3. If you see this error when building for Xamarin.iOS: `MTOUCH: Error MT5210: Native linking failed, undefined symbol: _OBJC_METACLASS_$_MS{SomeSdkClassName}. Please verify that all the necessary frameworks have been referenced and native libraries are properly linked in. (MT5210)` or a similar error (`MT5211` code with similar message mentioning App Center), make sure you call `AppCenter.Start` before using the APIs of a specific service. If you're calling `Start` but have this issue, you need to update Xamarin.iOS component to version **10.4.0.128** or more recent.
4. In the console, look for an Assert log with the message - "App Center SDK configured successfully". This verifies that the SDK is configured successfully.

## Analytics data doesn't show up in the portal

1. Make sure you've integrated the SDK modules correctly.
2. Make sure correct App Secret is included along with the `Start()` method call. You can copy the exact `Start()` code by opening the app in the portal and navigating to Getting Started page.
3. If you want to see the logs that get sent to the backend, change the log level to **Verbose** in your application and the SDK will print logs in the console. Call the API below before you start the SDK.

   ```csharp
   AppCenter.LogLevel = LogLevel.Verbose;
   ```

   Check the logs say "App Center SDK configured successfully" (in Info log level), then check if you see HTTPS request logs.

4. Make sure your device is online.
5. At times, logs might take few minutes to surface in the portal. Wait for some time if that’s the case.
6. To check if App Center backend received your data, go to the Log flow section in Analytics service. Your events should appear once it's been sent.

## Crashes don't show up in the portal

1. Make sure you've integrated the SDK modules correctly.
1. Make sure correct App Secret is included along with the `Start()` method call. You can copy the exact `Start()` code by opening the app in the portal and navigating to Getting Started page.
1. Restart the app after a crash. App Center Crashes will forward the crash log only after it's restarted. Also, on Xamarin.iOS and Xamarin.Mac the SDK won't save any crash log if you attached a debugger. Make sure the debugger isn't attached when you crash the iOS app. On Xamarin.Android, you can crash while having debugger attached but you need to continue execution after breaking into the unhandled exception.
1. If you want to see the logs that get sent to the backend, change the log level to **Verbose** in your application and the SDK will print logs in the console. Call the API below before you start the SDK.

   ```csharp
   AppCenter.LogLevel = LogLevel.Verbose;
   ```

   Check the logs say "App Center SDK configured successfully" (in Info log level), then check if you see HTTPS request logs.

1. Don't use any other library that provides Crash Reporting functionality, such as Xamarin Insights or HockeyApp. You can only have one crash reporting SDK integrated.
1. Make sure your device is online.
1. At times, logs might take few minutes to surface in the portal. Wait for some time if that’s the case.
1. If you want to check if the SDK detected the crash on the next app start, you can call the API to check whether the app crashed in the last session and shows an alert. Or you can extend the crash callback to see if it was successfully sent to the server.
1. To check if App Center backend received the crash, go to the Log flow section in the Analytics service. Your crashes should appear there, once it's been sent.

## Distribute and in-app updates are blocking my automated UI tests

If you're running automated UI tests, enabled in-app updates will block your automated UI tests as they'll try to authenticate against the App Center backend. We recommend to not enable App Center Distribute for your UI tests. 

## Google Play rejects the application after adding DistributePlay

Google Play rejects apps that use the `Distribute` module because it contains in-app update implementation. To avoid this situation, App Center SDK has `DistributePlay` module, which contains stubs for the main module. It replaces the main assemble with stubbed assemble, which imitates the `Distribute` API. No additional changes are required in the client application. The `DistributePlay` doesn't have any in-app update related code.
If after replacing `Distribute` module with `DistributePlay` your app still gets rejected, please follow the steps below to verify that the `Distribute` module was replaced correctly:
- Make sure that you replaced `Distribute` with `DistributePlay` in all your releases for all your publishing groups in Google Play Console.
- If you use `Xamarin.Forms` please make sure that you add the `DistributePlay` module to both shared and `Xamarin.Android` projects.
- Verify that your output file does not contain the `Microsoft.AppCenter.Distribute.Android.Bindings.dll` assembly. You can use the [APK Analyzer](https://developer.android.com/studio/build/apk-analyzer) to check it.

## Messages in the console that indicate that the database couldn't be opened on iOS

App Center uses SQLite to persist logs before they're sent to the backend. If you're bundling your application with your own SQLite library instead of using the one provided by the OS, you might see errors like this in the console `[AppCenter] ERROR: -[MSACDBStorage executeSelectionQuery:]/147 Failed to open database` and won't see any analytics or crash information in the backend. Update the SDK to version 0.16.0 or later.

## Protect the App Center secret value

[!INCLUDE [app secret secure](../includes/app-secret-secure.md)]
