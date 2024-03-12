---
title: App Center Crashes for macOS
description: App Center Crashes for macOS
keywords: sdk, crash
author: lucen-ms
ms.author: lucen
ms.date: 10/22/2020
ms.topic: article
ms.assetid: 3f6481de-55d6-11e7-907b-a6006ad3dba0
ms.custom: sdk
ms.tgt_pltfrm: macos
dev_langs:  
 - swift
 - objc
---

# App Center Crashes (macOS)
[!INCLUDE [Retirement announcement for App Center](../../includes/retirement.md)]
> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [MAUI/Xamarin](xamarin.md)
> * [UWP](uwp.md)
> * [WPF/WinForms](wpf-winforms.md)
> * [React Native](react-native.md)
> * [macOS](macos.md)
> * [tvOS](tvos.md)
> * [Unity](unity.md)
> * [Cordova](cordova.md)

App Center Crashes will automatically generate a crash log every time your app crashes. The log is first written to the device's storage and when the user starts the app again, the crash report will be sent to App Center. Collecting crashes works for both beta and live apps, i.e. those submitted to the App Store. Crash logs contain valuable information for you to help fix the crash.

Follow the [Getting Started](~/sdk/getting-started/macos.md) section if you haven't set up the SDK in your application yet.

Also, crash logs on macOS require Symbolication, check out the [App Center Diagnostics documentation](~/diagnostics/iOS-symbolication.md) that explains how to provide symbols for your app.

> [!NOTE]
> In the `4.0.0` version of App Center breaking changes were introduced. Follow the [Migrate to App Center SDK 4.0.0 and higher](../getting-started/migration/apple-sdk-update.md) section to migrate App Center from previous versions.

### Crash reporting in extensions

App Center supports crash reporting in macOS extensions. The usage is the same as in the container application.

[!INCLUDE [apple common methods](includes/apple-common-methods-1.md)]

### Ask for the user's consent to send a crash log

If user privacy is important to you, you might want to get user confirmation before sending a crash report to App Center. The SDK exposes a callback that tells App Center Crashes to await user confirmation before sending any crash reports.

If you chose to do so, you're responsible for obtaining the user's confirmation, e.g. through a dialog prompt with one of the following options: **Always send**, **Send**, and **Don't send**. Based on the input, you'll tell App Center Crashes what to do and the crash will then be handled accordingly.

> [!NOTE]
> The SDK doesn't display a dialog for this, the app must provide its own UI to ask for user consent.

The following method shows how to set up a user confirmation handler:

```objc
[MSACCrashes setUserConfirmationHandler:(^(NSArray<MSACErrorReport *> *errorReports) {

  // Your code to present your UI to the user, e.g. an NSAlert.
  NSAlert *alert = [[NSAlert alloc] init];
  [alert setMessageText:@"Sorry about that!"];
  [alert setInformativeText:@"Do you want to send an anonymous crash report so we can fix the issue?"];
  [alert addButtonWithTitle:@"Always send"];
  [alert addButtonWithTitle:@"Send"];
  [alert addButtonWithTitle:@"Don't send"];
  [alert setAlertStyle:NSWarningAlertStyle];

  switch ([alert runModal]) {
  case NSAlertFirstButtonReturn:
    [MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationAlways];
    break;
  case NSAlertSecondButtonReturn:
    [MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationSend];
    break;
  case NSAlertThirdButtonReturn:
    [MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationDontSend];
    break;
  default:
    break;
  }

  return YES; // Return YES if the SDK should await user confirmation, otherwise NO.
})];
```
```swift
Crashes.setUserConfirmationHandler({ (errorReports: [ErrorReport]) in

  // Your code to present your UI to the user, e.g. an NSAlert.
  let alert: NSAlert = NSAlert()
  alert.messageText = "Sorry about that!"
  alert.informativeText = "Do you want to send an anonymous crash report so we can fix the issue?"
  alert.addButton(withTitle: "Always send")
  alert.addButton(withTitle: "Send")
  alert.addButton(withTitle: "Don't send")
  alert.alertStyle = NSWarningAlertStyle

  switch (alert.runModal()) {
  case NSAlertFirstButtonReturn:
    Crashes.notify(with: .always)
    break;
  case NSAlertSecondButtonReturn:
    Crashes.notify(with: .send)
    break;
  case NSAlertThirdButtonReturn:
    Crashes.notify(with: .dontSend)
    break;
  default:
    break;
  }

  return true // Return true if the SDK should await user confirmation, otherwise return false.
})
```

