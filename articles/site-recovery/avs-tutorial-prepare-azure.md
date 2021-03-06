---
title: 为 Azure VMware 解决方案 VM 的灾难恢复准备 Azure Site Recovery 资源
description: 了解如何使用 Azure Site Recovery 为 Azure VMware 解决方案计算机的灾难恢复准备 Azure 资源。
services: site-recovery
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/29/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 2bd305e3760a8c3d743037e7d90b71f5e9579eda
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93395472"
---
# <a name="prepare-azure-site-recovery-resources-for-disaster-recovery-of-azure-vmware-solution-vms"></a>为 Azure VMware 解决方案 VM 的灾难恢复准备 Azure Site Recovery 资源

本文介绍如何准备 Azure 资源和组件，以便可以使用 [Azure Site Recovery](site-recovery-overview.md) 服务设置 Azure VMware 解决方案的灾难恢复。 [Azure VMware 解决方案](../azure-vmware/introduction.md)可在 Azure 中提供私有云。 这些私有云包含基于专用裸机 Azure 基础结构构建的 vSphere 群集。

本文是此系列的第一个教程，演示如何为 Azure VMware 解决方案 VM 设置灾难恢复。 


在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 验证 Azure 帐户是否拥有复制权限。
> * 创建恢复服务保管库。 保管库保存 VM 和其他复制组件的元数据和配置信息。
> * 设置 Azure 虚拟网络 (VNet)。 在故障转移后创建的 Azure VM 将加入此网络。

> [!NOTE]
> 教程介绍了某个方案的最简单部署路径。 它们尽可能使用默认选项，并且不显示所有可能的设置和路径。 有关详细说明，请查看 Site Recovery 目录的“操作指南”部分所列的文章。

> [!NOTE]
> 将 Azure Site Recovery 用于 Azure VMware 解决方案的一些概念与本地 VMware VM 的灾难恢复重叠，因此将相应地交叉引用文档。

## <a name="before-you-start"></a>开始之前

- 在 Azure 中[部署](../azure-vmware/tutorial-create-private-cloud.md) Azure VMware 解决方案私有云
- 查看 [VMware](vmware-azure-architecture.md) 灾难恢复的体系结构
- 阅读有关 [VMware](vmware-azure-common-questions.md) 的常见问题

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/pricing/free-trial/)。 然后登录到 [Azure 门户](https://portal.azure.com)。


## <a name="verify-account-permissions"></a>验证帐户权限

如果你刚刚创建了免费 Azure 帐户，那么你就是订阅的管理员，并拥有所需的权限。 如果你不是订阅管理员，请要求管理员分配你所需的权限。 若要为新虚拟机启用复制，必须有权执行以下操作：

- 在所选资源组中创建 VM。
- 在所选虚拟网络中创建 VM。
- 写入 Azure 存储帐户。
- 写入 Azure 托管磁盘。

若要完成这些任务，应为帐户分配“虚拟机参与者”内置角色。 此外，若要在保管库中管理 Site Recovery 操作，应为帐户分配“Site Recovery 参与者”内置角色。


## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

1. 在 Azure 门户菜单中选择“创建资源”，然后在市场中搜索“恢复” 。
2. 从搜索结果中选择“备份和站点恢复”，然后在“备份和站点恢复”页中单击“创建”。 
3. 在“创建恢复服务保管库”页中，选择“订阅”。 我们将使用“Contoso 订阅”。
4. 在 **资源组** 中，选择现有资源组或创建新资源组。 在本教程中，我们使用 **contosoRG**。
5. 在“保管库名称”中输入一个易记名称，用于标识此保管库。 对于这组教程，我们使用 **ContosoVMVault**。
6. 在“区域”中，选择保管库应位于的区域。 我们将使用“西欧”。
7. 选择“查看 + 创建”。

   ![创建恢复服务保管库页的屏幕截图。](./media/tutorial-prepare-azure/new-vault-settings.png)

   新保管库此时会列在“仪表板” > “所有资源”中，以及“恢复服务保管库”主页上。  

## <a name="set-up-an-azure-network"></a>设置 Azure 网络

 Azure VMware 解决方案 VM 将复制到 Azure 托管磁盘。 故障转移时，Azure VM 将从这些托管磁盘创建，并加入到在此过程中指定的 Azure 网络。

1. 在 [Azure 门户](https://portal.azure.com)中，选择 **“创建资源”**  >  **“网络”**  >  **“虚拟网络”** 。
2. 选择“资源管理器”作为部署模型。
3. 在“名称”中，输入网络名称。 名称在 Azure 资源组中必须唯一。 在本教程中我们将使用 **ContosoASRnet**。
4. 在“地址空间”中，输入用 CDR 标记表示的虚拟网络地址范围。 我们将使用 **10.1.0.0/24**。
5. 在“订阅”中，选择要在其中创建网络的订阅。
6. 指定将在其中创建网络的“资源组”。 我们将使用现有资源组 contosoRG。
7. 在“位置”中，选择创建了恢复服务保管库的同一个区域。 本教程使用“西欧”。 该网络必须位于与保管库相同的区域中。
8. 在“地址范围”中，输入网络的范围。 我们将使用 10.1.0.0/24，而不使用子网。
9. 我们将保留基本 DDoS 防护的默认选项，网络上没有服务终结点或防火墙。
9. 选择“创建”。

   ![创建虚拟网络选项的屏幕截图。](media/tutorial-prepare-azure/create-network.png)

创建虚拟网络需要几秒钟的时间。 创建后，可在 Azure 门户仪表板中看到它。




## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [准备基础结构](avs-tutorial-prepare-avs.md)
- [了解](../virtual-network/virtual-networks-overview.md) Azure 网络。
- [了解](../virtual-machines/managed-disks-overview.md)托管磁盘。
