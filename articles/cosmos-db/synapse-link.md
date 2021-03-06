---
title: Azure Synapse Link for Azure Cosmos DB、优势以及何时使用
description: 了解有关 Azure Synapse Link for Azure Cosmos DB 的信息。 借助 Synapse Link，可以使用 Azure Synapse Analytics 对 Azure Cosmos DB 中的操作数据运行准实时分析 (HTAP)。
author: Rodrigossz
ms.author: rosouz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: 97f24537f2fa68f1a9be83e2c9abdc8101edb8d0
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014539"
---
# <a name="what-is-azure-synapse-link-for-azure-cosmos-db-preview"></a>什么是 Azure Synapse Link for Azure Cosmos DB（预览版）？

> [!IMPORTANT]
> Azure Synapse Link for Azure Cosmos DB 目前以预览版提供。 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

Azure Synapse Link for Azure Cosmos DB 是一种云原生混合事务和分析处理 (HTAP) 功能，可用于对 Azure Cosmos DB 中的操作数据运行准实时分析。 Azure Synapse Link 在 Azure Cosmos DB 和 Azure Synapse Analytics 之间建立紧密无缝的集成。

通过使用 [Azure Cosmos DB 分析存储](analytical-store-introduction.md)（完全隔离的列存储），Azure Synapse Link 可以在 [Azure Synapse Analytics](../synapse-analytics/overview-what-is.md) 中针对大规模操作数据提供无提取-转换-加载 (ETL) 分析。 业务分析师、数据工程师和数据科学家现在可以互换使用 Synapse Spark 或 Synapse SQL 来运行准实时商业智能、分析和机器学习管道。 可以实现此目的，而不会影响 Azure Cosmos DB 上的事务工作负荷的性能。 

下图显示了 Azure Synapse Link 与 Azure Cosmos DB 和 Azure Synapse Analytics 的集成： 

:::image type="content" source="./media/synapse-link/synapse-analytics-cosmos-db-architecture.png" alt-text="Azure Synapse Analytics 与 Azure Cosmos DB 集成的体系结构关系图" border="false":::

## <a name="benefits"></a><a id="synapse-link-benefits"></a> 优势

若要分析较大的操作数据集，同时最大限度地降低对任务关键型事务工作负荷性能的影响，通常情况下会通过提取-转换-加载 (ETL) 管道提取并处理 Azure Cosmos DB 中的操作数据。 ETL 管道需要大量数据移动，这样会增加操作复杂性，并对事务工作负荷产生性能影响。 还会增加延迟以从原始时间分析操作数据。

与传统的基于 ETL 的解决方案相比，Azure Synapse Link for Azure Cosmos DB 提供了多种优势，例如：  

### <a name="reduced-complexity-with-no-etl-jobs-to-manage"></a>降低了复杂性，无需管理 ETL 作业

借助 Azure Synapse Link，可以使用 Azure Synapse Analytics 直接访问 Azure Cosmos DB 分析存储，无需进行复杂的数据移动。 对操作数据所做的任何更新都准实时显示在分析存储中，不包含任何 ETL 或更改源作业。 可以从 Synapse Analytics 对分析存储运行大型分析，无需额外的数据转换。

### <a name="near-real-time-insights-into-your-operational-data"></a>准实时了解操作数据

使用 Azure Synapse Link，现在可以准实时深入了解操作数据。 由于有许多层要提取、转换和加载操作数据，因此基于 ETL 的系统在分析操作数据时往往会产生较高的延迟。 通过 Azure Cosmos DB 分析存储与 Azure Synapse Analytics 的本机集成，可以准实时分析操作数据，从而启用新的业务方案。 

### <a name="no-impact-on-operational-workloads"></a>不会影响操作工作负荷

借助 Azure Synapse Link，可以在使用事务工作负荷（基于行的事务存储）的预配吞吐量处理事务操作时，针对 Azure Cosmos DB 分析存储（单独的列存储）运行分析查询。  分析工作负荷独立于事务工作负荷流量提供，无需使用为操作数据预配的任何吞吐量。

### <a name="optimized-for-large-scale-analytics-workloads"></a>针对大规模分析工作负荷进行了优化

Azure Cosmos DB 分析存储已经过优化，可为分析工作负荷提供可伸缩性、弹性和性能，无需依赖计算运行时。 存储技术是自行管理，可优化分析工作负荷。 通过对 Azure Synapse Analytics 的内置支持，访问此存储层可提供简单性和高性能。

