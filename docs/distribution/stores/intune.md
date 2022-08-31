---
title: Publish applications to Intune Company Portal
description: Simplify distribution of line-of-business mobile applications to the Company Portal on Intune
keywords: distribution store
author: lucen-ms
ms.author: lucen
ms.date: 04/08/2020
ms.topic: article
ms.service: vs-appcenter
ms.custom: distribute
ms.assetid: 7360e04f-01af-4c3f-ac0a-75c2dab979ba
---

# Intune Company Portal Distribution
Publish new and upgrade existing line-of-business (LOB) iOS and Android applications from App Center into Company Portal on your Intune Cloud instance.

## Prerequisites
* Your organization has an Intune Cloud subscription. If not, you could sign up for a [trial account](/intune/free-trial-sign-up).
* An Intune admin and tenant account activated. 
* Create a [DNS CNAME](/intune/custom-domain-name-configure) to simplify and customize the logon experience. When your organization signs up for a Microsoft cloud-based service like Intune, you're given an initial domain.
* Manually [add users](/intune/users-add) to your Intune subscription via the [Office 365 portal](https://www.office.com/signin) or the [Azure Intune portal](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) and give users additional permissions including global administrator permission.
* Create [groups](/intune/groups-add) to organize users or devices by geographic location, department, or hardware characteristics.
* Azure Active Directory for a tenant acts as a security broker to enable access to sites. [Visual Studio App Center](https://appcenter.ms) will require access to the Intune graph API to enable a particular tenant's user to publish apps to the Intune Company Portal. The tenants Azure Active Directory (AD) global admin needs to give consent. To provide this consent, the global admin of your company's Azure Active Directory global admin will need to go to this [consent page](https://login.microsoftonline.com/common/adminconsent?client_id=9aa18e05-1deb-4254-98a8-fab3591a3ad3&redirect_uri=https://appcenter.ms) and agree to grant access for Visual Studio App Center to access the Intune Graph APIs. The admin will then be navigated to [Visual Studio App Center](https://appcenter.ms). Providing consent is a one-time activity for a company's Active Directory global admin. Once completed, any user with the Intune app manager role, for this company, can navigate to [Visual Studio App Center](https://appcenter.ms) and publish apps to the Intune Company Portal.
* For more information, review the [introduction to Intune](/intune/introduction-intune).


## Step 1: Create an Intune connection in App Center

1. Select **Stores** under Distribution.
2. In the middle of the page, click on the **Connect to Store** button.
3. Select the store type as Intune Company Portal and click **Next**.
4. You'll be redirected to Microsoft login page, to provide your initial domain name in Azure Active Directory (AD). It looks like **your-domain.onmicrosoft.com**.
5. On successful login, you'll be asked to enter a **Store name**.
6. **Select Category** of the app from the drop-down.
7. For **Audience**, provide the Azure AD group (security enabled group) name to distribute this application to. The early version of this feature requires the developer to know the Azure AD group that the app is being distributed to. The Intune administrator that connected App Center to the Intune Graph API in step 1.3 must be a member of the group for it to be eligible and appear in the autocomplete drop-down.
8. Click **Connect**.
9. A connection to Intune Company Portal has now been set up. You should see a store with the name provided on the **Stores** home page. The connection is valid for 90 days for an application in App Center. 


## Step 2: Publish your application to the Intune Company Portal

1. From the **Stores** home page, select the Intune Store created above.
2. Click on **Publish to Store**.
3. Upload the app package.
4. Once the package has been uploaded, you should see some details of the application. Click **Next**.
5. Click on **Publish** to push the app to the **Intune Company Portal**. The status for this release on the store details page will show as **Processing**.
6. Once App Center has completed the hand-over of the app to Intune, the status of the app will change to **Submitted**.
7. If there's a failure while publishing, the status is **failed** with an error message.

For any issues, you can always open a support ticket. Select the help menu (?) in the upper right corner of App Center portal, then choose 'Contact support'. Our dedicated support team will respond to your questions and feedback. 


## Publishing through the CLI

> [!WARNING]
> You need App Center CLI 2.7 or newer to use this feature.

Using the CLI is an easy way to integrate the App Center's store connection as part of your CI/CD setup like Jenkins or Go CI.

Before you can use the CLI, you'll need to establish a connection to a destination, that is, Google Play, App Store, or Intune in the App Center. And compile a binary for your destination.

```bash
appcenter distribute stores list \
--app {app_owner}/{app_name} \
--output json
```

You'll get a result like this:

```bash
[["Alpha","googleplay","alpha"],["Beta","googleplay","beta"],["Production","googleplay","production"]
```

And it's the Store column we'll use in the final step.

The final step is to publish your app by running:

```bash
appcenter distribute stores publish \
--file /path/to/file.aab \
--store Production \
--app {app_owner}/{app_name} \
--release-notes "Some note."
```

You'll need to fill in the blanks like the list command. Instead of having a static release note, it's possible to use the `--release-notes-file` instead. A release note file is plain text file encoded with UTF-8.

## Troubleshooting

### Failed to publish app to Intune Store. The app was previously published as ***

This error can happen when you try to publish using an app you've already published to Intune with a different app name.  App Center uses the name of the app in App Center when publishing to Intune. If the name is different from what you have in Intune, Intune rejects with that error message.

To fix this problem, you need to rename or create a new app that exactly matches what you have in Intune. If you see the error message, try to rename your App Center app to what follows after ...published as '***'.
