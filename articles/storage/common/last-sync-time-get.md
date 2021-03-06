---
title: 检查存储帐户的“上次同步时间”属性
titleSuffix: Azure Storage
description: 了解如何检查异地复制存储帐户的“上次同步时间”属性。 “上次同步时间”属性表示，最近一次将主要区域中的所有写入数据成功写入到次要区域的时间。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 242700c05053aa9d07e3a561a21986c8451a46c7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91612438"
---
# <a name="check-the-last-sync-time-property-for-a-storage-account"></a>检查存储帐户的“上次同步时间”属性

在配置存储帐户时，可以指定将数据复制到距离主要区域数百英里的次要区域。 异地复制在主要区域发生严重中断（例如自然灾害）时为你的数据提供持续性。 如果你额外启用对次要区域的读取访问，则当主要区域不可用时，你的数据仍可以用于读取操作。 你可以将应用程序设计为当主要区域无响应时无缝切换到从次要区域进行读取。

异地冗余存储 (GRS) 和地理区域冗余存储 (GZRS) 都会将数据异步复制到次要区域。 若要对次要区域进行读取访问，可启用读取访问异地冗余存储 (RA-GRS) 或读取访问地理区域冗余存储 (RA-GZRS)。 若要详细了解 Azure 存储提供的冗余的各种选项，请参阅 [Azure 存储冗余](storage-redundancy.md)。

本文介绍了如何检查存储帐户的“上次同步时间”属性，以便评估主要区域与次要区域之间的任何差异。

## <a name="about-the-last-sync-time-property"></a>关于“上次同步时间”属性

因为异地复制是异步的，所以在发生中断时，已写入到主要区域的数据可能尚未写入到次要区域。 “上次同步时间”属性表示，最近一次将主要区域中的数据成功写入到次要区域的时间。 在上次同步时间之前对主要区域所做的所有写入都可以从次要位置读取。 在上次同步时间之后对主要区域所做的写入尚不一定可供读取。

“上次同步时间”属性是一个 GMT 日期/时间值。

## <a name="get-the-last-sync-time-property"></a>获取“上次同步时间”属性

可以使用 PowerShell 或 Azure CLI 检索“上次同步时间”属性的值。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要使用 PowerShell 获取存储帐户的上次同步时间，请安装版本 1.11.0 或更高版本的 [Az.Storage](https://www.powershellgallery.com/packages/Az.Storage) 模块。 然后检查存储帐户的 **GeoReplicationStats.LastSyncTime** 属性。 请务必将占位符值替换为你自己的值：

```powershell
$lastSyncTime = $(Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account> `
    -IncludeGeoReplicationStats).GeoReplicationStats.LastSyncTime
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要使用 Azure CLI 获取存储帐户的上次同步时间，请检查存储帐户的 **geoReplicationStats.lastSyncTime** 属性。 使用 `--expand` 参数返回嵌套在 geoReplicationStats 下的属性的值。 请务必将占位符值替换为你自己的值：

```azurecli-interactive
$lastSyncTime=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --expand geoReplicationStats \
    --query geoReplicationStats.lastSyncTime \
    --output tsv)
```

---

## <a name="see-also"></a>另请参阅

- [Azure 存储冗余](storage-redundancy.md)
- [更改存储帐户的冗余选项](redundancy-migration.md)
- [使用异地冗余设计高度可用的应用程序](geo-redundant-design.md)
