---
title: App Center Analytics for Windows
description: App Center Analytics for Windows
keywords: analytics
author: lucen-ms
ms.author: lucen
ms.date: 11/22/2021
ms.topic: article
ms.assetid: 7835dedf-b170-416b-8a89-0a2a18f6196b
ms.custom: sdk
ms.tgt_pltfrm: windows
---

# App Center Analytics (Windows)

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [React Native](react-native.md)
> * [Windows](windows.md)
> * [Xamarin](xamarin.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [tvOS](tvos.md)
> * [Cordova](cordova.md)

App Center Analytics helps you understand user behavior and customer engagement to improve your app. The SDK automatically captures session count and device properties like model, OS version, etc. You can define your own custom events to measure things that matter to you. All the information captured is available in the App Center portal for you to analyze the data.

Follow the [WPF/WinForms Getting Started](~/sdk/getting-started/wpf-winforms.md) or [UWP/WinUI Getting Started](~/sdk/getting-started/uwp.md) section (based on your platform) if you haven't set up the SDK in your application yet.

The instructions in this page work for UWP (including Xamarin.Forms and WinUI), WPF, and WinForms.

## Session and device information

Once you add App Center Analytics to your app and start the SDK, it will automatically track sessions and device properties like OS Version, model, etc.

> [!NOTE]
> On WinUI apps, the amount of sessions may be lower than on UWP apps due to specifics of its lifecycle.

### Country Code

Country code isn't automatically reported by the SDK. If you want to report it manually, you can follow the instructions for your platform below.

#### UWP

1. Make sure that you [enable Location Capability](/windows/uwp/maps-and-location/get-location#enable-the-location-capability) for your app.
2. [Obtain a Bing Maps Authentication Key](/windows/uwp/maps-and-location/authentication-key#get-a-key).
3. Use the following code in any place before you call `AppCenter.Start(... typeof(Analytics) ...);`. 
	As `BingMapsToken`, use the key obtained in step 2.

```csharp
private static async Task SetCountryCode()
{
    // The following country code is used only as a fallback for the main implementation.
    // This fallback country code doesn't reflect the physical device location, but rather the
    // country that corresponds to the culture it uses.
    var countryCode = new GeographicRegion().CodeTwoLetter;
    var accessStatus = await Geolocator.RequestAccessAsync();
    switch (accessStatus)
    {
        case GeolocationAccessStatus.Allowed:
            var geoLocator = new Geolocator
            {
                DesiredAccuracyInMeters = 100
            };
            var position = await geoLocator.GetGeopositionAsync();
            var myLocation = new BasicGeoposition
            {
                Longitude = position.Coordinate.Point.Position.Longitude,
                Latitude = position.Coordinate.Point.Position.Latitude
            };
            var pointToReverseGeocode = new Geopoint(myLocation);
            MapService.ServiceToken = Constants.BingMapsAuthKey;
            var result = await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);
            if (result.Status != MapLocationFinderStatus.Success || result.Locations == null || result.Locations.Count == 0)
            {
                break;
            }

            // The returned country code is in 3-letter format (ISO 3166-1 alpha-3).
            // Below we convert it to ISO 3166-1 alpha-2 (two letter).
            var country = result.Locations[0].Address.CountryCode;
            countryCode = new GeographicRegion(country).CodeTwoLetter;
            break;
        case GeolocationAccessStatus.Denied:
            AppCenterLog.Info(LogTag, "Geolocation access denied. To set country code in App Center, enable location service in Windows 10.");
            break;
        case GeolocationAccessStatus.Unspecified:
            break;
    }
    AppCenter.SetCountryCode(countryCode);
}
```

> [!NOTE]
> For country code to be displayed on Analytics sessions, `AppCenter.SetCountryCode` must be called prior to calling
> `AppCenter.Start`.

#### WPF/WinForms

As WPF/WinForms platforms don't have a Geolocation API, you can use a system country code.

```csharp
using System.Globalization;

private static void SetCountryCode()
{
    // This fallback country code doesn't reflect the physical device location, but rather the
    // country that corresponds to the culture it uses.
    var countryCode = RegionInfo.CurrentRegion.TwoLetterISORegionName;
    AppCenter.SetCountryCode(countryCode);
}
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

## Check if App Center Analytics is enabled

You can also check if App Center Analytics is enabled or not.

```csharp
bool isEnabled = await Analytics.IsEnabledAsync();
```

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

By default, the SDK stores all the event logs up to 10 MB. Developers can use an API to increase the [storage size](../other-apis/wpf-winforms.md#storage-size) and the SDK will keep storing logs until the storage is full.

## No internet access

When there isn't any network connectivity, the SDK saves up to 10MB of logs in the local storage. Once the storage is full, the SDK will start discarding old logs to make room for the new logs. Once the device gets internet access back, the SDK will send logs in the batch of 50 or after every 6 seconds.

## Batching event logs

The App Center SDK uploads logs in a batch of 50 and if the SDK doesn't have 50 logs to send, it will still send logs after 6 seconds. There can be a maximum of 3 batches sent in parallel.

## Retry and back-off logic

App Center SDK supports back-off retries on recoverable network errors. Below is the retry logic:

* 3 tries maximum per request.
* Each request has its own retry state machine.
* All the transmission channels are disabled (until next app process) after 1 request exhausts all its retries.

Back-off logic

* 50% randomization, 1st retry between 5 and 10s, second retry between 2.5 and 5 minutes, last try between 10 and 20 minutes.
* If network switches from off to on (or from wi-fi to mobile), retry states are reset and requests are retried immediately.
