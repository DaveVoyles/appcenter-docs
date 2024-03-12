---
title: Build Sample Android App on App Center
description: Tutorial to build sample Android app in App Center.
keywords: app center
author: lucen-ms
ms.author: lucen
ms.topic: article
ms.service: vs-appcenter
ms.date: 07/27/2017
ms.assetid: faef539f-3bbe-43b5-af8c-7f48a14340ed
---

# Build Sample Android App on App Center

[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

### Prerequisites
- Follow the [getting started tutorial](getting-started.md) to set up the sample app.
- Optional: signed APK. Go to the [Android Developer Documentation](https://developer.android.com/studio/index.html) to learn about creating a new keystore and manually signing an APK.

## Set up the repository
1. Go to the **Build** service in [App Center](https://appcenter.ms/apps).

2. Choose **GitHub** as the selected service.

3. Sign in to GitHub, and click **Authorize AppCenter**.

4. Choose **sampleapp-android**.

## Configure the build

1. Choose the main branch.

2. Click **Configure build**. The Build configuration panel will show up.

3. The default setting for build frequency is **Build this branch on every push**. Every time you push code to a configured branch, a build will automatically begin. You can enable this continuous integration feature or opt to manually choose when builds run.

4. If you want to run the build on a device, turn **Sign builds** on. If you followed the instructions on **Getting Started**, you can upload your keystore file and fill in the environment variables or check **My Gradle settings are entirely set to handle signing automatically**.

5. Click **Save** at the bottom of the page, and the build will begin.

The build should complete after a couple minutes. Once the build has completed, you can download the results by clicking **Download**. This includes logs and, if the build was successful, build output and symbols. 
