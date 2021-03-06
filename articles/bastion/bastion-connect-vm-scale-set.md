---
title: 使用 Azure 堡垒连接到 Windows 虚拟机规模集 |Microsoft Docs
description: 在本文中，学习如何使用 Azure Bastion 连接到 Azure 虚拟机规模集。
services: bastion
author: charwen
ms.service: bastion
ms.topic: how-to
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: e3dc7ce36e773b5a615b1abf4f50406fcb07826b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "84744300"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>使用 Azure Bastion 连接到虚拟机规模集

本文介绍如何使用 Azure Bastion 安全、无缝地通过 RDP 连接到 Azure 虚拟网络中的 Windows 虚拟机规模集实例。 你可通过 Azure 门户直接连接到虚拟机规模集实例。 使用 Azure Bastion 时，VM 不需要客户端、代码或其他软件。 有关 Azure Bastion 的详细信息，请参阅[概述](bastion-overview.md)。

## <a name="before-you-begin"></a>准备阶段

请确保已为虚拟机规模集所在的虚拟网络设置 Azure Bastion 主机。 有关详细信息，请参阅[创建 Azure Bastion 主机](bastion-create-host-portal.md)。 在虚拟网络中预配和部署 Bastion 服务后，就可用它来连接到此虚拟网络中的虚拟机规模集实例。 Bastion 假设你正在使用 RDP 连接 Windows 虚拟机规模集，用 SSH 连接 Linux 虚拟机规模集。 要了解到 Linux VM 的连接，请参阅[连接到 VM - Linux](bastion-connect-vm-ssh.md)。

## <a name="connect-using-rdp"></a><a name="rdp"></a>使用 RDP 进行连接

1. 打开 [Azure 门户](https://portal.azure.com)。 导航到你想要连接到的虚拟机规模集。

   ![导航](./media/bastion-connect-vm-scale-set/1.png)
2. 导航到你想要连接到的虚拟机规模集实例，然后选择“连接”。 使用 RDP 连接时，虚拟机规模集应为 Windows 虚拟机规模集。

   ![虚拟机规模集](./media/bastion-connect-vm-scale-set/2.png)
3. 选择 " **连接**" 后，将显示一条侧栏，其中包含三个选项卡-RDP、SSH 和堡垒。 从侧边栏中选择“Bastion”选项卡。 如果未为虚拟网络预配 Bastion，可选择链接来配置 Bastion。 有关配置说明，请参阅[配置 Bastion](bastion-create-host-portal.md)。

   ![“Bastion”选项卡](./media/bastion-connect-vm-scale-set/3.png)
4. 在“Bastion”选项卡上，输入虚拟机规模集的用户名和密码，然后选择“连接”。

   ![连接](./media/bastion-connect-vm-scale-set/4.png)
5. 通过 Bastion 连接到此虚拟机的 RDP 将使用端口 443 和 Bastion 服务在 Azure 门户中（通过 HTML5）直接打开。

## <a name="next-steps"></a>后续步骤

阅读 [Bastion 常见问题解答](bastion-faq.md)。