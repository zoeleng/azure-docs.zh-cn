---
title: 快速入门：从 VM 创建 Bastion 主机并通过专用 IP 地址连接
titleSuffix: Azure Bastion
description: 本快速入门文章介绍如何从虚拟机创建 Azure Bastion 主机并使用专用 IP 地址进行安全连接。
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 6f451e7b115c00bc7b2cf350e00b9f704ab1d29f
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019046"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>快速入门：使用专用 IP 地址和 Azure Bastion 连接到虚拟机

本快速入门文章介绍如何使用 Azure Bastion 和 Azure 门户通过浏览器连接到虚拟机。 在 Azure 门户中，你可以从 Azure VM 将 Bastion 部署到虚拟网络中。 部署 Bastion 之后，可以使用 Azure 门户通过 VM 的专用 IP 地址连接到该 VM。 你的 VM 不需要公共 IP 地址或特殊软件。 直接从 VM 为 VNet 创建 Bastion 主机的一个优点是，已预填充了许多设置。

预配服务后，RDP/SSH 体验即可用于同一虚拟网络中的所有虚拟机。 有关 Azure Bastion 的详细信息，请参阅[什么是 Azure Bastion](bastion-overview.md)。

## <a name="prerequisites"></a><a name="prereq"></a>先决条件

* 一个虚拟网络。
* 虚拟网络中的 Windows 虚拟机。
* 需要以下角色：
  * 虚拟机上的读者角色。
  * NIC 上的读者角色（使用虚拟机的专用 IP）。

* 端口：若要连接到 VM，必须在 VM 上打开以下端口：
  * 入站端口：RDP (3389)

### <a name="example-values"></a>示例值

|**名称** | **值** |
| --- | --- |
| 名称 |  TestVNet1-bastion |
| 虚拟网络 |  TestVNet1（基于 VM） |
| + 子网名称 | AzureBastionSubnet |
| AzureBastionSubnet 地址 |  10.1.254.0/27 |
| 公共 IP 地址 |  新建 |
| 公共 IP 地址名称 | VNet1BastionPIP  |
| 公用 IP 地址 SKU |  Standard  |
| 分配  | 静态 |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>创建 Bastion 主机

使用现有虚拟机在 Azure 门户中创建 Bastion 主机时，各种设置将自动默认为与虚拟机和/或虚拟网络相对应。

1. 打开 [Azure 门户](https://portal.azure.com)。 转到虚拟机，然后选择“连接”。

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="虚拟机设置" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. 在下拉列表中，选择“Bastion”。
1. 在“TestVM | 连接”页上，选择“使用 Bastion” 。

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="虚拟机设置" border="false":::

1. 在“Bastion”页上，填写以下设置字段：

   * 名称：为 Bastion 主机命名。
   * **子网**：虚拟网络中的子网，将在其中部署 Bastion 资源。 必须使用名称 AzureBastionSubnet 创建子网。 此名称告知 Azure 要将 Bastion 资源部署到哪个子网。 这不同于网关子网。 使用至少为 /27 或更大（/27、/26、/25 等）的子网。
   
      * 选择“管理子网配置”。
      * 选择“AzureBastionSubnet”。
      * 如有必要，请调整以 CIDR 表示法表示的地址范围。 例如 10.1.254.0/27。
      * 请勿调整任何其他设置。 如果要接受并保存子网更改，选择“确定”；如果不想进行任何更改，选择页面顶部的“x” 。
1. 单击浏览器上的后退按钮，导航回“Bastion”页，然后继续指定值。
   * **公共 IP 地址名称**：公共 IP 地址资源的名称。
   * **公共 IP 地址**：这是要在其上通过端口 443 访问 RDP/SSH 的 Bastion 资源的公共 IP。 创建新的公共 IP。
1. 选择“创建”以创建 Bastion 主机。 Azure 会验证设置，然后创建主机。 主机和其资源的创建及部署大约需要 5 分钟。

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="虚拟机设置":::

## <a name="connect"></a><a name="connect"></a>连接

在将 Bastion 部署到虚拟网络后，屏幕切换到连接页面。

1. 键入虚拟机的用户名和密码。 然后，选择“连接”。

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="虚拟机设置":::
1. 通过 Bastion 连接到此虚拟机的 RDP 将使用端口 443 和 Bastion 服务在 Azure 门户中（通过 HTML5）直接打开。

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="虚拟机设置":::

## <a name="clean-up-resources"></a>清理资源

使用完虚拟网络和虚拟机之后，请删除资源组和其包含的所有资源：

1. 在门户顶部的“搜索”框中输入资源组的名称，并从搜索结果中选择资源组。

1. 选择“删除资源组”  。

1. 在“键入资源组名称”中输入资源组名称，然后选择“删除” 。

## <a name="next-steps"></a>后续步骤

在本快速入门中，你为虚拟网络创建了一个 Bastion 主机，然后通过该 Bastion 主机安全连接到了虚拟机。 接下来，如果要连接到虚拟机规模集，可以继续执行以下步骤。

> [!div class="nextstepaction"]
> [使用 Azure Bastion 连接到虚拟机规模集](bastion-connect-vm-scale-set.md)
