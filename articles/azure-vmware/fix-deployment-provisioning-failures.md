---
title: 获取有关 Azure VMware 解决方案部署或预配失败的帮助
description: 如何从 Azure VMware 解决方案私有云获取所需信息，以便为 Azure VMware 解决方案部署或预配故障提供服务请求。
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 1f46dde895db417fd2b488a6203d5482e73d3c5e
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92779486"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>获取有关 Azure VMware 解决方案部署或预配失败的帮助

本文将帮助你进行 Azure VMware 解决方案部署和预配失败。 如果你的私有云发生故障，你将需要在 Azure 门户中 (SR) [提出支持请求](https://rc.portal.azure.com/#create/Microsoft.Support) 。 

但首先，您需要在 Azure 门户中收集一些关键信息：

- 相关性 ID
- ExpressRoute 线路 ID

## <a name="collect-the-correlation-id"></a>收集相关 ID
 
当你在 Azure 中创建私有云或任何资源时，将生成相关 ID。 每个 Azure 资源管理器部署还会生成相关 ID。 此 ID 可实现更快的 SR 创建和解决。 
 
下面是失败的私有云部署的输出示例，其中突出显示了相关 ID。

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="具有相关 ID 的私有云部署失败。":::

复制并保存要包含在服务请求中的相关 ID。 有关详细信息，请参阅本文末尾的 [创建支持请求](#create-your-support-request) 。

如果预验证阶段失败，则不会生成相关 ID。 在这种情况下，你可以提供创建 Azure VMware 解决方案私有云时使用的信息，包括：

- 位置
- 资源组
- 资源名称
 
### <a name="collect-a-summary-of-errors"></a>收集错误摘要

任何错误的详细信息还可帮助解决问题。 在上面的屏幕中，选择 "警告" 消息以查看错误的摘要。
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="具有相关 ID 的私有云部署失败。":::

同样，复制并保存此摘要，并将其保存到 SR 中。
 
### <a name="retrieve-past-deployments"></a>检索过去的部署

通过选择 "通知" 图标，可以在访问的部署活动日志中搜索，以检索过去的部署，包括失败的部署。

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="具有相关 ID 的私有云部署失败。" 中的 "更多事件"** 。

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="具有相关 ID 的私有云部署失败。":::

然后搜索用于创建资源的资源的名称，以查找失败的部署及其相关 ID。 以下示例显示 (pc03) 的私有云资源上的搜索结果。
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="具有相关 ID 的私有云部署失败。" 选项卡，然后查找 correlationId。 Copy 和 include in。 
 
## <a name="collect-the-expressroute-id-uri"></a>收集 ExpressRoute ID (URI) 
 
您可能正在尝试使用私有云 ExpressRoute 线路来扩展或对现有私有云进行缩放，并且失败。 在这种情况下，需要 ExpressRoute ID。 

在 Azure 门户中，选择 " **连接" > ExpressRoute** ，并将 **ExpressRoute ID** 复制到剪贴板。
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="具有相关 ID 的私有云部署失败。" [收集错误摘要](#collect-a-summary-of-errors)"。

## <a name="create-your-support-request"></a>创建支持请求

有关创建支持请求的一般指南，请参阅 [如何创建 Azure 支持请求](../azure-portal/supportability/how-to-create-azure-support-request.md)。 

下面是有关为 Azure VMware 解决方案部署或预配失败创建 SR 的特定指导。

1. 选择 " **帮助** " 图标，然后选择 " **+ 新建支持请求** "。

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="具有相关 ID 的私有云部署失败。" 来创建 SR。
