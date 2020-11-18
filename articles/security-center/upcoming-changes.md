---
title: 即将推出的对 Azure 安全中心的重要更改
description: 即将推出的对 Azure 安全中心的更改，你可能需要了解这些更改并针对这些更改做好计划
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2020
ms.author: memildin
ms.openlocfilehash: df863372cbf7abfb6fee145b7d13bb00d8deb074
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380155"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>即将推出的对 Azure 安全中心的重要更改

> [!IMPORTANT]
> 此页上的信息与预发行的产品或功能相关，这些产品或功能在正式发布之前可能会进行重大修改（如果有的话）。 对于此处提供的信息，Microsoft 不做任何承诺或者明示或暗示的保证。

在此页上，你将了解针对安全中心计划的更改。 此页介绍对产品所做的计划性修改，这些修改可能会影响安全功能分数或工作流之类的内容。

如果要查找最新的发行说明，可查看 [Azure 安全中心的新增功能](release-notes.md)。


## <a name="planned-changes"></a>计划的更改

### <a name="system-updates-should-be-installed-on-your-machines-recommendation-getting-sub-recommendations"></a>“应在计算机上安装系统更新”建议获取子建议

#### <a name="summary"></a>摘要

| 方面 | 详细信息 |
|---------|---------|
|公布日期 | 2020 年 11 月 9 日  |
|进行此更改的日期  |  2020 年 11 月中旬 |
|影响     | 在从此建议的当前版本到替换版本的过渡过程中，安全分数可能会更改。 |
|  |  |

我们要发布“应在计算机上安装系统更新”建议的增强版本。 新版本将替换应用系统更新安全控件中的当前版本，并提供以下改进：

- 为每个缺失更新提供了子建议
- 重新设计了 Azure 门户的 Azure 安全中心页面的体验
- 丰富了 Azure Resource Graph 的建议数据

#### <a name="transition-period"></a>过渡期

过渡期为 36 小时（大约）。 为了最大限度地减少任何潜在的中断，我们已安排更新在周末进行。 在过渡过程中，安全分数可能会受到影响。

#### <a name="redesigned-portal-experience"></a>重新设计的门户体验

“应在计算机上安装系统更新”的建议详细信息页包括发现结果列表，如下所示。 选择单个发现结果时，结果窗格将打开，并提供指向修正信息和受影响资源列表的链接。

:::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="在门户体验中打开其中一个子建议，了解更新的建议":::


#### <a name="richer-data-from-azure-resource-graph"></a>Azure Resource Graph 中更丰富的数据

Azure Resource Graph 是 Azure 中的一项服务，旨在提供高效的资源浏览。 可以使用 ARG 在一组给定的订阅中进行大规模查询，以便有效地控制环境。 

对于 Azure 安全中心，你可以使用 ARG 和 [Kusto 查询语言 (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) 来查询各种安全状态数据。

如果查询当前版本的“应在计算机上安装系统更新”，ARG 提供的唯一信息是，需要在计算机上修正建议。 发布更新版本后，以下查询将返回按计算机分组的每个缺失的系统更新。

```kusto
securityresources
| where type =~ "microsoft.security/assessments/subassessments"
| where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
| where properties.status.code == "Unhealthy"
```

## <a name="next-steps"></a>后续步骤

有关产品的所有最新更改，请参阅 [Azure 安全中心的新增功能](release-notes.md)。