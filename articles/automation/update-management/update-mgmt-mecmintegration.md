---
title: 将 Azure 自动化更新管理与 Microsoft Endpoint Configuration Manager 集成
description: 本文介绍如何使用更新管理配置 Microsoft Endpoint Configuration Manager，以便将软件更新部署到管理器客户端。
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 09c8ef818428157c7de8c3a87bcce8a7b80e62ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "88245904"
---
# <a name="integrate-update-management-with-microsoft-endpoint-configuration-manager"></a>将更新管理与 Microsoft Endpoint Configuration Manager 集成

在软件更新管理 (SUM) 周期中，已经投资购买 Microsoft Endpoint Configuration Manager 来管理电脑、服务器和移动设备的客户还可以依赖其在管理软件更新方面的优势和成熟度。

可以通过在 Microsoft Endpoint Configuration Manager 中创建和预暂存软件更新部署来报告和更新托管 Windows 服务器，并使用[更新管理](update-mgmt-overview.md)获取已完成的更新部署的详细状态。 如果使用 Microsoft Endpoint Configuration Manager 提供 Windows 服务器的更新合规性报告而不使用它管理 Windows 服务器的更新部署，则可以继续向 Endpoint Configuration Manager 进行报告，而使用 Azure 自动化更新管理来管理安全更新。

>[!NOTE]
>虽然更新管理支持 Windows Server 2008 R2 的更新评估和修补，但它不支持由运行此操作系统的 Microsoft Endpoint Configuration Manager 管理的客户端。

## <a name="prerequisites"></a>先决条件

* 必须将 [Azure 自动化更新管理](update-mgmt-overview.md)添加到自动化帐户。
* 当前由 Microsoft Endpoint Configuration Manager 环境管理的 Windows 服务器还需要向也启用了更新管理的 Log Analytics 工作区进行报告。
* Microsoft Endpoint Configuration Manager 的当前分支版本 1606 和更高版本中启用了此功能。 若要将 Microsoft Endpoint Configuration Manager 中心管理站点或独立主站点与 Azure Monitor 日志和重要集合进行集成，请查看[将 Configuration Manager 连接到 Azure Monitor 日志](../../azure-monitor/platform/collect-sccm.md)。  
* 如果 Windows 代理不从 Microsoft Endpoint Configuration Manager 接收安全更新，则必须将它们配置为与 Windows Server Update Services (WSUS) 服务器进行通信或有权访问 Microsoft 更新。

如何使用现有 Microsoft Endpoint Configuration Manager 环境管理 Azure IaaS 中托管的客户端主要取决于已在 Azure 数据中心与基础结构之间建立的连接。 此连接会影响你可能需要对 Microsoft Endpoint Configuration Manager 基础结构做的任何设计更改，还会影响与支持这些必要更改相关的成本。 若要了解在继续操作之前需要评估哪些规划注意事项，请查看 [Azure 上的 Configuration Manager - 常见问题解答](/configmgr/core/understand/configuration-manager-on-azure#networking)。

## <a name="manage-software-updates-from-microsoft-endpoint-configuration-manager"></a>从 Microsoft Endpoint Configuration Manager 管理软件更新

如果打算继续从 Microsoft Endpoint Configuration Manager 管理更新部署，请执行以下步骤。 Azure 自动化会连接到 Microsoft Endpoint Configuration Manager，以便向连接到 Log Analytics 工作区的客户端计算机应用更新。 可以从客户端计算机缓存获取更新内容，就像部署是由 Microsoft Endpoint Configuration Manager 管理的一样。

1. 使用[部署软件更新](/configmgr/sum/deploy-use/deploy-software-updates)中介绍的过程从 Microsoft Endpoint Configuration Manager 层次结构中的顶层站点创建软件更新部署。 与标准部署不同的必须配置的唯一设置是选项“不安装软件更新”，此选项用于控制部署包的下载行为。 通过在下一步中创建计划的更新部署，可以在更新管理中管理此行为。

2. 在 Azure 自动化中，选择“更新管理”。 根据[创建更新部署](update-mgmt-deploy-updates.md#schedule-an-update-deployment)中介绍的步骤创建一个新部署，并从“类型”下拉列表中选择“导入的组”，以便选择合适的 Microsoft Endpoint Configuration Manager 集合 。 请记住以下要点：

    a. 如果在所选的 Microsoft Endpoint Configuration Manager 设备集合上定义了维护窗口，则它会存储在集合的成员中，而不是存储在计划的部署中定义的“持续时间”设置中。

    b. 目标集合的成员必须连接到 Internet（直接连接、通过代理服务器或者通过 Log Analytics 网关）。

通过 Azure 自动化完成更新部署后，属于所选计算机组的成员的目标计算机将按计划的时间从本地客户端缓存中安装更新。 可以[查看更新部署状态](update-mgmt-deploy-updates.md#check-deployment-status)来监视部署结果。

## <a name="manage-software-updates-from-azure-automation"></a>从 Azure 自动化管理软件更新

若要从属于 Microsoft Endpoint Configuration Manager 客户端的 Windows Server VM 管理更新，需要配置客户端策略，为通过更新管理进行管理的所有客户端禁用软件更新管理功能。 默认情况下，客户端设置以层次结构中的所有设备为应用目标。 有关此策略设置以及如何配置此设置的详细信息，请查看[如何在 Configuration Manager 中配置客户端设置](/configmgr/core/clients/deploy/configure-client-settings)。

在执行此配置更改后，根据[创建更新部署](update-mgmt-deploy-updates.md#schedule-an-update-deployment)中介绍的步骤创建一个新部署，并从“类型”下拉列表中选择“导入的组”来选择合适的 Microsoft Endpoint Configuration Manager 集合 。

## <a name="next-steps"></a>后续步骤

若要设置集成计划，请参阅[计划更新部署](update-mgmt-deploy-updates.md#schedule-an-update-deployment)。
