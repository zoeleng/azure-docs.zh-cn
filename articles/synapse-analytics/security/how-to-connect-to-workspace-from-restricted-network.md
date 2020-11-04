---
title: 从受限网络连接到 Synapse Studio 工作区资源
description: 本文介绍如何从受限网络连接到 Azure Synapse Studio 工作区资源
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: 5d28b8f2ff3045c9fdf5e8a866419a22bfbc6504
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321762"
---
# <a name="connect-to-synapse-studio-workspace-resources-from-a-restricted-network"></a>从受限网络连接到 Synapse Studio 工作区资源

本文的目标读者是公司的 IT 管理员，负责管理公司的受限网络。 IT 管理员要在 Azure Synapse Studio 与此受限网络内的工作站之间启用网络连接。

本文介绍如何从受限网络环境连接到 Azure Synapse 工作区。 

## <a name="prerequisites"></a>先决条件

* **Azure 订阅** ：如果还没有 Azure 订阅，可以在开始前创建一个 [免费 Azure 帐户](https://azure.microsoft.com/free/)。
* **Azure Synapse 工作区** ：如果没有 Synapse Studio，请从 Azure Synapse Analytics 创建 Synapse 工作区。 以下步骤4中将需要工作区名称。
* **受限网络** ：受限网络由公司 IT 管理员维护。 it 管理员有权配置网络策略。 下面的步骤3中将需要虚拟网络名称及其子网。



## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>步骤1：向受限网络添加网络出站安全规则

需要添加四个包含四个服务标记的网络出站安全规则。 详细了解 [服务标记概述](/azure/virtual-network/service-tags-overview.md) 
* AzureResourceManager
* AzureFrontDoor.Frontend
* AzureActiveDirectory
* AzureMonitor (可选。 仅当你想要将数据与 Microsoft 共享时，才添加此类型的规则。 ) 

**Azure 资源管理器** 如下所示的出站规则详细信息。 创建其他三个规则时，请将 " **目标服务标记** " 的值替换为从下拉选择列表中选择服务标记名称 " **AzureFrontDoor** "、" **AzureActiveDirectory** " 和 " **AzureMonitor** "。

![AzureResourceManager](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)


## <a name="step-2-create-azure-synapse-analytics-private-link-hubs"></a>步骤2：创建 Azure Synapse Analytics (专用链接中心) 

需要从 Azure 门户中创建一个 Azure Synapse Analytics (专用链接中心) 。 通过 Azure 门户搜索 " **Azure Synapse Analytics (专用链接中心)** "，然后填写所需字段并进行创建。 

> [!Note]
> 该区域应与 Synapse 工作区所在的区域相同。

![创建 Synapse Analytics 专用链接中心](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-private-link-endpoint-for-synapse-studio-gateway"></a>步骤3：创建 Synapse Studio 网关的专用链接终结点

若要访问 Synapse Studio 网关，需要从 Azure 门户创建专用链接端点。 通过 Azure 门户搜索 " **专用链接** "。 选择 " **专用链接中心** " 中的 " **创建专用终结点** "，并填写所需字段并创建它。 

> [!Note]
> 该区域应与 Synapse 工作区所在的区域相同。

![正在为 Synapse studio 1 创建专用终结点](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

在 " **资源** " 的下一个选项卡中，选择在上面的步骤2中创建的专用链接中心。

![正在为 Synapse studio 2 创建专用终结点](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

在 " **配置** " 的下一个选项卡中， 
* 选择 " **虚拟网络** " 所用的受限制的虚拟网络名称。
* 为 " **子网** " 选择受限虚拟网络的子网。 
* 选择 " **是** " 以 " **与专用 DNS 区域集成** "。

![正在为 Synapse studio 3 创建专用终结点](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

创建专用链接终结点后，可以访问 Synapse studio web 工具的登录页。 但是，在需要完成下一步之前，你将无法访问 Synapse 工作区内的资源。

## <a name="step-4-create-private-link-endpoints-for-synapse-studio-workspace-resource"></a>步骤4：创建 Synapse Studio 工作区资源的专用链接终结点

若要访问 Synapse Studio 工作区资源中的资源，需要至少创建一个 **具有 "****目标子资源** " 类型的专用链接终结点，并使用 " **Sql** " 或 " **SqlOnDemand** " 类型的两个其他可选专用链接终结点，具体取决于要访问的 Synapse Studio 工作区中的资源。 此用于 Synapse studio 工作区的专用链接终结点类似于创建终结点。  

请注意 " **资源** " 选项卡中以下各区域：
* 选择 " **Synapse/工作区** " 以 " **资源类型** "。
* 选择之前创建的 " **资源** " " **YourWorkSpaceName** "。
* 在 " **目标子资源** " 中选择终结点类型：
  * **Sql** ：用于 sql 池中的 sql 查询执行。
  * **SqlOnDemand** ：用于 SQL 内置查询执行。
  * **Dev** ：用于访问 Synapse Studio 工作区中的其他所有内容。 需要至少创建具有此类型的专用链接端点。

![正在为 Synapse studio 工作区创建专用终结点](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)

现在，全部设置。 可以访问 Synapse studio 工作区资源。

## <a name="next-steps"></a>后续步骤

了解有关[托管工作区虚拟网络](./synapse-workspace-managed-vnet.md)的详细信息

详细了解[托管专用终结点](./synapse-workspace-managed-private-endpoints.md)
