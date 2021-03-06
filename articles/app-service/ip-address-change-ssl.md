---
title: 为 SSL IP 地址更改做准备
description: 如果 SSL IP 地址将要更改，请了解如何在更改后继续运行应用。
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: dcfe11bcab25f6267a557de5faf7befab467bc29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "81535717"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>如何为 SSL IP 地址更改做好准备

如果收到有关 Azure 应用服务应用的 SSL IP 地址即将更改的通知，请遵照本文中的说明释放现有的 SSL IP 地址并分配新地址。

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>释放 SSL IP 地址并分配新地址

1.  打开 [Azure 门户](https://portal.azure.com)。

2.  在左侧导航菜单中选择“应用服务”。

3.  从列表中选择自己的应用服务应用。

4.  在左侧导航窗格中，单击“设置”标头下的“SSL 设置”。 

1. 在“TLS/SSL 绑定”部分，选择主机名记录。 在打开的编辑器中，从“SSL 类型”下拉菜单中选择“SNI SSL”，然后单击“添加绑定”。   如果出现操作成功的消息，则表示已释放现有的 IP 地址。

6.  在“SSL 绑定”部分，再次选择包含证书的同一主机名记录。 在打开的编辑器中，这次请从“SSL 类型”下拉菜单中选择“基于 IP 的 SSL”，然后单击“添加绑定”。   如果出现操作成功的消息，表示已获取新的 IP 地址。

7.  如果在域注册门户（第三方 DNS 提供程序或 Azure DNS）中配置了 A 记录（直接指向你的 IP 地址的 DNS 记录），请将现有 IP 地址替换为新生成的 IP 地址。 遵照下一部分中的说明可以找到新 IP 地址。

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>在 Azure 门户中找到新的 SSL IP 地址

1.  等待几分钟时间，然后打开 [Azure 门户](https://portal.azure.com)。

2.  在左侧导航菜单中选择“应用服务”。

3.  从列表中选择自己的应用服务应用。

4.  在“设置”标题下，单击左侧导航栏中的“属性”，找到标有“虚拟 IP 地址”的部分。

5. 复制 IP 地址并重新配置域记录或 IP 机制。

## <a name="next-steps"></a>后续步骤

本文介绍了如何对 Azure 发起的 IP 地址更改做好准备。 有关 Azure 应用服务中的 IP 地址的详细信息，请参阅 [Azure 应用服务中的入站和出站 IP 地址](overview-inbound-outbound-ips.md)。
