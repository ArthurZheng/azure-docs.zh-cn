---
title: 监视 Azure Site Recovery | Microsoft Docs
description: 使用门户监视和排查 Azure Site Recovery 复制问题与操作
author: raynew
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: raynew
ms.openlocfilehash: aa9d776df50306ab1705426c923413b5a5d545a5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "68717347"
---
# <a name="monitor-site-recovery"></a>监视 Site Recovery

本文介绍如何使用 Site Recovery 的内置监视功能监视 Azure [Site Recovery](site-recovery-overview.md)。  可以监视：

- 通过 Site Recovery 复制的计算机的运行状况和状态
- 测试计算机的故障转移状态。
- 影响配置和复制的问题和错误。
- 基础结构组件，例如本地服务器。


## <a name="before-you-start"></a>开始之前

在开始之前，可能需要查看[常见监视问题](monitoring-common-questions.md)。

## <a name="monitor-in-the-dashboard"></a>在仪表板中监视

1. 在保管库中，单击“概览”  。 恢复服务仪表板在单个位置合并了保管库的所有监视信息。 Site Recovery 和 Azure 备份服务都有页面，可在这些页面之间切换。

    ![Site Recovery 仪表板](./media/site-recovery-monitor-and-troubleshoot/dashboard.png)

2. 在该仪表板中，向下钻取到不同的区域。 

    ![Site Recovery 仪表板](./media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)。

3. 在“复制的项”中，单击“全部查看”可查看保管库中的所有服务器。  
4. 单击每个部分的状态详细信息，以便向下钻取。
5. 在“基础结构”视图中，按复制的计算机类型将监视信息排序。 

## <a name="monitor-replicated-items"></a>监视复制的项

“复制的项”监视保管库中已启用复制的所有计算机的运行状况。 

**State** | **详细信息**
--- | ---
正常 | 复制正常进行。 未检测到任何错误或警告症状。
警告 | 检测到一个或多个可能影响复制的警告症状。
严重 | 检测到一个或多个严重复制错误症状。<br/><br/> 这些错误症状通常指示复制处于停滞状态，或者复制进度跟不上数据更改速率。
不适用 | 目前预期服务器无法复制。 这可能包括已故障转移的计算机。

## <a name="monitor-test-failovers"></a>监视测试故障转移

在“故障转移测试成功”中，监视保管库中计算机的故障转移状态。 

- 我们建议每隔六个月在复制的计算机上至少运行测试故障转移一次。 这样，便可以在不中断生产环境的情况下，检查故障转移是否按预期工作。 
- 只有在成功完成故障转移以及故障转移后的清理过程之后，才将测试故障转移视为成功。

**State** | **详细信息**
--- | ---
建议的测试 | 自启用保护以来未进行测试故障转移的计算机。
已成功执行 | 已成功完成一次或多次测试故障转移的计算机。
不适用 | 目前不符合测试故障转移条件的计算机。 例如，已故障转移的计算机、正在进行初始复制/测试故障转移/故障转移的计算机。

## <a name="monitor-configuration-issues"></a>监视配置问题

在“配置问题”中监视任何可能影响你能否成功进行故障转移的问题。 

- 一个默认每隔 12 小时定期运行的验证程序操作将会检测配置问题（但不会检测软件更新可用性）。 单击“配置问题”部分标题旁边的刷新图标可以强制验证程序操作立即运行。 
- 单击相应的链接获取更多详细信息。 对于影响特定计算机的问题，请单击“目标配置”列中的“需要关注”。   详细信息包括补救措施的建议。

**State** | **详细信息**
--- | ---
缺少配置 | 缺少所需的设置，例如恢复网络或资源组。
缺少资源 | 指定的资源未找到，或者在订阅中不可用。 例如，已删除或迁移了资源。 受监视的资源包括目标资源组、目标 VNet/子网、日志/目标存储帐户、目标可用性集、目标 IP 地址。
订阅配额 |  将可用订阅资源配额的余量，与故障转移保管库中所有计算机所需的余量进行比较。<br/><br/> 如果资源不足，则报告不足的配额余量。<br/><br/> 配额是要监视的 VM 核心计数、VM 系列核心计数和网络接口卡 (NIC) 计数。
软件更新 | 新软件更新的可用性，以及有关即将过期的软件版本的信息。


## <a name="monitor-errors"></a>监视错误

在“错误摘要”中，监视目前尚未解决的、可能影响保管库中服务器的复制的错误症状，以及监视受影响的计算机数目。 

- 该部分的开头显示影响本地基础结构组件的错误。 例如，未从本地配置服务器或 Hyper-V 主机上的 Azure Site Recovery 提供程序收到检测信号。
- 接下来显示影响已复制的服务器的复制错误症状。
- 表条目分别按错误严重性的降序以及受影响计算机数的降序排序。
- 参考受影响服务器数能够很好地了解单一根本问题是否影响了多台计算机。 例如，网络问题可能会影响复制到 Azure 的所有计算机。 
- 单个服务器上可能出现多个复制错误。 在这种情况下，每个错误症状会将该服务器计入到它所影响的服务器列表中。 解决问题后，复制参数将得到改善，而该错误将从计算机中清除。

## <a name="monitor-the-infrastructure"></a>监视基础架构。

在“基础结构”视图中，监视参与复制的基础结构组件，以及服务器与 Azure 服务之间的连接运行状况。 

