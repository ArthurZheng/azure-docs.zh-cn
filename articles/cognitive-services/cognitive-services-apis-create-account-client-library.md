---
title: 使用 Azure 管理客户端库创建认知服务资源
titleSuffix: Azure Cognitive Services
description: 使用 Azure 管理客户端库创建和管理 Azure 认知服务资源。
services: cognitive-services
keywords: 认知服务, 认知智能, 认知解决方案, ai 服务
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-ten
ms.openlocfilehash: e8628d051db7f5066a81171567f6f7e54fb0ab97
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2020
ms.locfileid: "91262444"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-management-client-library"></a>快速入门：使用 Azure 管理客户端库创建认知服务资源

按照本快速入门使用 Azure 管理客户端库创建和管理 Azure 认知服务资源。

Azure 认知服务是包含 REST API 和客户端库 SDK 的云服务，可帮助开发人员将认知智能内置于应用程序，而无需具备直接的人工智能 (AI) 或数据科学技能或知识。 借助 Azure 认知服务，开发人员可以通过能够看、听、说、理解甚至开始推理的认知解决方案，轻松将认知功能添加到他们的应用程序中。

独立的 AI 服务由 Azure 订阅下创建的 Azure [资源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)表示。 创建资源后，可以使用生成的密钥和终结点对应用程序进行身份验证。

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](includes/quickstarts/management-csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](includes/quickstarts/management-java.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](includes/quickstarts/management-node.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](includes/quickstarts/management-python.md)]

::: zone-end
