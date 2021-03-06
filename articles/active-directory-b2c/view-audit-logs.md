---
title: 访问和查看审核日志
titleSuffix: Azure AD B2C
description: 如何在 Azure 门户中以编程方式访问 Azure AD B2C 审核日志。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 02/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 4fc25edb873a2dfe84f6ca716a71cf028c74cb2f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "85383931"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>访问 Azure AD B2C 审核日志

Azure Active Directory B2C (Azure AD B2C) 发出审核日志，其中包含有关 B2C 资源、颁发的令牌和管理员访问权限的活动信息。 本文简要概述了审核日志中提供的信息，并介绍了如何访问 Azure AD B2C 租户的此数据。

审核日志事件只保留 **7 天**。 如果需要保留更长时间，请使用下面所示的方法计划下载并存储日志。

> [!NOTE]
> 无法在 Azure 门户中“Azure Active Directory”或“Azure AD B2C”页的“用户”部分查看各个 Azure AD B2C 应用程序的用户登录。************ 该处的登录事件会显示用户活动，但不能回过头来将其与用户登录到的 B2C 应用程序相关联。 必须使用其审核日志，这一点会在本文中进一步阐述。

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>审核日志的 B2C 类别中提供的活动的概述

审核日志中的“B2C”类别包含以下类型的活动****：

|活动类型 |说明  |
|---------|---------|
|授权 |涉及授权用户访问 B2C 资源（例如，管理员访问 B2C 策略列表）的活动。         |
|Directory |与管理员使用 Azure 门户登录时检索到的目录属性相关的活动。 |
|应用程序 | 与 B2C 应用程序相关的创建、读取、更新和删除 (CRUD) 操作。 |
|键 |与 B2C 密钥容器中存储的密钥相关的 CRUD 操作。 |
|资源 |与 B2C 资源相关的 CRUD 操作。 例如，策略和标识提供者。
|身份验证 |用户凭据和令牌颁发的验证。|

有关用户对象 CRUD 活动，请参阅“核心目录”类别****。

## <a name="example-activity"></a>示例活动

Azure 门户中的此示例图像显示用户使用外部标识提供者（在本例中为 Facebook）登录时捕获的数据：

![Azure 门户中“审核日志活动详细信息”页的示例](./media/view-audit-logs/audit-logs-example.png)

活动详细信息面板包含以下相关信息：

|部分|字段|说明|
|-------|-----|-----------|
| 活动 | 名称 | 发生了哪项活动。 例如，“向应用程序颁发 id_token”（这将结束实际的用户登录）。** |
| 发起者（参与者） | ObjectId | 用户登录的 B2C 应用程序的**对象 ID**。 此标识符在 Azure 门户中不可见，但可以通过 Microsoft Graph API 访问它。 |
| 发起者（参与者） | SPN | 用户登录的 B2C 应用程序的**应用程序 ID**。 |
| 目标 | ObjectId | 正在登录的用户的**对象 ID**。 |
| 其他详细信息 | TenantId | Azure AD B2C 租户的**租户 ID**。 |
| 其他详细信息 | PolicyId | 用于登录用户的用户流（策略）的**策略 ID**。 |
| 其他详细信息 | ApplicationId | 用户登录的 B2C 应用程序的**应用程序 ID**。 |

## <a name="view-audit-logs-in-the-azure-portal"></a>在 Azure 门户中查看审核日志

在 Azure 门户中可以访问 Azure AD B2C 租户中的审核日志事件。

