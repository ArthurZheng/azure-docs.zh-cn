---
title: 使用 Azure 流分析进行实时事件处理
description: 本文介绍使用 Azure 流分析实现实时事件处理和分析的参考体系结构。
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 23c7112639a64097d95df29bde16636702837f9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "83832973"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>参考体系结构：使用 Microsoft Azure 流分析实时处理事件
通过 Azure 流分析进行实时事件处理的参考体系结构的用途是提供一个通用的蓝图，以便通过 Microsoft Azure 部署实时平台即服务 (PaaS) 流处理解决方案。

## <a name="summary"></a>摘要
一直以来，分析解决方案始终基于 ETL（提取、转换、加载）这样的功能以及在分析之前存储数据的数据仓库。 由于要求在不管变化，而数据到达速度也越来越快，这个现有模式的使用已经到了极限。 一个解决方案是允许在进行存储之前，在移动的流中分析数据。虽然这不是一项新的功能，但该方法尚未在所有行业类别中广泛采用。 

Microsoft Azure 提供了各种类别的分析技术，支持一系列不同的解决方案和要求。 由于提供的产品/服务的多样性，因此选择为端到端解决方案部署何种 Azure 服务并不那么容易。 本文旨在介绍各种支持事件流式处理解决方案的 Azure 服务的功能和互操作性。 本文还介绍了允许客户充分利用此类方法的某些方案。

## <a name="contents"></a>内容
* 执行摘要
* 实时分析简介
* Azure 中实时数据的价值定位
* 实时分析常见方案
* 体系结构和组件
  * 数据源
  * 数据集成层
  * 实时分析层
  * 数据存储层
  * 呈现/消耗层
* 结论

**作者：** Charles Feddersen，Microsoft Corporation Data Insights 卓越中心解决方案架构师

**发布时间：** 2015 年 1 月

**修订版：** 1.0

**下载：** [使用 Microsoft Azure 流分析实时处理事件](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>获取帮助
若要获得进一步的帮助，可前往 [Azure 流分析的 Microsoft Q&A 问题页面](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

