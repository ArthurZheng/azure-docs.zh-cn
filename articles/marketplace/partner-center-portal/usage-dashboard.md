---
title: Microsoft 商业 marketplace 分析、Azure Marketplace 和 Microsoft AppSource 中的使用情况仪表板
description: 了解如何访问所有 VM 套餐使用情况和按流量计费指标。 在合作伙伴中心内转到商业市场下的使用情况仪表板。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 9b8432a54aa90b7d500898b2f6959d075ac89460
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "88245326"
---
# <a name="usage-dashboard-in-microsoft-commercial-marketplace-analytics"></a>Microsoft 商业市场分析中的使用情况仪表板

本文提供有关合作伙伴中心中的使用情况仪表板的信息。 此仪表板在两个单独选项卡中显示所有 VM 套餐使用情况和按流量计费指标：VM 使用情况和按流量计费使用情况。

若要访问使用情况仪表板，请在“商业市场”下打开[“分析”](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)仪表板 。

>[!NOTE]
> 有关分析术语的详细定义，请参阅[适用于商业市场分析的常见问题和术语](./faq-terminology.md)。

## <a name="usage-dashboard"></a>使用情况仪表板

使用情况仪表板会表示所有虚拟机 (VM) 套餐使用情况和按流量计费使用情况的指标。 可在两个单独选项卡中找到这些指标：VM 使用情况和按流量计费使用情况。

在 VM 使用情况选项卡中，有以下各项的图形表示形式：

