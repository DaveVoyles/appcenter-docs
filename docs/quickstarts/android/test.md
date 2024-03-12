---
title: Run Included Tests with App Center
description: Tutorial to run a test using Android sample app with App Center.
keywords: app center
author: lucen-ms
ms.author: lucen
ms.topic: article
ms.service: vs-appcenter
ms.date: 07/26/2017
ms.assetid: 7d2aa750-c7d0-47cb-acc8-97892bc78cf2
---

# Run Included Tests with App Center

[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

### Prerequisite
- Follow the [getting started tutorial](getting-started.md) to set up the sample app.

## Install App Center Command Line Interface

1. Install [Node.js](https://nodejs.org/en/) version 6.3 or later.
2. Open the terminal and run the command below to install the App Center CLI:

   ```shell
   npm install -g appcenter-cli
   ```

3. Run the command below and follow the directions to log in to App Center:

   ```shell
   appcenter login
   ```  

## Build the project and test APK files
1. Open terminal and go to the project directory.
2. Run each of the following commands:

   ```bash
   ./gradlew assembleDebug
   ./gradlew assembleDebugAndroidTest
   ```

## Prepare to run tests in App Center

1. Go to the **Test** service in [App Center](https://appcenter.ms/apps).

2. Click on **Start testing your app**.

3. Select the first device, the **Google Pixel XL**, and press **Select one device** at the bottom.

4. Choose **Espresso** as the Test framework.

5. Click **Next**.  
  

## Run the tests

1. In the **Submit** step, copy the custom command under **Running tests**.

2. Paste it into terminal while in the project directory.

> [!NOTE]
> `--app-path` should point to a mobile app apk and `--build-dir` should point to where the test apk is located at.

1. Navigate into **sampleapp-android/app/build/outputs/apk**.

2. Copy the pathname and replace the placeholder text `pathToEspressoBuildFolder` in the custom command with the pathname text.

3. Copy the pathname to **app-debug-androidTest.apk** and replace the placeholder text `pathToFile.apk` in the custom command with the pathname text.

4. Run the command. You'll see the tests begin to run.

5. In App Center, click **Done** to close the panel.  
