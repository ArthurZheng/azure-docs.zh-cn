---
title: 专用链接-Azure Database for MySQL
description: 了解 Azure Database for MySQL 的专用链接。
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: e49fd0d2b4d8fc801372dbc766c2fdc7beb21b10
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "90905910"
---
# <a name="private-link-for-azure-database-for-mysql"></a>Azure Database for MySQL 的专用链接

使用专用链接可以通过专用终结点连接到 Azure 中的各种 PaaS 服务。 Azure 专用链接实质上是将 Azure 服务引入专用虚拟网络 (VNet) 中。 就像 VNet 中的任何其他资源一样，可以使用专用 IP 地址访问 PaaS 资源。

有关支持专用链接功能的 PaaS 服务的列表，请查看专用链接 [文档](https://docs.microsoft.com/azure/private-link/index)。 专用终结点是特定 [VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) 和子网中的专用 IP 地址。

> [!NOTE]
> 专用链接功能仅适用于常规用途或内存优化定价层中的 Azure Database for MySQL 服务器。 请确保数据库服务器是这些定价层中的一种。

## <a name="data-exfiltration-prevention"></a>数据渗透防护

Azure Database for MySQL 中的数据（例如，数据库管理员）可以从一个系统提取数据并将其移到组织之外的其他位置或系统时进行筛选。 例如，该用户将数据移到第三方拥有的存储帐户。

假设有一个方案，该用户在连接到美国西部的 Azure Database for MySQL 服务器 (VM) 的 Azure 虚拟机中运行 MySQL 工作台。 下面的示例演示如何使用网络访问控制在 Azure Database for MySQL 上使用公共终结点限制访问。

* 通过将 " *允许 Azure 服务* " 设置为 "关闭"，禁用通过公共终结点 Azure Database for MySQL 的所有 Azure 服务流量。 请确保不允许 IP 地址或范围通过 [防火墙规则](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules) 或 [虚拟网络服务终结点](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet)访问服务器。

* 仅允许使用 VM 的专用 IP 地址将流量传输到 Azure Database for MySQL。 有关详细信息，请参阅有关[服务终结点](concepts-data-access-and-security-vnet.md)和 [VNet 防火墙规则](howto-manage-vnet-using-portal.md)的文章。

* 在 Azure VM 上，按如下所示使用网络安全组 (NSG) 和服务标记缩小传出连接的范围

    * 指定 NSG 规则以允许 *服务标记 = SQL 的流量。WestUs* -仅允许连接到美国西部 Azure Database for MySQL
    * 指定 NSG 规则 (优先级较高的) 拒绝服务标记的流量 *= SQL* -拒绝连接更新到所有区域中的 Azure Database for MySQL</br></br>

在此设置结束时，Azure VM 只能连接到美国西部区域中的 Azure Database for MySQL。 不过，连接并不限于单个 Azure Database for MySQL。 VM 仍可连接到美国西部区域中的任何 Azure Database for MySQL，包括不属于订阅的数据库。 尽管我们在上述场景中已将数据渗透范围缩小到了特定的区域，但我们并未完全消除这种渗透。</br>

通过专用链接，你现在可以设置网络访问控制（如 Nsg），以限制对专用终结点的访问。 然后，将单个 Azure PaaS 资源映射到特定的专用终结点。 恶意有问必答只能访问映射的 PaaS 资源 (例如 Azure Database for MySQL) ，而不能访问其他资源。

## <a name="on-premises-connectivity-over-private-peering"></a>通过专用对等互连建立本地连接

从本地计算机连接到公共终结点时，需要使用服务器级防火墙规则将 IP 地址添加到基于 IP 的防火墙。 尽管此模型非常适合用于允许对开发或测试工作负荷的单个计算机进行访问，但在生产环境中却难以管理。

使用 "专用" 链接，可以使用 [Express Route](https://azure.microsoft.com/services/expressroute/) (ER) 、专用对等互连或 [VPN 隧道](https://docs.microsoft.com/azure/vpn-gateway/)来启用对专用终结点的跨界访问。 然后，他们可以通过公共终结点禁用所有访问权限，而不使用基于 IP 的防火墙。

> [!NOTE]
> 在某些情况下，Azure Database for MySQL 和 VNet 子网位于不同的订阅中。 在这些情况下，必须确保以下配置：
> - 请确保这两个订阅都注册了 **DBforMySQL** 资源提供程序。 有关详细信息，请参阅[资源管理器注册][resource-manager-portal]

## <a name="configure-private-link-for-azure-database-for-mysql"></a>为 Azure Database for MySQL 配置专用链接

### <a name="creation-process"></a>创建过程

启用专用链接需要专用终结点。 可以使用以下操作方法指南完成此操作。

* [Azure 门户](https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-portal)
* [CLI](https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-cli)

### <a name="approval-process"></a>审批过程
网络管理员创建专用终结点 (PE) 后，MySQL 管理员可以 (PEC) 到 Azure Database for MySQL 来管理专用终结点连接。 网络管理员和 DBA 之间的这种职责分离有助于管理 Azure Database for MySQL 连接性。 

* 导航到 Azure 门户中的 Azure Database for MySQL server 资源。 
    * 在左窗格中选择 "专用终结点连接"
    * 显示 (PECs 的所有专用终结点连接的列表) 
    * 已创建 (PE) 对应的专用终结点

:::image type="content" source="media/concepts-data-access-and-security-private-link/select-private-link-portal.png" alt-text="选择专用终结点门户":::

* 在列表中选择单个 PEC。

:::image type="content" source="media/concepts-data-access-and-security-private-link/select-private-link.png" alt-text="选择专用终结点门户":::

* MySQL 服务器管理员可以选择批准或拒绝 PEC，还可以选择添加短文本响应。

:::image type="content" source="media/concepts-data-access-and-security-private-link/select-private-link-message.png" alt-text="选择专用终结点门户":::

* 批准或拒绝后，该列表将反映相应的状态以及响应文本

:::image type="content" source="media/concepts-data-access-and-security-private-link/show-private-link-approved-connection.png" alt-text="选择专用终结点门户":::

## <a name="use-cases-of-private-link-for-azure-database-for-mysql"></a>用于 Azure Database for MySQL 的私有链接案例

客户端可以从同一 VNet 中的对等互连 VNet 连接到专用终结点，也可以通过跨区域的 VNet 到 VNet 连接连接到专用终结点。 此外，客户端可以使用 ExpressRoute、专用对等互连或 VPN 隧道从本地进行连接。 以下简化示意图显示了常见用例。

:::image type="content" source="media/concepts-data-access-and-security-private-link/show-private-link-overview.png" alt-text="选择专用终结点门户":::

### <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>从对等互连虚拟网络 (VNet) 中的 Azure VM 进行连接
配置 [vnet 对等互连](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-powershell) ，以便与对等互连 VNet 中的 Azure VM 建立与 Azure Database for MySQL 的连接。

### <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>从 VNet 到 VNet 环境中的 Azure VM 进行连接
配置 [vnet 到 VNET VPN 网关连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) ，以便从另一区域或订阅中的 Azure VM 建立与 Azure Database for MySQL 的连接。

### <a name="connecting-from-an-on-premises-environment-over-vpn"></a>通过 VPN 从本地环境进行连接
若要建立从本地环境到 Azure Database for MySQL 的连接，请选择并实现以下选项之一：

* [点到站点连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)
* [站点到站点 VPN 连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)
* [ExpressRoute 线路](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)

## <a name="private-link-combined-with-firewall-rules"></a>将专用链接与防火墙规则结合使用

将专用链接与防火墙规则结合使用时，可能会出现以下情况和结果：

* 如果未配置任何防火墙规则，则默认情况下，任何流量都不能访问 Azure Database for MySQL。

* 如果配置公共流量或服务终结点并创建专用终结点，则不同类型的传入流量将由相应类型的防火墙规则授权。

* 如果未配置任何公用流量或服务终结点，并且创建专用终结点，则只能通过专用终结点访问 Azure Database for MySQL。 如果未配置公共流量或服务终结点，则在拒绝或删除所有已批准的专用终结点后，流量将无法访问 Azure Database for MySQL。

## <a name="deny-public-access-for-azure-database-for-mysql"></a>拒绝 Azure Database for MySQL 的公共访问

如果要仅依赖于专用终结点来访问其 Azure Database for MySQL，则可以通过在数据库服务器上设置 "**拒绝公共网络访问**" 配置来禁用设置所有公共终结点 (即[防火墙规则](concepts-firewall-rules.md)和[VNet 服务终结点](concepts-data-access-and-security-vnet.md)) 。 

如果此设置设置为 *"是"*，则只允许通过专用终结点连接到 Azure Database for MySQL。 如果此设置设置为 " *否*"，则客户端可以根据防火墙或 VNet 服务终结点设置连接到 Azure Database for MySQL。 此外，一旦设置了专用网络访问的值，客户就不能添加和/或更新现有的 "防火墙规则" 和 "VNet 服务终结点规则"。

> [!Note]
> 此功能在所有 Azure Database for PostgreSQL 单服务器支持常规用途和内存优化定价层的 Azure 区域中均可用。
>
> 此设置不会对 Azure Database for MySQL 的 SSL 和 TLS 配置产生任何影响。

若要了解如何设置对 Azure 门户的 Azure Database for MySQL **拒绝公共网络访问权限** ，请参阅 [如何配置拒绝公共网络访问](howto-deny-public-network-access.md)。

## <a name="next-steps"></a>后续步骤

若要详细了解 Azure Database for MySQL 安全功能，请参阅以下文章：

* 若要为 Azure Database for MySQL 配置防火墙，请参阅 [防火墙支持](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules)。

* 若要了解如何为 Azure Database for MySQL 配置虚拟网络服务终结点，请参阅 [从虚拟网络配置访问权限](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet)。

* 有关 Azure Database for MySQL 连接的概述，请参阅 [Azure Database for MySQL 连接体系结构](https://docs.microsoft.com/azure/mysql/concepts-connectivity-architecture)

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md