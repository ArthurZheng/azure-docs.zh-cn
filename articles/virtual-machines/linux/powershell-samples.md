---
title: PowerShell 示例
description: Vm 的 PowerShell 示例列表
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 6d20f02b846c7ae47aef395694aef2bc5732957e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978639"
---
# <a name="azure-vm-powershell-samples-for-creating-and-managing-linux-vms"></a>用于创建和管理 Linux Vm 的 Azure VM PowerShell 示例

下表包含用于创建和管理 Linux 虚拟机的 PowerShell 脚本示例的链接。

| Script | 说明 |
|---|---|
|**创建虚拟机**||
| [创建完全配置的虚拟机](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建资源组、虚拟机以及所有相关资源。|
| [创建已启用 Docker 的 VM](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建一个虚拟机，将此 VM 配置为 Docker 主机，并运行 NGINX 容器。 |
| [创建 VM 并运行配置脚本](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建一个虚拟机，并使用 Azure 自定义脚本扩展安装 NGINX。 |
| [创建安装有 WordPress 的 VM](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建一个虚拟机，并使用 Azure 自定义脚本扩展安装 WordPress。 |
| [从托管 OS 磁盘创建 VM](./../scripts/virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 通过将现有托管磁盘附加为 OS 磁盘来创建虚拟机。 |
| [从快照创建 VM](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 通过先从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘来从快照创建虚拟机。 |
|**管理存储**||
| [在相同或不同的订阅中从 VHD 创建托管磁盘](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 在相同或不同订阅中，从专用 VHD 将托管磁盘创建为 OS 磁盘，或从数据 VHD 将托管磁盘创建为数据磁盘。  |
| [从快照创建托管磁盘](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 从快照创建托管磁盘。 |
| [将快照作为 VHD 导出到存储帐户](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 将托管快照作为 VHD 导出到不同区域中的存储帐户。 |
| [将托管磁盘的 VHD 导出到存储帐户](../scripts/virtual-machines-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 将托管磁盘的基础 VHD 导出到不同区域的存储帐户。 |
| [从 VHD 创建快照](../scripts/virtual-machines-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 从 VHD 创建快照，然后使用该快照快速创建多个相同的托管磁盘。  |
| [将快照复制到相同或不同的订阅](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 将快照复制到父快照所在区域中的相同或不同订阅。 |
|**监视虚拟机**||
| [使用 Azure Monitor 日志监视 VM](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建一个虚拟机，安装 Log Analytics 代理，并在 Log Analytics 工作区中注册该 VM。  |
| [将托管磁盘复制到相同或不同的订阅](../scripts/virtual-machines-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json) | 将托管磁盘复制到父托管磁盘所在区域中的相同或不同订阅。
| [使用 PowerShell 收集订阅中所有 VM 的详细信息](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 创建一个 csv，其中包含所提供订阅中 VM 的 VM 名称、资源组名称、区域、虚拟网络、子网、专用 IP 地址、OS 类型和公共 IP 地址。
| | |