1. 登录到 [Azure 门户](https://portal.azure.com)
1. 切换到包含你的 Azure AD B2C 租户的目录，然后浏览到“Azure AD B2C”。****
1. 在左侧菜单中的“活动”下，选择“审核日志”。********

此时会显示过去七天内记录的活动事件列表。

![在 Azure 门户中使用筛选器后显示的两个活动事件示例](./media/view-audit-logs/audit-logs-example-filter.png)

可以使用多个筛选选项，包括：

* **活动资源类型** - 根据[可用活动概述](#overview-of-activities-available-in-the-b2c-category-of-audit-logs)部分的表格中所示的活动类型进行筛选。
* **日期** - 筛选显示的活动的日期范围。

如果在列表中选择某一行，将显示该事件的活动详细信息。

若要以逗号分隔值 (CSV) 文件格式下载活动事件列表，请选择“下载”。****

## <a name="get-audit-logs-with-the-azure-ad-reporting-api"></a>使用 Azure AD 报告 API 获取审核日志

审核日志将发布到与 Azure Active Directory 其他活动相同的管道，因此可通过 [Azure Active Directory 报告 API](https://docs.microsoft.com/graph/api/directoryaudit-list)进行访问。 有关详细信息，请参阅 [Azure Active Directory 报告 API 入门](../active-directory/reports-monitoring/concept-reporting-api.md)。

### <a name="enable-reporting-api-access"></a>启用报告 API 访问

若要允许对 Azure AD 报告 API 进行基于脚本或应用程序的访问，需要使用以下 API 权限在 Azure AD B2C 租户中注册的应用程序。 你可以对 B2C 租户中的现有应用程序注册启用这些权限，或者创建专用于审核日志自动化的新权限。

* Microsoft Graph > 应用程序权限 > 审核日志 > 审核日志

按照以下文章中的步骤操作，以注册具有所需权限的应用程序：

[使用 Microsoft Graph 管理 Azure AD B2C](microsoft-graph-get-started.md)

使用适当的权限注册应用程序后，请参阅本文后面的 "PowerShell 脚本" 一节，了解如何使用脚本获取活动事件的示例。

### <a name="access-the-api"></a>访问 API

若要通过 API 下载 Azure AD B2C 审核日志事件，请按 `B2C` 类别筛选日志。 若要按类别筛选，请在调用 Azure AD 报告 API 终结点时使用 `filter` 查询字符串参数。

```http
https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?$filter=loggedByService eq 'B2C' and activityDateTime gt 2019-09-10T02:28:17Z
```

### <a name="powershell-script"></a>PowerShell 脚本

以下 PowerShell 脚本通过一个示例演示如何查询 Azure AD 报告 API。 查询 API 后，该脚本将以标准输出的形式列显记录的事件，然后将 JSON 输出写入到某个文件。

可以在 [Azure Cloud Shell](overview.md)中尝试此脚本。 请务必使用自己的应用程序 ID、客户端密码和 Azure AD B2C 租户名称更新此脚本。

```powershell
# This script requires an application registration that's granted Microsoft Graph API permission
# https://docs.microsoft.com/azure/active-directory-b2c/microsoft-graph-get-started

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's client ID, a GUID
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's client secret
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # Insert your Azure AD B2C tenant domain name

$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.microsoft.com"           # Microsoft Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 through n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = "https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?`$filter=loggedByService eq 'B2C' and activityDateTime gt  " + $7daysago

    # loop through each query page (1 through n)
    Do {
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```

下面是本文前面所示的示例活动事件的 JSON 表示形式：

```json
{
    "id": "B2C_DQO3J_4984536",
    "category": "Authentication",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "result": "success",
    "resultReason": "N/A",
    "activityDisplayName": "Issue an id_token to the application",
    "activityDateTime": "2019-09-14T18:13:17.0618117Z",
    "loggedByService": "B2C",
    "operationType": "",
    "initiatedBy": {
        "user": null,
        "app": {
            "appId": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "servicePrincipalId": null,
            "servicePrincipalName": "00000000-0000-0000-0000-000000000000"
        }
    },
    "targetResources": [
        {
            "id": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "type": "User",
            "userPrincipalName": null,
            "groupType": null,
            "modifiedProperties": []
        }
    ],
    "additionalDetails": [
        {
            "key": "TenantId",
            "value": "test.onmicrosoft.com"
        },
        {
            "key": "PolicyId",
            "value": "B2C_1A_signup_signin"
        },
        {
            "key": "ApplicationId",
            "value": "00000000-0000-0000-0000-000000000000"
        },
        {
            "key": "Client",
            "value": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
        },
        {
            "key": "IdentityProviderName",
            "value": "facebook"
        },
        {
            "key": "IdentityProviderApplicationId",
            "value": "0000000000000000"
        },
        {
            "key": "ClientIpAddress",
            "value": "127.0.0.1"
        }
    ]
}
```

## <a name="next-steps"></a>后续步骤

你可以自动执行其他管理任务，例如， [通过 Microsoft Graph 管理 Azure AD B2C 用户帐户](manage-user-accounts-graph-api.md)。
