---
title: Understanding App Center Events Metrics
description: Help using the Events Metrics on App Center
keywords: app center, analytics
author: lucen-ms
ms.author: lucen
ms.date: 07/15/2021
ms.topic: article
ms.assetid: 85da48c4-7abb-49be-92df-3ae814529688
ms.service: vs-appcenter
ms.custom: analytics
---

# Events Metrics
 
[!INCLUDE [Retirement announcement for App Center](../includes/retirement.md)]

Events are actions that users take in your app. By tracking events in your app, you'll learn more about your users' behavior and understand the interaction between your users and your apps. Attaching properties to your events will provide you with contextual information about the events.

Tracking events in your app allows you to answer questions like:

- What's the top N content viewed?
- What's the most liked content?
- What type of files do my users send?

To track custom events in your app, use our `trackEvent` API from the App Center Analytics SDK. More information on how to define custom events using our SDK can be found in [App Center SDK Documentation](~/sdk/index.md) for [Android](~/sdk/analytics/android.md), [iOS](~/sdk/analytics/ios.md), [React Native](~/sdk/analytics/react-native.md), [MAUI/Xamarin](~/sdk/analytics/xamarin.md), [Windows](~/sdk/analytics/windows.md), [macOS](~/sdk/analytics/macos.md), and [Unity](~/sdk/analytics/unity.md).

## General information

You can filter the events page by **time range** (the last 1, 7 or 28 days), and for the selected **app version**. These filters allow you to drill down into your events data to the selected time period or app version.

## Events Page
The Events Page provides a table with an overview of the events happening in your app. This table can be sorted by count in both descending and ascending order. You can also look for an event in the table by its name. For each event, the following information is provided:

- **Count**: Number of times that an event has been triggered.
- **Trend**: Change of the count in absolute value and percentage change, compared to the previous selected time period.
- **Users**: Number of users that have triggered an event.
- **User change**: Change of the number of users in absolute value and percentage, compared to the previous selected time period.
- **Count per user**: Average number of times that an event is triggered per user.

## Event Detail View
Select an event within the events table to reveal more detailed information:

- **Users**: Total number of users that have triggered the event. LAST (n) DAYS is a trend calculation.
- **Count**: Total number of times the event has been triggered.
- **Count per user**: Average number of times the event is triggered per user.
- **Count per session**: Average number of times that the event is triggered per session.

### Delete Event History
Remove individual events from the Event Details view by clicking the settings option on top right of the page and selecting "Delete event history". This action cannot be undone.

## Events Properties

Events Properties provide contextual information about the events. They allow a deeper understanding about the actions that the users take in your app.

Examples of events and properties are:

- *Event Name:* "File Upload", *Event properties:* ".PDF", ".txt", ".PNG".
- *Event Name:* "Question answered", *Event properties:* "Accept", "Decline", "Ignore".

Property values are optimized for values of type 'String'.

## Limits

- The maximum number of distinct custom events that can be tracked daily is 200. This count resets at 12 AM UTC. However, there isn't a limit on the maximum number of events instances sent per device.
- The maximum number of event property names and/or values per event is 20. When more than 20 property names and/or values are sent, only the first 20 ones are processed, and the rest are automatically dropped.
- The SDK truncates event names to the first 256 characters.
- The SDK truncates each property key and value to the first 125 characters.
- The portal will show up to the 10 property values with highest counts.

## Breadcrumbs (Events before a crash)

Events can help you identify the root cause of app crashes. As part of the App Center Crashes service, you can see all the events that happened before a crash for a specific session.

To see the events before a crash, integrate the Analytics SDK into the app, and define one or more events.

Breadcrumbs are stored for 28 days, while crashes can potentially be stored indefinitely. This means that if you're looking at a crash that's older than 28 days, you won't see the corresponding breadcrumbs.

## Handled Errors

To track handled errors in App Center, we recommend that you use the specific functionality for it. Check out more information on this topic in our [official documentation](~/diagnostics/index.md#errors).
