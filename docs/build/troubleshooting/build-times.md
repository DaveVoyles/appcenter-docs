---
title: Understanding Build Execution Time
description: Comparing local build times to App Center
keywords: build, faq
author: lucen-ms
ms.author: lucen
ms.date: 07/31/2020
ms.topic: article
ms.assetid: bcb76094-34c6-48ae-a24d-60c14a261518
ms.service: vs-appcenter
ms.custom: build
---

# Understanding Build Execution Time

[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

There are many reasons why build duration can be higher when using App Center Build:

* When running your build locally, many things are cached, including NuGet packages, pods, dependencies, and so on. In App Center, we always do a clean build, and also redownload dependencies not already included in your repository.
* [Build configuration differences](~/build/troubleshooting/build-failed.md#if-building-works-locally-but-not-in-app-center) between your local build compared to App Center. For example, a signed device build typically takes longer than a simulator build. 
* The CPU power of your development machine may be higher than the CPU of our VMs.
* More builds queued then the build concurrency for your organization.
* If your build includes other production tasks, like running tests or publishing binaries, those can also increase the build time.

## Why do I get an extended build time when **Run launch test on a device** is enabled?
We run the test as part of the build operation, which gives the added build time. While App Center Test validates your app, extra tasks occur like signing, checking permissions, and so on. After that it's time to wait for a device. Once started, it runs the app on a real device, which takes time. And lastly, we move test logs, screenshots into the cloud.

Expect an additional **10 minutes of build time**.