---
title: Code Repository Troubleshooting
description: Troubleshooting issues with code repositories in Build
keywords: build, troubleshooting
author: lucen-ms
ms.author: lucen
ms.date: 10/09/2020
ms.topic: article
ms.assetid: 860b2438-d3e9-4f0c-bf92-cccb98fdd29b
ms.service: vs-appcenter
ms.custom: build
---

# Code Repository Troubleshooting
## Repository isn't listed in the Connect to Repository step
* **Only Git repositories are supported**. Other repository types like Mercurial, TFVC, and so on, won't show up in the repository list.
* **Connecting to a user-owned repository must be done by the owner**. If you aren't the owner, you can still onboard the app by inviting the repository owner as a collaborator in App Center. Then, the owner can connect the repository as a source to the App Center build service.
* **Connecting to a repository owned by an organization on GitHub or team on Bitbucket requires admin access to the repository**. This access is required because App Center will register a webhook on the repository coming from the appcenter.ms domain.
* **For GitHub repositories, an organization member with owner privileges might need to approve the initial access request**. See GitHub's instructions on [approving third-party apps for your organization](https://help.github.com/articles/approving-third-party-applications-for-your-organization/). It can take up to a few minutes until repositories from that organization will show up in App Center.

## When configuring a branch, I get an error message saying "no projects can be found" in my branch
> [!TIP]
> The Build service has a 30-second time limit for crawling the repository. Large or complex projects might not be indexed within this time limit. In this case, if it's not feasible to host the target project separately, you may consider using [Azure Pipelines](~/build/choose-between-services.md) instead.

App Center analyzes the contents of the branch in your repository to find an app project matching the platform selected for your app in App Center. This assumes your project uses the platform-specific standards for configuration, that is, an Xcode project or workspace for iOS apps, a Gradle project for Android apps and a solution or project for your Xamarin apps.

App Center currently only searches four directory levels deep for your project files. If App Center doesn't find your project in your branch, moving it to the root directory might help. If your repository is large, it may help to reduce its size or number of files.

## Can I use on-premises repositories?
App Center currently supports cloud hosted Git repositories on Azure DevOps (formerly known as Visual Studio Team Services (VSTS)), Bitbucket and GitHub, but doesn't support on-premises repositories.

## Repository connection broken after...
### Repo name change
This breaks the connection. To resolve the issue, revert to the original name or create a new repo with the original name.

### Account ownership change
If the user who originally connected the repository has their permission to access that repository revoked because of being removed from App Center or the repository ownership, then generally reconnecting the repository requires removing the old connection and creating it again. 

> [!WARNING]
> Disconnecting your repository will delete configurations and artifacts stored in App Center Build. You can [manually backup your data](~/build/troubleshooting/backup-data.md) before disconnecting to preserve it.

## I've connected to the wrong account. How can I reconnect to the correct one?
Disconnect the repository account, and reconnect to the correct one:
- [BitBucket](~/build/connect.md#bitbucket)
- [GitHub](~/build/connect.md#github)
- [Azure DevOps](~/build/connect.md#azure-devops)

## When connecting an Azure DevOps repository, I see "No Projects Found"
It's possible when you first attempt to connect App Center to Azure DevOps (formerly VSTS) that it gets authorized under the wrong organization. Determine [which Azure DevOps accounts are for an organization](https://app.vsaex.visualstudio.com/me) and ensure yours is listed. When you attempt to complete the authentication flow in Azure DevOps, verify the message reads **App requests the following permissions from email@example.com (org)**.

Another possibility is that OAuth is disabled under Security Policies settings. Follow [this guidance](/azure/devops/organizations/accounts/change-application-access-policies) to enable it.

## Are Git submodules supported?
For repositories hosted on GitHub and Azure DevOps, Git submodules over HTTPS are supported using the same authentication as the parent repository. 
For repositories hosted on Bitbucket, only unauthenticated Git submodules are supported for now.

## I'm connected to Bitbucket and my builds fail with Git error
If your builds fail with an error similar to

```Text
"fatal: reference is not a tree:..
##[error]Git checkout failed with exit code: 128",
```

try the following steps:

* Go to the Webhooks section of the connected Bitbucket account and look for any disabled Webhooks from **appcenter.ms** & **mobile.azure.com**
* Re-enable the Webhooks
* Commit a change to the affected repository/branch and try a new build

Contact [App Center support](/appcenter/general/support-center.md#contact-us) if the issue persists.

## What can I do if I'm using Team Foundation Version Control (TFVC)?
App Center Build only supports Git repositories. For projects using a TFVC repository, you can [convert them to Git](/vsts/git/import-from-tfvc), or you can use Visual Studio Team Services for continuous integration, and take advantage of the [Hosted macOS Preview](/vsts/build-release/apps/mobile/xcode-ios?tabs=vsts) agents and build tasks for App Center Distribution and Test.

## Is my source code secure?
App Center uses virtual machines to build your code. There's a clean virtual machine provisioned especially for your build, which gets discarded once the build finishes. The files uploaded for code signing and the password for the certificate are also stored securely on our servers.

For more information, you can read our [App Center Security doc](~/general/app-center-security.md).
