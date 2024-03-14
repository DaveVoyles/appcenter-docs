---
title: SDK Limitations & Known Issues
description: Limitations and known issues with the App Center SDK
keywords: sdk
author: lucen-ms
ms.author: lucen
ms.date: 07/29/2021
ms.topic: article
ms.assetid: e1e427f3-6949-4266-b1cd-13da703fc132
---

# SDK Limitations & Known Issues
[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

## React Native

1. The React Native SDK doesn't support In-App-Updates through Distribution. Instead, use the [CodePush service](https://microsoft.github.io/code-push/) to distribute application updates without creating a new build.

## Xamarin

1. Crashes caused by `StackOverflowException` aren't reported. This is a limitation of the Xamarin runtime.
2. You can't use most AppCenter SDK APIs in a Xamarin Forms application constructor if you want to use the previewer. The Xamarin previewer doesn't support native code.
3. You can't deploy an application using the AppCenter SDK to the Xamarin Live Player as the SDK contains native code.

## UWP, WinUI, WPF, and WinForms

1. Crashes caused by `StackOverflowException` aren't reported. This is a limitation of the .NET runtime.
2. Only crashes caused by .NET exceptions are reported, not C/C++ crashes.