In case you return `YES`/`true` in the handler block above, your app should obtain user permission and message the SDK with the result using the following API. If you're using an alert for this, as we do in the sample above, you'd call it based on the result (**ModalResponse**) of `runModal` call.

```objc
// Depending on the user's choice, call notifyWithUserConfirmation: with the right value.
[MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationDontSend];
[MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationSend];
[MSACCrashes notifyWithUserConfirmation:MSACUserConfirmationAlways];
```
```swift
// Depending on the user's choice, call notify(with:) with the right value.
Crashes.notify(with: .dontSend)
Crashes.notify(with: .send)
Crashes.notify(with: .always)
```

## Enable catching uncaught exceptions thrown on the main thread

AppKit catches exceptions thrown on the main thread, preventing the application from crashing on macOS, so the SDK can't catch these crashes. To mimic iOS behavior, set `NSApplicationCrashOnExceptions` flag before SDK initialization, this flag allows the application to crash on uncaught exceptions and the SDK can report them.

```objc
[[NSUserDefaults standardUserDefaults] registerDefaults:@{ @"NSApplicationCrashOnExceptions" : @YES }];
```
```swift
UserDefaults.standard.register(defaults: ["NSApplicationCrashOnExceptions": true])
```

> [!NOTE]
> App Center SDK set the flag automatically in versions 1.10.0 and below. Starting with version 1.11.0 this flag is no longer set automatically.

### Disable forwarding of the application main class's methods calls to App Center Crashes

The App Center Crashes SDK uses swizzling to improve its integration by forwarding itself some of the application main class's methods calls. Method swizzling is a way to change the implementation of methods at runtime. If for any reason you don't want to use swizzling (e.g. because of a specific policy), you should override the application's `reportException:` and `sendEvent:` methods yourself in order for Crashes to report exceptions thrown on the main thread correctly.

1. Create **ReportExceptionApplication.m** file and add the following implementation:

    ```objc
    @import Cocoa;
    @import AppCenterCrashes;

    @interface ReportExceptionApplication : NSApplication
    @end

    @implementation ReportExceptionApplication

    - (void)reportException:(NSException *)exception {
      [MSACCrashes applicationDidReportException:exception];
      [super reportException:exception];
    }

    - (void)sendEvent:(NSEvent *)theEvent {
      @try {
        [super sendEvent:theEvent];
      } @catch (NSException *exception) {
        [self reportException:exception];
      }
    }

    @end
    ```

    > [!NOTE]
    > Swift's `try`/`catch` doesn't work with `NSException`. These exceptions can be handled in Objective-C only.

2. Open **Info.plist** file and replace the **NSApplication** in the **Principal class** field with your application class name, **ReportExceptionApplication** in this example.

3. To disable swizzling in **App Center SDK**, add the `AppCenterApplicationForwarderEnabled` key to **Info.plist** file, and set the value to `0`.

[!INCLUDE [apple common methods](includes/apple-common-methods-2.md)]

## Disabling Mach exception handling

By default, App Center Crashes uses the Mach exception handler to catch fatal signals, e.g. stack overflows, via a Mach exception server.

The `disableMachExceptionHandler`-method provides an option to disable catching fatal signals via a Mach exception server. If you want to disable the Mach exception handler, you should call this method *BEFORE* starting the SDK. Your typical setup code would look like this:

```objc
[MSACCrashes disableMachExceptionHandler];
[MSACAppCenter start:@"{Your App Secret}" withServices:@[[MSACAnalytics class], [MSACCrashes class]]];
```
```swift
Crashes.disableMachExceptionHandler()
AppCenter.start(withAppSecret: "{Your App Secret}", services: [Analytics.self, Crashes.self])
```
