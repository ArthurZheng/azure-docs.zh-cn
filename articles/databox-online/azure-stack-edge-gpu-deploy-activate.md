---
title: 教程：在 Azure 门户中使用 GPU 激活 Azure Stack Edge Pro 设备 | Microsoft Docs
description: 本教程介绍 Azure Stack Edge Pro GPU 部署方法，指导你激活物理设备。
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 09/10/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to activate Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: 6807174f374e75f826001afecc1eccf8ba1a0603
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2020
ms.locfileid: "91743380"
---
# <a name="tutorial-activate-azure-stack-edge-pro-with-gpu"></a>教程：使用 GPU 激活 Azure Stack Edge Pro

本教程介绍如何通过本地 Web UI 使用板载 GPU 激活 Azure Stack Edge Pro 设备。

完成激活过程需要大约 5 分钟。

本教程介绍了以下内容：

> [!div class="checklist"]
> * 先决条件
> * 激活物理设备

## <a name="prerequisites"></a>先决条件

在使用 GPU 配置和设置 Azure Stack Edge Pro 设备之前，请确保：

* 对于物理设备： 
    
    - 已根据[安装 Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md) 中详述的内容安装了物理设备。
    - 已按[配置网络、计算网络和 Web 代理](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)中详述的内容配置网络和计算网络设置
    - 如果通过“设备”页面更改了设备名称或 DNS 域，则应上传自己的设备证书或在设备上生成设备证书。 如果尚未执行此步骤，则会在设备激活过程中出现错误，并阻止激活。 有关详细信息，请转到[配置证书](azure-stack-edge-gpu-deploy-configure-certificates.md)。
    
* 已从所创建的用于管理 Azure Stack Edge Pro 设备的 Azure Stack Edge 服务获得激活密钥。 有关详细信息，请转到[准备部署 Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md)。


## <a name="activate-the-device"></a>激活设备

1. 在设备的本地 Web UI 中，转到“开始使用”页。
2. 在“激活”磁贴中选择“激活” 。 

    ![本地 Web UI“云详细信息”页](./media/azure-stack-edge-gpu-deploy-activate/activate-1.png)
    
3. 在“激活”窗格中，输入你在[获取 Azure Stack Edge Pro 激活密钥](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key)中获取的激活密钥 。

4. 选择“应用”。 

    ![本地 Web UI“云详细信息”页 2](./media/azure-stack-edge-gpu-deploy-activate/activate-2.png)


5. 首先激活设备。 随后系统会提示你下载密钥文件。
    
    ![本地 Web UI“云详细信息”页 3](./media/azure-stack-edge-gpu-deploy-activate/activate-3.png)
    
    选择“下载并继续”，并将 device-serial-no.json 文件保存在设备之外的安全位置。 该密钥文件包含设备上的 OS 磁盘和数据磁盘的恢复密钥。 可能需要这些密钥来辅助将来的系统恢复。

    下面是 json 文件的内容：

        
    ```json
    {
      "Id": "<Device ID>",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "<BitLocker key for data disk>",
        "hcsdata": "<BitLocker key for data disk>"
      },
      "SystemVolumeBitLockerRecoveryKey": "<BitLocker key for system volume>",
      "ServiceEncryptionKey": "<Azure service encryption key>"
    }
    ```
        
 
    下表对各密钥进行了说明：
    
    |字段  |说明  |
    |---------|---------|
    |`Id`    | 这是设备的 ID。        |
    |`DataVolumeBitLockerExternalKeys`|这些是数据磁盘的 BitLockers 密钥，用于恢复设备上的本地数据。|
    |`SystemVolumeBitLockerRecoveryKey`| 这是系统卷的 BitLocker 密钥。 此密钥有助于恢复设备的系统配置和系统数据。 |
    |`ServiceEncryptionKey`| 此密钥可保护经过 Azure 服务的数据流。 此密钥确保 Azure 服务的安全威胁不会导致存储的信息受到安全威胁。 |

6. 转至“概述”页面。 设备状态应显示为“已激活”。

    ![本地 Web UI“云详细信息”页 4](./media/azure-stack-edge-gpu-deploy-activate/activate-4.png)
 
已完成设备激活。 现在可以在设备上添加共享。


## <a name="next-steps"></a>后续步骤

本教程介绍了以下内容：

> [!div class="checklist"]
> * 先决条件
> * 激活物理设备

若要了解如何使用 Azure Stack Edge Pro 设备传输数据，请参阅：

> [!div class="nextstepaction"]
> [使用 Azure Stack Edge Pro 传输数据](./azure-stack-edge-j-series-deploy-add-shares.md)