### <a name="cost-effective"></a>经济高效

借助 Azure Synapse Link，可以为运营分析获取成本优化且完全托管的解决方案。 它消除了用于分析操作数据的传统 ETL 管道所需的额外存储层和计算层。 

Azure Cosmos DB 分析存储遵循基于消耗的定价模型，该模型基于数据存储和分析读/写操作以及已执行的查询。 这不会要求你预配任何吞吐量，因为目前如此操作是为了事务工作负荷。 通过 Azure Synapse Analytics 中极具弹性的计算引擎来访问你的数据，使运行存储和计算的总体成本非常高效。


### <a name="analytics-for-locally-available-globally-distributed-multi-region-writes"></a>本地可用的分析、全球分布式多区域写入

你可以高效地对 Azure Cosmos DB 中距离最近的区域的数据副本运行分析查询。 Azure Cosmos DB 提供了最先进的功能，可按主动-主动的方式运行全球分发的分析工作负荷和事务工作负荷。

## <a name="enable-htap-scenarios-for-your-operational-data"></a>为操作数据启用 HTAP 方案

Synapse Link 汇集了 Azure Cosmos DB 分析存储和 Azure Synapse Analytics 运行时支持。 通过此集成，你可以构建云原生 HTAP（混合事务/分析处理）解决方案，该解决方案基于大型数据集的操作数据的实时更新生成见解。 它可解锁新的业务方案，以根据实时趋势发出警报，构建准实时仪表板并基于用户行为生成业务体验。

### <a name="azure-cosmos-db-analytical-store"></a>Azure Cosmos DB 分析存储

Azure Cosmos DB 分析存储是 Azure Cosmos DB 中的操作数据的面向列的表示形式。 此分析存储适用于对大型操作数据集进行快速且经济高效的查询，无需复制数据和影响事务工作负荷的性能。

分析存储准实时自动选择事务工作负荷中的高频率插入、更新、删除，作为 Azure Cosmos DB 的完全托管功能（“自动同步”）。 无需更改源或 ETL。 

若你拥有全局分发的 Azure Cosmos DB 帐户，为容器启用分析存储后，它将适用于该帐户的所有区域。 有关分析存储的详细信息，请参阅 [Azure Cosmos DB 分析存储概述](analytical-store-introduction.md)一文。

### <a name="integration-with-azure-synapse-analytics"></a><a id="synapse-link-integration"></a>与 Azure Synapse Analytics 集成

借助 Synapse Link，你现在可以直接从 Azure Synapse Analytics 连接到 Azure Cosmos DB 容器，并访问没有单独连接器的分析存储。 Azure Synapse Analytics 当前支持 Synapse 与 [Synapse Apache Spark](../synapse-analytics/spark/apache-spark-concepts.md) 和 [Synapse SQL 无服务器](../synapse-analytics/sql/on-demand-workspace-overview.md)的链接。

可以在 Azure Synapse Analytics 支持的不同分析运行时间内以互操作方式同时从 Azure Cosmos DB 分析存储查询数据。 不需要其他数据转换来分析操作数据。 可以使用以下对象来查询和分析分析存储数据：

* 完全支持 Scala、Python、SparkSQL 和 C# 的 Synapse Apache Spark。 Synapse Spark 是数据工程和数据科学方案的核心

* 具有 T-SQL 语言且支持熟悉的 BI 工具（例如，Power BI Premium 等）的 SQL 无服务器

> [!NOTE]
> 通过 Azure Synapse Analytics，可以同时访问 Azure Cosmos DB 容器中的分析存储和事务存储。 但是，如果要对操作数据运行大规模分析或扫描，我们建议你使用分析存储来避免对事务工作负荷的性能影响。

> [!NOTE]
> 可以通过将 Azure Cosmos DB 容器连接到 Azure 区域中的 Synapse 运行时，在该区域中以较低的延迟运行分析。

此集成可为不同用户启用以下 HTAP 方案：

* 一位 BI 工程师，想要建模和发布 Power BI 报表，以直接通过 Synapse SQL 访问 Azure Cosmos DB 中的实时操作数据。

* 数据分析人员，想要通过使用 Synapse SQL 查询 Azure Cosmos DB 容器中的操作数据来从中获得见解，大规模读取数据并将这些发现与其他数据源合并。

