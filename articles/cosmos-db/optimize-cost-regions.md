---
title: 在 Azure Cosmos DB 中优化多区域部署的成本
description: 本文说明如何在 Azure Cosmos DB 中管理多区域部署的成本。
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2020
ms.openlocfilehash: 010ca40f4f3aacd6353aecd150e944672cc09066
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097496"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中优化多区域成本
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

可随时对 Azure Cosmos 帐户添加或删除区域。 在与帐户关联的每个区域中会保留为各种 Azure Cosmos 数据库和容器配置的吞吐量。 如果每小时预配的吞吐量为你的 Azure Cosmos 帐户的所有数据库和容器中配置的 RU/s 的总和， `T` 以及与你的数据库帐户关联的 Azure 区域的数量为 `N` ，则给定小时的 Cosmos 帐户的总预配吞吐量为等于 `T x N RU/s` 。

具有单个写入区域的预配吞吐量每 100 RU/秒的成本为 0.008 美元/小时，具有多个可写区域的预配吞吐量每 100 RU/秒的成本为 0.016 美元/小时。 若要了解详细信息，请参阅 Azure Cosmos DB [定价页](https://azure.microsoft.com/pricing/details/cosmos-db/)。

## <a name="costs-for-multiple-write-regions"></a>多个写入区域的成本

在多区域写入系统中，写入操作的 net 可用 ru 增加了 `N` `N` 写入区域数目的时间。 与单区域写入不同，每个区域现在都是可写的，并且支持冲突解决。 从成本规划的角度来看，若要在 `M` 全球范围内执行 RU/秒的写入，则需要 `RUs` 在容器或数据库级别预配 M。 随后可以添加任意多个区域并将它们用于写入以在全球范围执行 `M` RU 写入。

### <a name="example"></a>示例

考虑到美国西部中的一个容器配置为单区域写入，使用吞吐量 10K RU/s 进行预配，并存储了每月 1 TB 的数据。 假设你使用相同的存储和吞吐量添加一个区域 "美国东部"，你希望能够从应用程序的两个区域写入容器。 一个月的总月度帐单（假定为 31 天）如下所示：

|**Item**|**使用情况（每月）**|**费率**|**每月成本**|
|----|----|----|----|
|"美国西部" 容器的吞吐量帐单 (单个写入区域)  |10K RU/秒 * 24 小时 * 31 天 |每小时 $0.008 每个 100 RU/秒 |$584.06 |
|2个区域中的容器的吞吐量帐单-美国西部 & 美国东部 (多个写入区域)  |2 * 10K RU/秒 * 24 小时 * 31 天|每小时每 100 RU/s 为 0.016 美元 |$2336.26 |
|美国西部容器的存储帐单 |1 TB（或 1,024 GB） |0\.25 美元/GB |$256 |
|2个区域的存储帐单-美国西部 & 美国东部 |2 * 1 TB (或 3072 GB)  |0\.25 美元/GB |$768 |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>按每个区域提高吞吐量利用率

例如，如果您的利用率低下，例如一个或多个充分利用的读取区域，则可以通过使用读取区域的 "更改源" 或将其移动到另一个辅助副本（如果过度利用），采取措施来充分利用读取区域中的 ru。 你将需要确保先优化预配吞吐量 (在写入区域中) 。 写入除读取之外的开销，除非非常大的查询，因此维持利用率可能会很困难。 整体上，监视区域中已消耗的吞吐量，并按需添加或删除区域，以扩展读写吞吐量，从而确保了解在同一区域中部署的任何应用的延迟影响。

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章详细了解 Azure Cosmos DB 中的成本优化：

* 详细了解[开发和测试优化](optimize-dev-test.md)
* 详细了解[了解 Azure Cosmos DB 帐单](understand-your-bill.md)
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](./optimize-cost-reads-writes.md)