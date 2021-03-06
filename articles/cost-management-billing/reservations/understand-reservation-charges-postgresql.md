---
title: 了解预留折扣 - Azure Database for PostgreSQL 单一服务器
description: 了解如何对 Azure Database for PostgreSQL 单一服务器应用预留折扣。
author: kummanish
ms.author: manishku
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 51b2f4964c01efbfc82008134d47f09648a772ff
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2020
ms.locfileid: "88681968"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-postgresql-single-server"></a>如何对 Azure Database for PostgreSQL Single 服务器应用预留折扣

购买 Azure Database for PostgreSQL Single 服务器预留容量后，预留折扣会自动应用到与预留属性和数量匹配的 PostgreSQL Single 服务器。 预留仅涵盖 Azure Database for PostgreSQL Single 服务器的计算费用。 按标准费率收取存储和网络费用。

## <a name="how-reservation-discount-is-applied"></a>如何应用预留折扣

预留折扣的性质是“不用就会失效”。 因此，如果你在任何小时内没有匹配资源，那么你将丢失该小时的预留数量。 不能结转未使用的预留小时数。</br>

关闭资源时，预留折扣将自动应用于指定范围内的另一个匹配资源。 如果在指定的范围内找不到匹配的资源，则预留小时数将丢失。

## <a name="discount-applied-to-azure-database-for-postgresql-single-server"></a>向 Azure Database for PostgreSQL Single 服务器应用的折扣

按小时向运行的 PostgreSQL Single 服务器应用 Azure Database for PostgreSQL Single 服务器预留容量折扣。 购买的预留容量则与运行中的 Azure Database for PostgreSQL Single 服务器产生的计算用量进行匹配。 对于非整小时运行的 PostgreSQL Single 服务器，预留自动应用到与预留属性匹配的其他 Azure Database for PostgreSQL Single 服务器。 折扣可以应用于同时运行的 Azure Database for PostgreSQL Single 服务器。 如果没有整小时运行且与预留属性匹配的 PostgreSQL 单一服务器，则无法获得该小时的全部预留折扣权益。

以下示例演示如何根据购买的核心的数目及其运行时间，来应用 Azure Database for PostgreSQL Single 预留容量折扣。

* **示例 1**：购买 8 vCore 的 Azure Database for PostgreSQL Single 服务器预留容量。 如果运行的是与其余预留属性匹配的 16 vCore Azure Database for PostgreSQL Single 服务器，需按照即用即付价格为 8 vCore PostgreSQL Single 服务器计算用量支付费用，并获得一小时 8 vCore PostgreSQL Single 服务器计算用量的预留折扣。</br>

余下的示例假设购买的 Azure Database for PostgreSQL Single 服务器预留容量用于 16 vCore Azure Database for PostgreSQL Single 服务器，并且剩余的预留属性与正在运行的 PostgreSQL Single 服务器相匹配。

* **示例 2**：运行两台 8 vCore 的 Azure Database for PostgreSQL Single 服务器一小时。 对这两台 8 vCore Azure Database for PostgreSQL Single 服务器的计算用量应用 16 vCore 预留折扣。

* **示例 3**：下午 1:00 到 1:30 运行一台 16 vCore Azure Database for PostgreSQL Single 服务器。 下午 1:30 到 2:00 运行另一台 16 vCore Azure Database for PostgreSQL Single 服务器。 预留折扣同时涵盖这两个数据库。

* **示例 4**：下午 1:00 到 1:45 运行一台 16 vCore Azure Database for PostgreSQL Single 服务器。 下午 1:30 到 2:00 运行另一台 16 vCore Azure Database for PostgreSQL Single 服务器。 将收取 15 分钟重叠期的即用即付费用。 预留折扣将应用到剩余时间的计算用量。

若要了解 Azure 预留的应用情况并在计费使用情况报告中查看该信息，请参阅[了解 Azure 预留使用情况](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)。

## <a name="next-steps"></a>后续步骤

如有任何疑问或需要帮助，请[创建支持请求](https://go.microsoft.com/fwlink/?linkid=2083458)。
