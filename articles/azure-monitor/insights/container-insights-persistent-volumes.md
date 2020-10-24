---
title: 配置针对容器 Azure Monitor 的 PV 监视 |Microsoft Docs
description: 本文介绍如何配置 Kubernetes 群集，其中包含具有容器 Azure Monitor 的持久卷。
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 30e99c2abbc66de257f9623dedc901fca51976c1
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92492149"
---
# <a name="configure-pv-monitoring-with-azure-monitor-for-containers"></a>为容器 Azure Monitor 配置 PV 监视

从代理版本 *ciprod10052020*开始，适用于容器的 Azure monitor 集成代理现在支持监视 PV (持久卷) 使用情况。

## <a name="pv-metrics"></a>PV 指标

容器 Azure Monitor 自动开始监视 PV，方法是按60sec 间隔收集以下指标，并将其存储在 **InsightMetrics** 表中。

|指标名称 |指标维度（标记） |说明 |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|特定永久性卷的已用空间（以字节为单位），具有特定 pod 使用的声明。 `pvCapacityBytes` 作为 "标记" 字段中的维度折叠，以减少数据引入成本并简化查询。|

## <a name="monitor-persistent-volumes"></a>监视永久性卷

容器 Azure Monitor 在每个群集的工作簿中包括此指标的预配置图表。 通过从左侧窗格中选择工作簿，并从见解中的 "**查看工作簿**" 下拉列表中选择工作簿，可以直接从 AKS 群集在 "**工作负荷详细信息**" 工作簿的 "永久卷" 选项卡中找到图表。 你还可以针对 PV 使用启用建议的警报，并在 Log Analytics 中查询这些指标。  

![Azure Monitor PV 工作负荷工作簿示例](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>后续步骤

- [在此处](https://aka.ms/ci/pvconfig)了解有关收集的 PV 指标的详细信息。
