---
title: Build Scripts
description: Details on how to write build scripts to enhance the build process
keywords: build, pre, post, custom, scripts
author: lucen-ms
ms.author: lucen
ms.date: 09/01/2020
ms.topic: article
ms.assetid: 3603ae8a-8adc-49df-9eee-f078008691af
ms.service: vs-appcenter
ms.custom: build
---

# Build scripts

[!INCLUDE [Retirement announcement for App Center](../../../includes/retirement.md)]


> [!NOTE]
> Build scripts allow you to customize your builds; but issues you find during that process are beyond the scope of App Center support. If one of your scripts is failing, the whole build will report a failure, even if the primary build up to that point were successful.

You can add custom build scripts that run at pre-defined stages during build time: [post-clone](~/build/custom/scripts/index.md#post-clone), [pre-build](~/build/custom/scripts/index.md#pre-build), [post-build](~/build/custom/scripts/index.md#post-build). Place the scripts with the format specified below next to the project-level (`.xcodeproj`, `.csproj`, `.sln`, or `package.json`) file or module-level (`build.gradle`) file that you've selected in the build configuration and we'll run them as custom build steps. 

In App Center, non-UWP apps are built on macOS, so they can use Bash scripts. UWP apps are built on Windows, so they can use PowerShell scripts. Many similar tasks can be done in either script format, but you might need to do some conversion if migrating between them. For example, macOS Bash requires the use of UNIX-style line endings (LF).

You can find a collection of build script examples on the [dedicated public GitHub repository](https://github.com/Microsoft/appcenter/tree/master/sample-build-scripts). Feel free to use the sample scripts, modify them, or submit new pull requests with your most useful scripts for the rest of the community to use.

> [!WARNING]
> When App Center detects build scripts for the first time, or you change the location of scripts, or you change where CocoaPods are stored; you must apply the changes by clicking the **Save** or **Save & Build** button in the build configuration. When you do this, App Center runs an analysis to index your repository tree and updates the build definition.

![Save detected scripts](~/build/custom/scripts/images/build-scripts-detected.png "Build configuration shows detected build scripts")

## Post-clone
The post-clone script runs immediately after the repository was cloned but before we do anything else on our end.

To run scripts post-clone, add the following file next to the project file in your repository:

- **appcenter-post-clone.sh** (Bash for iOS & Android)

    ```shell
    #!/usr/bin/env bash

    # Example: Clone a required repository
    git clone https://github.com/example/SomeProject

    # Example: Install App Center CLI
    npm install -g appcenter-cli
    ```

- **appcenter-post-clone.ps1** (PowerShell for UWP)

## Pre-build
The pre-build script runs before the actual build starts. For React Native apps, the script runs before generating source maps. For other platforms, it runs after we've installed dependencies, for example from NuGet, CocoaPods, or Carthage.

To run scripts pre-build, add the following file next to the project file in your repository:

- **appcenter-pre-build.sh** (Bash for iOS & Android)

    ```shell
    #!/usr/bin/env bash

    # Example: Change bundle name of an iOS app for non-production
    if [ "$APPCENTER_BRANCH" != "main" ];
    then
        plutil -replace CFBundleDisplayName -string "\$(PRODUCT_NAME) Beta" $APPCENTER_SOURCE_DIRECTORY/MyApp/Info.plist
    fi
    ```

- **appcenter-pre-build.ps1** (PowerShell for UWP)

## Post-build
The post-build script runs after the build has finished and copied all the necessary artifacts to the output directory. The post-build script will run even if the build fails.

To run scripts post-build, add the following file next to the project file in the repository:

- **appcenter-post-build.sh** (Bash for iOS & Android)

    ```shell
    if [ "$AGENT_JOBSTATUS" == "Succeeded" ]; then
        HOCKEYAPP_API_TOKEN={API_Token}
        HOCKEYAPP_APP_ID={APP_ID}

        # Example: Upload main branch app binary to HockeyApp using the API
        if [ "$APPCENTER_BRANCH" == "main" ];
         then
            curl \
            -F "status=2" \
            -F "ipa=@$APPCENTER_OUTPUT_DIRECTORY/MyApps.ipa" \
            -H "X-HockeyAppToken: $HOCKEYAPP_API_TOKEN" \
            https://rink.hockeyapp.net/api/2/apps/$HOCKEYAPP_APP_ID/app_versions/upload
        else
            echo "Current branch is $APPCENTER_BRANCH"
        fi
    fi
    ```

- **appcenter-post-build.ps1** (PowerShell for UWP)

## Environment Variables
You can use pre-defined, custom, or in-script environment variables to help write your Build scripts, see our [Environment Variables guide](~/build/custom/variables/index.md) for more info. 
