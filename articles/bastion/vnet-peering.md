---
title: VNet 对等互连和 Azure 堡垒体系结构
description: 本文介绍如何结合使用 VNet 对等互连和 Azure 堡垒来连接到 Vm。
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: cherylmc
ms.openlocfilehash: ad3cf33dacffc8bcda9376857206784afedf7139
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2020
ms.locfileid: "94361972"
---
# <a name="vnet-peering-and-azure-bastion-preview"></a>VNet 对等互连和 Azure 堡垒 (预览版) 

可以结合使用 Azure 堡垒和 VNet 对等互连。 配置 VNet 对等互连后，无需在每个对等互连 VNet 中部署 Azure 堡垒。 这意味着，如果将一个虚拟网络中配置的 Azure 堡垒主机 (VNet) ，则它可用于连接到在对等互连 VNet 中部署的 Vm，而无需部署其他堡垒主机。 有关 VNet 对等互连的详细信息，请参阅 [关于虚拟网络对等互连](../virtual-network/virtual-network-peering-overview.md)。

Azure 堡垒使用以下类型的对等互连：

* **虚拟网络对等互连：** 将虚拟网络连接到同一 Azure 区域中。
* **全局虚拟网络对等互连：** 跨 Azure 区域连接虚拟网络。

## <a name="architecture"></a>体系结构

配置 VNet 对等互连后，可以在中心辐射型拓扑或全网格拓扑中部署 Azure 堡垒。 Azure Bastion 部署是按虚拟网络进行的，而不是按订阅/帐户或虚拟机进行的。

在虚拟网络中预配 Azure 堡垒服务后，可在同一 VNet 中的所有 Vm 以及对等互连 Vnet 中使用 RDP/SSH 体验。 这意味着，你可以将堡垒部署整合到单个 VNet，并仍可访问部署在对等互连 VNet 中的 Vm，同时集中整个部署。

:::image type="content" source="./media/vnet-peering/design.png" alt-text="设计和体系结构示意图":::

此图显示了一个中心辐射型模型中的 Azure 堡垒部署的体系结构。 在此图中，可以看到以下配置：

* 堡垒主机部署在集中式中心虚拟网络中。
* 已部署 (NSG) 集中的网络安全组。
* Azure VM 上不需要公共 IP。

**逐步**

1. 使用任意 HTML5 浏览器连接到 Azure 门户。
1. 选择要连接到的虚拟机。
1. 跨对等互连 VNet 无缝检测到 Azure 堡垒。
1. 单击一下，在浏览器中打开 RDP/SSH 会话。 有关 RDP 和 SSH 并发会话的限制，请参阅 [rdp 和 ssh 会话](bastion-faq.md#limits)。

   :::image type="content" source="../../includes/media/bastion-vm-rdp/connect-vm.png" alt-text="“连接”":::

   有关通过 Azure 堡垒连接到 VM 的详细信息，请参阅：

   * [连接到 VM-RDP](bastion-connect-vm-rdp.md)。
   * [连接到 VM-SSH](bastion-connect-vm-ssh.md)。

## <a name="faq"></a>常见问题解答

[!INCLUDE [FAQ for VNet peering](../../includes/bastion-faq-peering-include.md)]

## <a name="next-steps"></a>后续步骤

阅读 [Bastion 常见问题解答](bastion-faq.md)。