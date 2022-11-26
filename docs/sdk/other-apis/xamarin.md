---
title: Other Xamarin APIs
description: Other APIs in the App Center SDK for Xamarin
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 09/27/2021
ms.topic: article
ms.assetid: 64f8592a-73e0-4f08-9c29-4de82e2d1131
ms.tgt_pltfrm: xamarin
---

# Other MAUI and Xamarin APIs

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [React Native](react-native.md)
> * [MAUI/Xamarin](xamarin.md)
> * [UWP](uwp.md)
> * [WPF/WinForms](wpf-winforms.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [tvOS](tvos.md)
> * [Cordova](cordova.md)

## Adjust the log level

You can control the amount of log messages that show up from App Center in the console. Use the `LogLevel`-API to enable additional logging while debugging. By default, it's set to `ASSERT` for the App Store environments and `WARN` otherwise.

To have as many log messages as possible, use `LogLevel.Verbose`.

```csharp
AppCenter.LogLevel = LogLevel.Verbose;
```

## Identify installations

The App Center SDK creates a UUID for each device once the app is installed. This identifier remains the same for a device when the app is updated and a new one is generated only when the app is re-installed. The following API is useful for debugging purposes.

```csharp
System.Guid? installId = await AppCenter.GetInstallIdAsync();
```

> [!NOTE]
> On Android, this method must only be used after `AppCenter` has been started, it will always return `null` before start.

## Identify users

The App Center SDK supports setting a **user ID** that's used to augment crash reports. To use this capability:

1. Configure the App Center SDK by calling `AppCenter.Start(...)` as described in the [Getting started guide](~/sdk/getting-started/xamarin.md).
2. Set a `userID` in the SDK using the following code:

```csharp
AppCenter.SetUserId("your-user-id");
```

[!INCLUDE [user id](includes/user-id.md)]

## Disable all services at runtime

If you want to disable all App Center services at once, use the `Enabled` property. When disabled, the SDK won't forward any information to App Center.

```csharp
AppCenter.SetEnabledAsync(false);
```

To enable all services at once again, use the same API but pass `true` as a parameter.

```csharp
AppCenter.SetEnabledAsync(true);
```

You don't need to await this call to make other API calls (such as `IsEnabledAsync`) consistent.

The state is persisted in the device's storage across application launches.

> [!NOTE]
> This method must only be used after `AppCenter` has been started.

## Disallow network requests

In the App Center SDK, network requests are allowed by default. If you want to send data that the App Center SDK collects by the user concern you can disallow automatic sending data.

```csharp
AppCenter.IsNetworkRequestsAllowed = false;
```

In this case, the App Center SDK continues to collect data but it will be sent only when the network requests will be allowed.

```csharp
AppCenter.IsNetworkRequestsAllowed = true;
```

>[!NOTE]
> This value is retained between starts.

At any time, you can check whether sending data in the App Center SDK is allowed or not.

```csharp
AppCenter.IsNetworkRequestsAllowed;
```

>[!NOTE]
> The value saved previously in `SharedPreferences` is ignored until `AppCenter` is started on Android platform.
> It will return the last value set using `AppCenter.IsNetworkRequestsAllowed = allowed` or `true` if the value wasn't changed before AppCenter start.

## Change state of service in runtime

You can enable or disable the service at runtime with the following code:

```csharp
Analytics.SetEnabledAsync(true);
```

> [!NOTE]
> This method must only be used after `Analytics` has been started.

## Check if App Center is enabled

You can also check if App Center is enabled or not.

```csharp
bool enabled = await AppCenter.IsEnabledAsync();
```

> [!NOTE]
> This method must only be used after `AppCenter` has been started, it will always return `false` before start.

## Check App Center SDK version at runtime

You can get the version of App Center SDK that you're currently using.

```csharp
AppCenter.SdkVersion;
```
