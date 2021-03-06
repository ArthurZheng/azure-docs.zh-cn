---
title: 查询要求
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/30/2020
ms.author: aahi
ms.openlocfilehash: 518865f78c170f1fbe4e65b96dc149c1b449a88b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91631353"
---
在查询中，使用 `@StartTime` 参数获取单一时间戳的指标数据。 指标顾问将在运行查询时将参数替换为 `yyyy-MM-ddTHH:mm:ss` 格式字符串。

> [!IMPORTANT]
> 对于每个维度组合，查询应在每个时间戳处最多返回一条记录。 查询返回的所有记录必须具有相同的时间戳。 指标顾问将针对每个时间戳运行此查询以引入数据。 有关详细信息和示例，请参阅[查询常见问题解答部分](../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data)。 