- 绿线表示连接正常。
- 带有叠加错误图标的红线指示存在一个或多个影响连接的错误症状。
-  将鼠标指针悬停在错误图标上会显示错误和受影响实体的数目。 单击图标会显示受影响实体的筛选列表。

    ![Site Recovery 基础结构视图（保管库）](./media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

### <a name="tips-for-monitoring-the-infrastructure"></a>有关监视基础结构的提示

- 确保本地基础结构组件（配置服务器、进程服务器、VMM 服务器、Hyper-V 主机、VMware 计算机）运行最新版本的 Site Recovery 提供程序和/或代理。
- 若要使用基础结构视图的所有功能，应运行这些组件的[更新汇总 22](https://support.microsoft.com/help/4072852)。
- 若要使用基础结构视图，请选择适用于环境的复制方案。 可以在视图中向下钻取以查看更多详细信息。 下表显示了代表的方案。

    **方案** | **State**  | **视图可用？**
    --- |--- | ---
    **在本地站点之间复制** | 所有状态 | 否 
    **Azure 区域之间的 Azure VM 复制**  | 已启用复制/初始复制正在进行 | 是
    **Azure 区域之间的 Azure VM 复制** | 已故障转移/故障回复 | 否   
    **从 VMware 复制到 Azure** | 已启用复制/初始复制正在进行 | 是     
    **从 VMware 复制到 Azure** | 已故障转移/故障回复 | 否      
    **从 Hyper-V 复制到 Azure** | 已故障转移/故障回复 | 否

- 若要查看单个复制计算机的基础结构视图，请在保管库菜单中单击“复制的项”，然后选择一个服务器。   




## <a name="monitor-recovery-plans"></a>监视恢复计划

在“恢复计划”中，监视计划数目、创建新计划，以及修改现有计划。   

## <a name="monitor-jobs"></a>监视作业

在“作业”中，监视 Site Recovery 操作的状态。 

- Azure Site Recovery 中的大多数操作以异步方式执行，将创建并使用一个跟踪作业来跟踪操作进度。 
- 作业对象包含跟踪操作状态和进度的全部所需信息。 

按如下所述监视作业：

1. 在仪表板中转到“作业”部分，可以看到过去 24 小时内已完成的、正在进行的或等待输入的作业的摘要。  可以单击任一状态获取相关作业的详细信息。
2. 单击“全部查看”可查看过去 24 小时内的所有作业。 

    > [!NOTE]
    > 还可以从保管库菜单 >“Site Recovery 作业”访问作业信息。  

2. “Site Recovery 作业”列表中显示了作业列表。  在顶部菜单中，可以获取特定作业的错误详细信息、根据特定的条件筛选作业列表，以及将选定作业的详细信息导出到 Excel。
3. 单击某个作业可深入查看更多信息。 

## <a name="monitor-virtual-machines"></a>监视虚拟机

在“复制的项”中，获取复制的计算机的列表。  
    ![Site Recovery 中“复制的项”列表视图](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

2. 可以查看和筛选信息。 在顶部的操作菜单中，可以针对特定的计算机执行操作，包括运行测试故障转移，或查看特定的错误。
3. 单击“列”可显示其他列，例如，显示 RPO、目标配置问题和复制错误。 
4. 单击“筛选器”可以根据复制运行状况或特定复制策略等特定参数来查看信息。 
5. 右键单击某个计算机可以启动操作，例如，执行测试故障转移，或查看与它关联的特定错误详细信息。
6. 单击某个计算机可以深入查看其更多详细信息。 详细信息包括：
   - **复制信息**：计算机的当前状态和运行状况。
   - **RPO**（恢复点目标）：虚拟机的当前 RPO，以及上次计算 RPO 的时间。
   - **恢复点**：计算机的最新可用恢复点。
   - **故障转移就绪性**：指示是否对该计算机运行了测试故障转移、计算机上运行的代理版本（适用于运行移动服务的计算机）和任何配置问题。
   - **错误**：列出当前在计算机上观察到的复制错误症状，以及可能的原因/措施。
   - **事件**：影响计算机的最近事件列表，按时间顺序列出。 错误详细信息显示当前可观测到的错误症状，而事件是影响了计算机的问题的历史记录。
   - **基础结构视图**：显示将计算机复制到 Azure 时方案的基础结构状态。

     ![Azure Site Recovery 中复制的项详细信息/概述](./media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

## <a name="subscribe-to-email-notifications"></a>订阅电子邮件通知

可以订阅接收以下关键事件的电子邮件通知：
 
- 复制的计算机的严重状态。
- 本地基础结构组件与 Site Recovery 服务之间无连接。 使用检测信号机制来检测 Site Recovery 与注册到保管库中的本地服务器之间的连接。
- 故障转移失败。

订阅方式如下：

在保管库 > **监视** "部分中，单击" **Site Recovery 事件**"。
1. 单击“电子邮件通知”。
1. 在“电子邮件通知”中，启用通知并指定收件人。**** 可向所有订阅管理员发送通知，并选择性地发送到特定的电子邮件地址。

    ![电子邮件通知](./media/site-recovery-monitor-and-troubleshoot/email.png)

## <a name="next-steps"></a>后续步骤

[了解](monitor-log-analytics.md)如何使用 Azure Monitor 监视 Site Recovery。