* 数据科学家，想要使用 Synapse Spark 查找一项功能来改善其模型并在不进行复杂的数据工程的情况下训练该模型。 他们还可以将模型 post 推理的结果写入 Azure Cosmos DB 以便通过 Spark Synapse 对数据进行实时评分。

* 数据工程师，想要无需手动 ETL 过程即可对 Azure Cosmos DB 容器创建 SQL 或 Spark 表，以便使数据可供使用者访问。

有关 Azure Cosmos DB 的 Azure Synapse Analytics 运行时支持的详细信息，请参阅 [Azure Synapse Analytics for Cosmos DB 支持](../synapse-analytics/synapse-link/concept-synapse-link-cosmos-db-support.md)。

## <a name="when-to-use-azure-synapse-link-for-azure-cosmos-db"></a>何时使用 Azure Synapse Link for Azure Cosmos DB？

建议在以下情况下使用 Synapse Link：

* 如果你是 Azure Cosmos DB 客户，并且想要对操作数据运行分析、BI 和机器学习。 在此类情况下，Synapse Link 提供了更集成的分析体验，同时不影响事务存储的预配吞吐量。 例如：

  * 如果你现在直接使用单独的连接器对 Azure Cosmos DB 操作数据运行分析或 BI，或

  * 如果你正在运行 ETL 过程以将操作数据提取到单独的分析系统中。
 
在此类情况下，Synapse Link 提供了更集成的分析体验，同时不影响事务存储的预配吞吐量。

如果需要传统的数据仓库要求（例如，高并发、工作负荷管理和跨多个数据源的聚合持久性），则不建议使用 Synapse Link。 有关详细信息，请参阅[可由 Synapse Link for Azure Cosmos DB 提供帮助的常见方案](synapse-link-use-cases.md)。

## <a name="limitations"></a>限制

* Azure Azure Cosmos DB 的 Azure Synapse 链接适用于 SQL API 和 MongoDB Azure Cosmos DB API。 它不支持 Gremlin API 和表 API。 支持 Cassandra API 提供个人预览版，有关详细信息，请联系 [Azure Synapse 链接团队](mailto:cosmosdbsynapselink@microsoft.com)。  

* 目前只能为新容器启用分析存储。 若要对现有容器使用分析存储，请使用 [Azure Cosmos DB 迁移工具](cosmosdb-migrationchoices.md)将数据从现有容器迁移到新容器。 可以在新的和现有 Azure Cosmos DB 帐户上启用 Synapse 链接。

* 通过 Synapse SQL 无服务器访问 Azure Cosmos DB 分析存储当前处于封闭预览阶段。 若要请求访问权限，请向 [Azure Synapse 链接团队](mailto:cosmosdbsynapselink@microsoft.com)发送电子邮件。

* 对于启用了分析存储的容器，此时不支持在分析存储中自动备份和还原数据。 如果在数据库帐户上启用了 Synapse 链接，Azure Cosmos DB 将继续自动将数据 [备份](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore) 到事务性存储区中， (只按计划的备份间隔) 容器。 需要特别注意的是，当启用了分析存储的容器被还原到新帐户时，将仅使用事务性存储来还原容器，而不会启用分析存储。 

* 当前无法访问预配了 Synapse SQL 的 Azure Cosmos DB 分析存储。

## <a name="pricing"></a>定价

Azure Synapse Link 的计费模型包括使用 Azure Cosmos DB 分析存储和 Synapse 运行时所产生的成本。 若要了解详细信息，请参阅 [Azure Cosmos DB 分析存储定价](analytical-store-introduction.md#analytical-store-pricing)和 [Azure Synapse Analytics 定价](https://azure.microsoft.com/pricing/details/synapse-analytics/)文章。

## <a name="next-steps"></a>后续步骤

若要了解更多信息，请参阅下列文档：

* [Azure Cosmos DB 分析存储概述](analytical-store-introduction.md)

* [开始使用 Azure Synapse Link for Azure Cosmos DB](configure-synapse-link.md)
 
* [Azure Synapse Analytics 运行时支持的功能](../synapse-analytics/synapse-link/concept-synapse-link-cosmos-db-support.md)

* [有关 Azure Synapse Link for Azure Cosmos DB 的常见问题](synapse-link-frequently-asked-questions.md)

* [Azure Synapse Link for Azure Cosmos DB 用例](synapse-link-use-cases.md)
