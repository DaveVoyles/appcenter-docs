---
title: App Center Analytics for Xamarin
description: App Center Analytics for Xamarin
keywords: analytics
author: lucen-ms
ms.author: lucen
ms.date: 11/26/2021
ms.topic: article
ms.assetid: d70cdd52-e53c-45df-89df-6394ed887174
ms.custom: sdk
ms.tgt_pltfrm: xamarin
---

# App Center Analytics (MAUI and Xamarin)
[!INCLUDE [Retirement announcement for App Center](../../includes/retirement.md)]

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [React Native](react-native.md)
> * [Windows](windows.md)
> * [MAUI/Xamarin](xamarin.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [tvOS](tvos.md)
> * [Cordova](cordova.md)

App Center Analytics helps you understand user behavior and customer engagement to improve your app. The SDK automatically captures session count and device properties like model, OS version, etc. You can define your own custom events to measure things that matter to you. All the information captured is available in the App Center portal for you to analyze the data.

Follow the [Get started](~/sdk/getting-started/xamarin.md) section if you haven't set up the SDK in your application yet.

## Session and device information

Once you add App Center Analytics to your app and start the SDK, it will automatically track sessions and device properties like OS Version, model, etc. without writing any additional code.

### Country Code

The SDK automatically reports a user's country code if the device has a mobile data modem and a SIM card installed. WiFi-only devices won't report a country code by default. To set the country code of those users, you must retrieve your user's location yourself and use the `SetCountryCode:` method in the SDK:

```csharp
AppCenter.SetCountryCode("en");
```

> [!NOTE]
> For country code to be displayed on Analytics sessions, `AppCenter.SetCountryCode` must be called prior to calling
> `AppCenter.Start`.

## Custom events

You can track your own custom events with **up to 20 properties** to understand the interaction between your users and the app.

Once you've started the SDK, use the `TrackEvent()` method to track your events with properties. You can send **up to 200 distinct event names**. Also, there's a maximum limit of 256 characters per event name and 125 characters per event property name and event property value.

```csharp
Analytics.TrackEvent("Video clicked", new Dictionary<string, string> {
	{ "Category", "Music" },
	{ "FileName", "favorite.avi"}
});
```

Properties for events are entirely optional – if you just want to track an event, use this sample instead:

```csharp
Analytics.TrackEvent("Video clicked");
```

## Enable or disable App Center Analytics at runtime

You can enable and disable App Center Analytics at runtime. If you disable it, the SDK won't collect any more analytics information for the app.

```csharp
Analytics.SetEnabledAsync(false);
```

To enable App Center Analytics again, use the same API but pass `true` as a parameter.

```csharp
Analytics.SetEnabledAsync(true);
```

You don't need to await this call to make other API calls (such as `IsEnabledAsync`) consistent.

The state is persisted in the device's storage across application launches.

> [!NOTE]
> This method must only be used after `Analytics` has been started.

## Check if App Center Analytics is enabled

You can also check if App Center Analytics is enabled or not.

```csharp
bool isEnabled = await Analytics.IsEnabledAsync();
```

> [!NOTE]
> This method must only be used after `Analytics` has been started, it will always return `false` before start.

[!INCLUDE [manual session tracker](includes/manuall-session-tracker.md)]

- Call the following method before the SDK start:

```csharp
Analytics.EnableManualSessionTracker();
```

- Then you can use the `StartSession` API after the `AppCenter.Start`:

```csharp
Analytics.StartSession();
```

## Local storage size

By default, the SDK stores up to 10 MB of logs in the storage.

## No internet access

When there isn't any network connectivity, the SDK saves up to 10 MB of logs in the local storage. Once the storage is full, the SDK will start discarding old logs to make room for the new logs. Once the device gets internet access back, the SDK will send logs in the batch of 50 or after every 6 seconds.

## Batching event logs

The App Center SDK uploads logs in a batch of 50 and if the SDK doesn't have 50 logs to send, it will still send logs after 6 seconds. There can be a maximum of three batches sent in parallel.

## Retry and back-off logic

App Center SDK supports back-off retries on recoverable network errors. Below is the retry logic:
* 3 tries maximum per request.
* Each request has its own retry state machine.
* All the transmission channels are disabled (until next app process) after one request exhausts all its retries.

Back-off logic
* 50% randomization, first retry between 5s and 10s, second retry between 2.5 and 5 minutes, last try between 10 and 20 minutes.
* If network switches from off to on (or from wi-fi to mobile), retry states are reset and requests are retried immediately.