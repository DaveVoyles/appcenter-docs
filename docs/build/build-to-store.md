---
title: Distributing a build to the store
description: How to distribute your app to the store
keywords: build, ios, android, store, distribute
author: lucen-ms
ms.author: lucen
ms.date: 07/31/2020
ms.topic: article
ms.assetid: 2684ff69-d7e8-4215-abd5-fe830bf75ead
ms.service: vs-appcenter
ms.custom: build
---

# Distributing a build to the store

[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]

In this document, you'll learn how to distribute your build to a store. You can get your completed app to the store directly from App Center.

## Automatically distribute from your branch configuration
First, the branch needs to be set up before it's ready to automatically distribute to the store. You can read more about configuring your branch in the "configure build" articles respectively for [Android](~/build/android/index.md) and [iOS](~/build/ios/index.md).

Use the branch configuration to define the target store for the build. If you haven't already connected to a store, you'll see the ability to do so from the drop-down. Follow the instructions to set up a [store connection](~/distribution/stores/index.md).

![An image showing how to connect to a store using the drop-down](~/build/images/distribute-to-store.png)

With the store connected, choose the store you want to automatically distribute successful builds to. Ensure you've signed the app with the correct signing credentials to distribute to a store. After you pick the store you want to distribute to, click **Save** or **Save and Build**. Your app will be distributed to the selected store on each successful build!

> [!TIP]
> The recommended workflow is configuring a branch to distribute to the store with the correct signing credentials and configuration. Merge to and build this branch only when you want to distribute to a store.
