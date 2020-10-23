---
title: 如何创建和管理 DHCP
description: 本文介绍如何在 Azure VMware 解决方案中管理 DHCP。
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: cb74ed4474cc14081e59142f2f60915fccd47559
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92461050"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>如何在 Azure VMware 解决方案中创建和管理 DHCP

NSX 为私有云提供配置 DHCP 的能力。 如果你使用 NSX-T 来托管你的 DHCP 服务器，请参阅 [创建 dhcp 服务器](#create-dhcp-server)。 否则，如果在网络中有第三方外部 DHCP 服务器，请参阅 [创建 DHCP 中继服务](#create-dhcp-relay-service)。

## <a name="create-dhcp-server"></a>创建 DHCP 服务器

使用以下步骤在 NSX-T 上配置 DHCP 服务器。

1. 在 "NSX 管理器" 中，导航到 " **网络** " 选项卡，然后选择 **DHCP**。 
1. 选择 " **添加服务器** "，然后提供服务器名称和 IP 地址。 
1. 选择“保存”。

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="添加 DHCP 服务器" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>将 DHCP 服务器连接到第1层网关。

1. 从列表中选择 " **第1层网**关" 和 "网关"，然后选择 " **编辑**"。

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="添加 DHCP 服务器" border="true":::

1. 选择 " **无 IP 分配设置** " 以添加子网。

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="添加 DHCP 服务器" border="true":::

1. 为 "**类型**" 选择 " **DHCP 本地服务器**"。 
1. 选择**Dhcp 服务器**的**默认 dhcp** ，然后选择 "**保存**"。


1. 在 " **第1层网关** " 窗口中，选择 " **保存**"。 
1. 选择 " **关闭编辑** " 完成操作。

### <a name="add-a-network-segment"></a>添加网络段

创建 DHCP 服务器后，需要向其添加网络段。

1. 在 "NSX-T" 中选择 "**网络**" 选项卡，然后选择 "**连接**" 下的**段**。 
1. 选择 " **添加段** " 并为段命名并为第1层网关建立连接。 
1. 选择 " **设置子网** " 以配置新的子网。 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="添加 DHCP 服务器" border="true":::

1. 在 " **设置子网** " 窗口中，选择 " **添加子网**"。 
1. 输入网关 IP 地址和 DHCP 范围，选择 "**添加**"，然后单击 "**应用**"

1. 选择 " **保存** " 以添加新的网络段。

## <a name="create-dhcp-relay-service"></a>创建 DHCP 中继服务

1. 选择 " **网络** " 选项卡，然后在 " **IP 管理**" 下选择 **DHCP**。 
1. 选择 " **添加服务器**"。 
1. 对于 " **服务器类型** " 选择 "DHCP 中继"，然后输入中继服务器的服务器名称和 IP 地址。 
1. 选择“保存”。

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="添加 DHCP 服务器" border="true":::

1. 在 "**连接**" 下选择**第1层网关**。 
1. 选择第1层网关上的垂直省略号，然后选择 " **编辑**"。

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="添加 DHCP 服务器" border="true":::

1. 选择 " **没有 Ip 分配集** "，以定义 ip 地址分配。

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="添加 DHCP 服务器" border="true":::

1. 为 "**类型**" 选择 " **DHCP 中继服务器**"。
1. 选择用于 **Dhcp 中继**的 dhcp 中继服务器。 
1. 选择“保存”。


## <a name="specify-a-dhcp-range-ip-on-a-segment"></a>在段上指定 DHCP 范围 IP

> [!NOTE]
> 此配置是在 DHCP 客户端段上实现 DHCP 中继功能所必需的。 

1. 在 " **连接**" 下，选择 " **段**"。 
1. 选择垂直省略号，然后选择 " **编辑**"。 

   >[!TIP]
   >如果要添加新段，请选择 " **添加段**"。

1. 添加有关段的详细信息。 
1. 选择 " **设置子网** " 下的值以添加或修改子网。

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="添加 DHCP 服务器" border="true":::

1. 选择垂直省略号，然后选择 " **编辑**"。 如果需要创建新的子网，请选择 " **添加子网** " 来创建网关并配置 DHCP 范围。 
1. 提供 IP 池的范围并选择 "**应用**"，然后选择 "**保存**"

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="添加 DHCP 服务器" border="true":::

   DHCP 服务器池分配给段。

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="添加 DHCP 服务器" border="true":::
