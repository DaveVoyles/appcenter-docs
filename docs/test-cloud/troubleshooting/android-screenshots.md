---
title: Android screenshots
description: Troubleshooting App Center Test Reports show blank or scrambled screenshots on Android
keywords: test cloud, android, screenshots
author: lucen-ms
ms.author: lucen
ms.date: 04/08/2020
ms.topic: article
ms.assetid: 83a77f81-8274-4b54-8c67-d7fce668d8cd
ms.service: vs-appcenter
ms.custom: test
---

# Android screenshots
If your App Center Test Reports consistently show blank or scrambled screenshots on Android, the typical cause is that your application has disabled screenshots. This Android setting is frequently used in applications that handle sensitive data like finance or medical information.

This setting is controlled by the constant `FLAG_SECURE` in `WindowManager.LayoutParams` (`Android.Views.WindowManagerFlags.Secure` in Xamarin.Android):

 >Window flag: treat the content of the window as secure, preventing it from appearing in screenshots or from being viewed on non-secure displays.

See [WindowManager.LayoutParams FLAG_SECURE](https://developer.android.com/reference/android/view/WindowManager.LayoutParams#FLAG_SECURE). For Xamarin.Android, also see [Android.Views.WindowManagerFlags.Secure](/dotnet/api/android.views.windowmanagerflags).

`FLAG_SECURE` is handled differently from device to device and between different Android OS versions. Some devices or older Android versions will still allow screenshots, others show blank screens, and still others show scrambled screenshots.

If your app has disabled screenshots, then seeing blank or scrambled screenshots in the test report demonstrates this security feature is working as intended for your app.

If your app has disabled screenshots by setting the flag `FLAG_SECURE` in the `WindowManager.LayoutParams` but you want to see the screenshots in your test reports, then the workaround in App Center Test is to create a testing build that doesn't set the `FLAG_SECURE` flag. You can search your code for `FLAG_SECURE` or `WindowManagerFlags.Secure` (Xamarin.Android) to see where and how this value is set.

## Getting help
You can contact support in the App Center portal. In the upper right corner of screen, select the Help (?) menu, then choose 'Contact support'. Our dedicated support team will respond to your questions.

If you want help with a test run, navigate to the test run in question and copy the URL from your browser and paste it into the support conversation. A test run URL looks like something like `https://appcenter.ms/orgs/OrgName/apps/App-Name/test/runs/77a1c67e-2cfb-4bbd-a75a-eb2b4fd0a747`.
