---
# required metadata
title: CLI exit codes
description: Troubleshooting App Center Test
keywords: test, troubleshooting
author: lucen-ms
ms.author: lucen
ms.date: 02/07/2019
ms.topic: article
ms.service: vs-appcenter
ms.assetid: 6a1da1b3-66d8-4088-9544-76877a8b4410
---

# CLI exit codes
[!INCLUDE [Retirement announcement for App Center](../../includes/retirement.md)]

## Exit codes
The App Center CLI tool returns an exit code that reflects whether App Center Test received the necessary information, or if it triggered an error. In this table, we list the exit codes with the corresponding explanation.

| Exit code | Explanation |
| --------- | ----------- |
|  0 | Success |
|  1 | Unknown Error |
|  2 | Invalid Options |
|  3 | App File Not Found |
|  4 | Assembly Directory Not Found |
|  5 | NUnit Not Found |
|  6 | UITest Not Found |
|  7 | Test Assemblies Not Found |
|  8 | Sign Info File Not Found |
|  9 | Key Store Not Found |
| 10 | dSym Not Found Or Not Directory |
| 11 | dSym Directory Wrong Extension |
| 12 | dSym Contains More than One Dwarf |
| 13 | Test Chunking Failed |
| 14 | Upload Negotiation Failed |
| 15 | Upload Failed |
| 16 | Job Status Retrieval Failed |
| 17 | NUnit Report Not Returned |
| 18 | Job Failed |
| 19 | Test Failures |
| 20 | Incompatible Versions |
| 21 | No Tests Would Run |
| 22 | Data File Not Contained In Assembly Directory |
| 23 | Data File Not Found |
| 24 | Did Not Pass Validation |
| 25 | Cancelled |
| 26 | NUnit Version Unsupported |
| 27 | Artifacts Directory Invalid |

## Getting help
You can always contact us through [the ? > Contact Support icon in the upper-right hand corner](~/help.md). We don't provide 24/7 support, but will strive to reply as fast as possible.

To help debug your test run, you can navigate to the test run in question and copy the URL from your browser and paste it into the support conversation. A test run URL looks like something like 
> https://appcenter.ms/orgs/OrgName/apps/App-Name/test/runs/77a1c67e-2cfb-4bbd-a75a-eb2b4fd0a747.