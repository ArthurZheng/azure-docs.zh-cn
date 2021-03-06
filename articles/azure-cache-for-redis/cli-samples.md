---
title: 使用 Azure CLI 管理 Azure Cache for Redis
description: 管理 Azure Cache for Redis 的 Azure CLI 示例：创建缓存，删除缓存，获取缓存详细信息、主机名、端口和密钥，并连接 Web 应用。
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/14/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9bfdd2d03b3ab6edd04a641787475930435a9ffc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "87499596"
---
# <a name="manage-azure-cache-for-redis-with-azure-cli"></a>使用 Azure CLI 管理 Azure Cache for Redis

下表包含指向使用 Azure CLI 生成的 bash 脚本的链接。

| 创建缓存 | 说明 |
| ------------ | ----------- |
| [创建缓存](./scripts/create-cache.md) | 创建资源组和基本层 Azure Redis 缓存。 |
| [使用群集创建高级缓存](./scripts/create-premium-cache-cluster.md) | 通过启用群集来创建资源组和高级层缓存。|
| [获取缓存的详细信息](./scripts/show-cache.md) | 获取 Azure Redis 缓存实例的详细信息，包括预配状态。 |
| [获取主机名、端口和密钥](./scripts/cache-keys-ports.md) | 获取 Azure Redis 缓存实例的主机名、端口和密钥。 |
|**Web 应用和缓存**| **说明**|
| [将 Web 应用连接到 Azure Redis 缓存](./../app-service/scripts/cli-connect-to-redis.md) | 创建 Azure Web 应用和 Azure Redis 缓存，然后将 Redis 连接详细信息添加到应用设置。 |
|**删除缓存**| **说明** |
| [删除缓存](./scripts/delete-cache.md) | 删除 Azure Redis 缓存实例  |

有关 Azure CLI 的详细信息，请参阅[安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 和 [Azure CLI 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)。