- [使用情况摘要](#usage-summary)
- [按地域的使用情况](#usage-by-geography)
- [按套餐的使用情况](#usage-by-offers)
- [按产品/服务和计划的使用趋势](#usage-trend-by-offers-and-plans)
- [按套餐类型的使用情况](#usage-by-offer-type)
- [按 VM 大小的使用情况](#usage-by-vm-size)
- [按销售渠道的使用情况](#usage-by-sales-channel)
- [详细使用情况数据](#detailed-usage-data)

伙伴中心中的使用事件生成和报告之间的最大延迟为48小时。

### <a name="usage-summary"></a>使用情况摘要

使用情况摘要表显示客户购买的所有套餐的客户使用小时数。

- 规范化使用小时数定义为按 VM 核心数规范化的使用小时数（[VM 核心数] x [原始使用小时数]）。 指定为“SHAREDCORE”的 VM 使用 1/6（或 0.1666）作为 [VM 核心数] 的乘数。
- 原始使用小时数定义为 VM 已运行的时间数量（以小时为单位）。
- 百分比值表示所选日期范围的使用情况增长变化（[上个月使用情况 – 第一个月使用情况]/第一个月使用情况）。
- 朝上的绿色三角形表示增长变化。
- 朝下的红色三角形表示相对于上个月的负增长变化。
- 微型条形图表示每月值。 可以通过将鼠标悬停在图表中的列上方来显示每个月的值。

### <a name="usage-by-geography"></a>按地域的使用情况

" **按地理位置划分的规范化使用量** " 热图显示根据客户所在国家/地区映射的使用小时数。 国家/地区颜色变化表示规范化使用情况集中程度。 按地图上的主页按钮可还原为原始视图。

### <a name="usage-by-offers"></a>按套餐的使用情况

- 按套餐的规范化使用情况饼图根据所选日期范围，按套餐显示规范化使用小时数细分。 前五个套餐会显示在图中，而其余套餐会分组为“其余全部”类别。
- 条形图描绘所选日期范围的逐月增长趋势。 月份列表示套餐的使用小时数，以及相应月份的最高使用小时数。 折线图描绘在辅助 Y 轴上绘制的增长百分比趋势。
- 使用图表顶部的滑块沿 x 轴向右滚动，并且/或是将焦点置于特定数据点上。

### <a name="usage-trend-by-offers-and-plans"></a>按产品/服务和计划的使用趋势

此图显示了所选计划的规范化使用趋势 (以前称为产品/服务的 Sku) 。 套餐排行榜显示使用量最高的前 50 个套餐，按使用小时数排序。 计划排行榜显示最高的50计划，其中包含所选产品/服务的最高使用量。

### <a name="usage-by-offer-type"></a>按套餐类型的使用情况

- 按套餐类型的使用情况饼图根据套餐类型来组织使用情况。
- 排名靠前的套餐会显示在图表中，而其余套餐会分组为“其余全部”。
- 趋势图表显示逐月增长趋势。 月份列表示该月中排名靠前套餐类型的使用情况。

### <a name="usage-by-vm-size"></a>按 VM 大小的使用情况

此图显示了所选 VM 大小的使用趋势 (所有产品/服务的最多五个) 。 柱形图随所选 VM 大小的使用时间堆积。

排行榜显示使用量最高的前 50 个 VM 大小，按使用小时数排序。

### <a name="usage-by-sales-channel"></a>按销售渠道的使用情况

- 按销售渠道的使用情况饼图根据销售渠道来组织使用情况
- 使用量最高的排名靠前销售渠道会显示在图表中，而其余销售渠道会分组为“其余全部”。
- 月份列表示该月中排名靠前销售渠道的使用情况。
- 此图表的功能与“按套餐的使用情况”图表相同

### <a name="detailed-usage-data"></a>详细使用情况数据

使用情况详细信息表显示按使用量排序的前 1000 个使用情况记录的编号列表。

- 网格中的每列都可进行排序。
- 如果记录计数小于 1000，则可以将数据提取到 CSV 文件中。
- 如果记录计数超过 1000，则导出数据会异步放置在将在接下来 30 天内可用的下载页面中。
- 将筛选器应用于 **详细使用情况数据** 以仅显示你感兴趣的数据。 按国家/地区、销售渠道、Marketplace 许可证类型、使用类型、产品名称、产品类型、免费试用版、Marketplace 订阅 ID、客户 ID 和公司名称筛选数据。

> [!NOTE]
> 在页面筛选器中选择“使用类型”，以在“规范化视图”或“原始视图”中查看页面上的图表。 这些图表的默认视图为“规范化视图”。

使用情况页面筛选器在页面级别进行应用。 可以选择多个筛选器以针对选择查看的条件，以及希望显示在“详细使用情况数据”网格/导出中的数据来呈现图表。 筛选器会应用于为订单页面右上角选择的数据范围提取的数据。

- 只会为在所选日期范围内获取的套餐列出套餐类型和套餐名称。 会为从列表中选择的套餐类型显示列表中的套餐名称。
- 每个筛选器选项的默认选择都是“全部”，除了“使用类型”。 “使用类型”的默认选择是规范化使用情况。 若要在图表中显示原始使用情况，请选择“原始使用情况”。
- 应用的筛选器会显示针对所选筛选器的计数选择。 不会为默认选择显示应用的筛选器。

> [!NOTE]
> 在[常见问题解答和术语](link needed)文章的数据字典部分中定义了“详细订单数据”网格、页面筛选器和所有可能选择中的每个字段的详细定义。

“按流量计费使用情况”选项卡提供按计量维度度量使用情况的套餐类型的使用情况信息。 当前提供 SaaS 套餐类型超额。 该选项卡显示 SaaS 按流量计费使用情况的超额趋势的图形表示形式：

- 按计量维度的超额趋势：显示套餐的所选计量维度的每月超额趋势。 X 轴表示月份，Y 轴表示使用数量。 自定义计量的度量单位也显示在 Y 轴上。
- **按计划的超额趋势**：按计划表示所选计量维度的使用情况的趋势。 显示的计划将代表所选产品/服务的最高使用量的前5个计划。
- 按前 50 个客户的超额趋势：使用小时数最高的前 50 个套餐会显示在排行榜上，并按自定义计量的最高使用量进行排名。 在排行榜中选择一个客户可查看所选计量维度的使用趋势。
- 按排名靠前客户的超额趋势：显示占总体使用量百分比的排名靠前客户百分点值。 排名靠前客户百分点值沿 X 轴显示，由客户的使用数量确定。 Y 轴显示使用数量。 可以通过将鼠标悬停在折线图的各个点上方来显示详细信息。

如果你有多个使用自定义计量的产品/服务，则按流量计费使用情况报表将根据其自定义计量维度显示你的所有产品/服务的使用情况信息。

> [!NOTE]
> 对于为页面筛选器选择的任何计量维度，都会显示此页面上的使用情况详细信息和所有图表。

## <a name="next-steps"></a>后续步骤

- 有关合作伙伴中心商业市场内可用的分析报表的概述，请参阅[合作伙伴中心内的商业市场分析](./analytics.md)。
- 有关为产品/服务汇总市场活动的聚合数据的图表、趋势和值，请参阅[商业市场分析中的“摘要”仪表板](./summary-dashboard.md)。
- 有关采用图形和可下载格式的订单的信息，请参阅[商业市场分析中的“订单”仪表板](./orders-dashboard.md)。
- 有关客户的详细信息（包括增长趋势），请参阅[商业市场分析中的“客户”仪表板](./customer-dashboard.md)。
- 有关过去 30 天内的下载请求列表，请参阅[商业市场分析中的“下载”仪表板](./downloads-dashboard.md)。
- 若要查看有关 Microsoft AppSource 和 Azure 市场上的套餐的客户反馈综合视图，请参阅[商业市场分析中的“评分和评价”仪表板](./ratings-reviews.md)。
- 有关商业市场分析的常见问题解答以及数据术语的综合字典，请参阅[商业市场分析的常见问题解答和术语](./faq-terminology.md)。
