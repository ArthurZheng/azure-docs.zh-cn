---
title: 使用 PowerShell 管理 Azure 独立云中的数据
titleSuffix: Azure Storage
description: 使用 Azure PowerShell 管理中国云、政府云和德国云中的存储。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: b610a5537d110a4046bd42ac86f5c938aeafe953
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "89072939"
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>使用 PowerShell 管理 Azure 独立云中的存储

大多数人为其全球 Azure 部署使用了 Azure 公有云。 但出于主权等方面的原因，还存在一些独立的 Microsoft Azure 部署。 这些独立部署称为“环境”。 以下列表详细说明了当前存在的独立云。

* [Azure 政府云](https://azure.microsoft.com/features/gov/)
* [由中国世纪互联运营的 Azure 中国世纪互联云](http://www.windowsazure.cn/)
* [Azure 德国云](../../germany/germany-welcome.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="using-an-independent-cloud"></a>使用独立云

若要在某个独立云中使用 Azure 存储，需要连接到该云而不是 Azure 公有云。 若要使用某个独立云而不是 Azure 公有云，需要：

* 指定要连接到的环境。
* 确定并使用可用的区域。
* 使用正确的终结点后缀，它不同于 Azure 公有云。

本文中的示例需要 Azure PowerShell 模块 Az 版本 0.7 或更高版本。 在 PowerShell 窗口中，运行 `Get-Module -ListAvailable Az` 可查找版本。 如果未列出任何信息或需要升级，请参阅[安装 Azure PowerShell 模块](/powershell/azure/install-Az-ps)。

## <a name="log-in-to-azure"></a>登录 Azure

运行 [Get-AzEnvironment](/powershell/module/az.accounts/get-azenvironment) cmdlet 以查看可用的 Azure 环境：

```powershell
Get-AzEnvironment
```

登录到有权访问所要连接的云的帐户，并设置环境。 此示例演示如何登录到使用 Azure 政府云的帐户。   

```powershell
Connect-AzAccount –Environment AzureUSGovernment
```

若要访问中国云，请使用环境 **AzureChinaCloud**。 若要访问德国云，请使用 **AzureGermanCloud**。

此时，如果需要查看可在其中创建存储帐户或其他资源的位置列表，可以使用 [Get-AzLocation](/powershell/module/az.resources/get-azlocation) 查询所选云可用的位置。

```powershell
Get-AzLocation | select Location, DisplayName
```

下表显示了针对德国云返回的位置。

|位置 | 显示名称 |
|----|----|
| `germanycentral` | 德国中部|
| `germanynortheast` | 德国东北部 |


## <a name="endpoint-suffix"></a>终结点后缀

其中每个环境的终结点后缀不同于 Azure 公有云终结点。 例如，Azure 公有云的 Blob 终结点后缀为 **blob.core.windows.net**。 政府云的 Blob 终结点后缀为 **blob.core.usgovcloudapi.net**。

### <a name="get-endpoint-using-get-azenvironment"></a>使用 Get-AzEnvironment 获取终结点

使用 [Get-AzEnvironment](/powershell/module/az.accounts/get-azenvironment) 检索终结点后缀。 终结点是环境的 *StorageEndpointSuffix* 属性。

下面的代码片段演示如何检索终结点后缀。 所有这些命令返回类似于 "core.cloudapp.net" 或 "core.cloudapi.de" 等的内容。将后缀追加到存储服务以访问该服务。 例如，追加“queue.core.cloudapi.de”可访问德国云中的队列服务。

此代码片段检索所有环境，以及每个环境的终结点后缀。

```powershell
Get-AzEnvironment | select Name, StorageEndpointSuffix 
```

此命令返回以下结果。

| 名称| core.usgovcloudapi.net|
|----|----|
| AzureChinaCloud | core.chinacloudapi.cn|
| AzureCloud | core.windows.net |
| AzureGermanCloud | core.cloudapi.de|
| AzureUSGovernment | core.usgovcloudapi.net |

若要检索指定环境的所有属性，请调用 Get-AzEnvironment 并指定云名称。 此代码片段返回属性列表；请在列表中查找 **StorageEndpointSuffix**。 以下示例适用于德国云。

```powershell
Get-AzEnvironment -Name AzureGermanCloud
```

结果类似于以下值：

|属性名称|Value|
|----|----|
| 名称 | `AzureGermanCloud` |
| EnableAdfsAuthentication | `False` |
| ActiveDirectoryServiceEndpointResourceI | `http://management.core.cloudapi.de/` |
| GalleryURL | `https://gallery.cloudapi.de/` |
| ManagementPortalUrl | `https://portal.microsoftazure.de/` |
| ServiceManagementUrl | `https://manage.core.cloudapi.de/` |
| PublishSettingsFileUrl| `https://manage.microsoftazure.de/publishsettings/index` |
| ResourceManagerUrl | `http://management.microsoftazure.de/` |
| SqlDatabaseDnsSuffix | `.database.cloudapi.de` |
| **StorageEndpointSuffix** | `core.cloudapi.de` |
| ... | ... |

若只要检索存储终结点后缀属性，请检索特定的云，并仅请求该属性。

```powershell
$environment = Get-AzEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix
```

此命令返回以下信息：

`Storage Endpoint Suffix = core.cloudapi.de`

### <a name="get-endpoint-from-a-storage-account"></a>从存储帐户获取终结点

还可以通过检查存储帐户的属性来检索终结点：

```powershell
# Get a reference to the storage account.
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"
$storageAccount = Get-AzStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
  # Output the endpoints.
Write-Host "blob endpoint = " $storageAccount.PrimaryEndPoints.Blob 
Write-Host "file endpoint = " $storageAccount.PrimaryEndPoints.File
Write-Host "queue endpoint = " $storageAccount.PrimaryEndPoints.Queue
Write-Host "table endpoint = " $storageAccount.PrimaryEndPoints.Table
```

对于政府云中的存储帐户，此命令返回以下输出：

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>设置环境之后

现在可以使用 PowerShell 来管理存储帐户并访问 blob、队列、文件和表数据。 有关详细信息，请参阅 [Az.Storage](/powershell/module/az.storage)。

## <a name="clean-up-resources"></a>清理资源

如果为本练习创建了新的资源组和存储帐户，可以通过删除资源组来删除这两个资产。 删除资源组会删除其包含的所有资源。

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>后续步骤

* [在不同的 PowerShell 会话中保留用户登录](/powershell/azure/context-persistence)
* [Azure 政府版存储](../../azure-government/documentation-government-services-storage.md)
* [Microsoft Azure Government 开发人员指南](../../azure-government/documentation-government-developer-guide.md)
* [Azure 中国世纪互联应用程序的开发人员说明](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Azure 德国版文档](../../germany/germany-welcome.md)
