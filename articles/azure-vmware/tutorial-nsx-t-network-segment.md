---
title: 教程 - 在 Azure VMware 解决方案中创建 NSX-T 网段
description: 了解如何创建用于 vCenter 中的 VM 的 NSX-T 网段
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: f88f186d2af10bcc114d64920a3ac489ef7be54f
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367663"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution"></a>教程：在 Azure VMware 解决方案中创建 NSX-T 网段

在 vCenter 中创建的虚拟机 (VM) 将被放到在 NSX-T 中创建的网段，并且在 vCenter 中可见。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 在 NSX-T Manager 中导航以添加网段
> * 添加新的网段
> * 在 vCenter 中查看新的网段

## <a name="prerequisites"></a>先决条件

你需要拥有有权访问 vCenter 的 Azure VMware 解决方案私有云和 NSX-T Manager 管理界面。 有关详细信息，请参阅[配置网络](tutorial-configure-networking.md)教程。

## <a name="provision-a-network-segment-in-nsx-t"></a>在 NSX-T 中预配网段

1. 在私有云的 vCenter 中，选择“SDDC-Datacenter > 网络”，请注意此时并未有任何网络。

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 在私有云的 NSX-T Manager 中，选择“网络”。

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 选择“网段”。

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 在“NSX-T 网段概述”页中，选择“添加网段”。 三个网段作为私有云预配的一部分创建而成，不能用于 VM。  要用于此目的，需要添加新的网段。

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 为此网段命名，选择预配置的 Tier1 网关 (TNTxx-T1) 作为“连接的网关”，将“类型”保留为“灵活”，选择预配置的覆盖“传输区域”(TNTxx-OVERLAY-TZ)，然后选择“设置子网”。   此部分中的所有其他设置以及“端口”和“网段配置文件”均可以保留为默认值，如同配置一样。 

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 设置新网段的网关 IP 地址，然后选择“添加”。 使用的 IP 地址必须位于不重叠的 RFC1918 地址块上，这样可确保可以连接到新网段上的 VM。

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 通过选择“应用”应用新的网段，然后使用“保存”保存配置。 

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 现已创建新的网段，通过选择“否”，你将拒绝用于继续配置网段的选项。

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 选择“网络 > 网段”并看到列表中有此新网段（在此示例中为“ls01”），从而确认 NSX-T 中存在该新网段。

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

1. 选择“网络 > SDDC-Datacenter”并看到列表中有此新网段（在此示例中为“ls01”），从而确认 vCenter 中存在该新网段。

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="在私有云的 vCenter 中，选择“SDDC-Datacenter”>“网络”，请注意此时并未有任何网络。":::

## <a name="next-steps"></a>后续步骤

本教程中创建了用于 vCenter 中 VM 的 NSX-T 网段。 现在可以[创建内容库以在 Azure VMware 解决方案中部署 VM](deploy-vm-content-library.md)。 还可以在本教程中创建的网络上预配 VM。

否则，请继续学习下一教程，了解如何在 Azure VMware 解决方案中创建与私有云对等互连的 ExpressRoute Global Reach。

> [!div class="nextstepaction"]
> [将本地环境与私有云对等互连](tutorial-expressroute-global-reach-private-cloud.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
