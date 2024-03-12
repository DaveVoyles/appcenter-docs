---
title: App Center bug tracker documentation
description: how to integrate App Center with bug trackers like Jira, Visual Studio Team Services (VSTS) and GitHub
keywords: app center, appcenter, visual studio app center, visual studio appcenter, bug tracker, bugtracker, documentation, hockeyapp, VSTS, github
author: lucen-ms
ms.author: lucen
ms.date: 01/06/2020
ms.topic: article
ms.service: vs-appcenter
ms.assetid: f56710ac-9797-4e58-8bd4-9581553c28a1
---

# App Center bug tracker integration

[!INCLUDE [Retirement announcement for App Center](../../includes/retirement.md)]

## Overview
App Center has bug tracker integration for the crashes service. Users can be quickly informed about critical App Center events within the tools that you use regularly in your day to day flow for a seamless experience. App Center supports bug trackers like Jira Cloud, Azure DevOps (formerly Visual Studio Team Services (VSTS)), and GitHub. Users need manager or developer permissions to create and configure the bug tracker.

## Getting Started
1. Navigate to [https://appcenter.ms/apps](https://appcenter.ms/apps), and select the specific app you want for bug tracker integration.

2. In your app, select **Settings**, **Services**, and click the **Add account or service** button.

3. <a name="step3"></a> Select which bug tracker (Jira, VSTS, Azure DevOps, or GitHub) to integrate with. To add a bug tracker for a repository, you need admin permissions for it.

   1. For Jira:

      1. Login with your Jira credentials (email, password and base URL) and click **Add**. See here on [how to find your base URL](https://confluence.atlassian.com/adminjiraserver071/configuring-the-base-url-802593107.html).
      > Note: Jira has [deprecated basic authentication](https://developer.atlassian.com/cloud/jira/platform/deprecation-notice-basic-auth-and-cookie-based-auth/). Instead of typing in your password, create an [API token in Jira](https://confluence.atlassian.com/cloud/api-tokens-938839638.html) and use that as your password.
      2. Select which Jira projects to integrate the bug tracker with and click **Next**.
      3. Toggle **On** for **Auto create ticket** and click **Add**:

   2. For Azure DevOps (formerly VSTS):

      1. Login with your Azure DevOps credentials and click **Accept** when prompted on app authorization.
      2. Select which Azure DevOps projects to integrate the bug tracker with and click **Next**.
      3. Toggle **On** for **Auto create ticket**, and fill in the fields for **Area** and **Default Payload**, and click **Add**:
         - Default payload is an optional field to fill in for use in work items. For example, `{"System.IterationPath": "Area\\Iteration 1", "System.AssignedTo": "Fabrikam"}`. Refer to the [work item types API](/rest/api/azure/devops/wit/work-item-types/) for additional information.

   3. For GitHub:

      1. Login with your GitHub credentials and click **Authorize App Center**.
      2. Select which GitHub repository to integrate the bug tracker with and click **Next**.
      3. Toggle **On** for **Auto create ticket** and click **Add**:

4. Done! Your bug tracker is now configured. With this configuration:

    - When a new crash group is created in App Center, it will trigger the creation of a bug in the configured bug tracker
    - The bug will contain a link to the crash group in App Center 
    - When you [change the state of the crash group](#changeState) in App Center, it changes the state of the bug in the connected bug tracker, and vice versa.

5. Toggle to the extreme right hand of the bug tracker to **Configure** or **Disconnect** it.

   - **Configure** enables you to change the responses to **Auto create ticket**, **Number of crashes**, **Area**, and **Default payload**.
   - **Disconnect** unlinks all the bugs from your crash group.

6. Bug tracker integration is done at the app level, and only one bug tracker can be configured per app. To change bug trackers:

   1. Go to the **Services** page.
   2. Toggle to the extreme right hand of the bug tracker to **Disconnect** it.
   3. Repeat [step 3](#step3).

  > [!NOTE]
  > A new work item will be created for new error groups with symbols provided. If no symbols are found for the new error group, the bug tracker integration won't create a new work item.

## <a name="changeState"></a> Changing the state of a crash group in App Center
1. Navigate to [https://appcenter.ms/apps](https://appcenter.ms/apps), and select the specific app that has a bug tracker integration.

2. In your app, select **Diagnostics**, **Crashes**, and click on the crash group that has a connected bug for it.

3. In the top right corner, click on the **green drop down box** that has either of the states:
   - **Open**
   - **Closed**
   - **Ignored**
 
 4. Choose the state of the crash group. Once you've changed the state of the crash group in App Center, it will update the state in bug. Similarly, if you change the state of the bug in your connected bug tracker, it will update the state in the crash group as well.

