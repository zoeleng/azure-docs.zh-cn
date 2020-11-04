---
title: Azure Synapse Analytics（以前称为 SQL DW）体系结构
description: 了解 Azure Synapse Analytics 如何 (以前的 SQL DW) 将分布式查询处理功能与 Azure 存储结合起来，实现高性能和可伸缩性。
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d32aa011e9e816f97b050d43f9558af0cf82e90
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93319654"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-architecture"></a>Azure Synapse Analytics（以前称为 SQL DW）体系结构

Azure Synapse 是一种无限制的分析服务，它将企业数据仓库和大数据分析结合在一起。 借助它可以使用无服务器的按需资源或预配资源，任意执行自己定义的大规模数据查询。 Azure Synapse 将这两个领域结合在一起，以统一的体验引入、准备、管理和提供数据，以满足即时 BI 和机器学习的需求。

 Azure Synapse 包含四个组件：

- Synapse SQL：基于 T-SQL 的完整分析

  - 专用 SQL 池 (按预配) 按预配付费–已正式发布
  - 无服务器 SQL 池 (每 TB 支付的已处理) – (预览) 
- Spark：深度集成的 Apache Spark（预览）
- 数据集成：混合数据集成（预览）
- 工作室：统一的用户体验。  （预览版）

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="synapse-sql-architecture-components"></a>Synapse SQL 体系结构组件

[Synapse SQL](sql-data-warehouse-overview-what-is.md#dedicated-sql-pool-in-azure-synapse) 利用横向扩展体系结构在多个节点间分布数据的计算处理。 缩放单位是计算能力（称为[数据仓库单位](what-is-a-data-warehouse-unit-dwu-cdwu.md)）的抽象概念。 计算与存储分离开来，以便用户能够独立于系统中的数据进行缩放计算。

![Synapse SQL 体系结构](./media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

Synapse SQL 使用基于节点的体系结构。 应用程序将 T-SQL 命令连接到、发布给控制节点，该节点是 Synapse SQL 的单一入口点。 控制节点承载分布式查询引擎，该引擎优化查询以进行并行处理，然后将操作传递到计算节点以并行完成工作。

计算节点将所有用户数据存储在 Azure 存储中并运行并行查询。 数据移动服务 (DMS) 是一项系统级内部服务，它根据需要在节点间移动数据以并行运行查询和返回准确的结果。

如果使用了分离的存储和计算，则在使用 Synapse SQL 池时，可以：

- 无论存储需求如何，都可独立计算大小。
- 在 SQL 池（数据仓库）内增大或缩小计算能力，而无需移动数据。
- 在保持数据不受影响的情况下暂停计算容量，因此只需为存储付费。
- 在操作期间恢复计算容量。

### <a name="azure-storage"></a>Azure 存储

Synapse SQL 使用 Azure 存储保护用户数据。  由于数据通过 Azure 存储进行存储和管理，因此会对存储消耗单独收费。 将数据分片到“分布区”中来优化系统性能。 可选择在定义表时用于分布数据的分片模式。 支持以下分片模式：

- 哈希
- 循环
- 复制

### <a name="control-node"></a>控制节点

控制节点是体系结构的核心。 它是与所有应用程序和连接进行交互的前端。 分布式查询引擎在控制节点上运行以优化和协调并行查询。 提交 T-SQL 查询时，控制节点会将其转换为可针对每个分布区并行运行的查询。

### <a name="compute-nodes"></a>计算节点

计算节点提供计算能力。 分布区映射到计算节点以进行处理。 当你为更多计算资源付费时，分布区将重新映射到可用的计算节点。 计算节点数的范围是 1 到 60，它由 Synapse SQL 的服务级别确定。

每个计算节点均有一个节点 ID，该 ID 会显示在系统视图中。 在名称以 sys.pdw_nodes 开头的系统视图中找到 node_id 列即可查看计算节点 ID。 有关这些系统视图的列表，请参阅 [SYNAPSE SQL 系统视图](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。

### <a name="data-movement-service"></a>数据移动服务

数据移动服务 (DMS) 是一项数据传输技术，它可协调计算节点间的数据移动。 某些查询需要移动数据以确保并行查询返回准确的结果。 需要移动数据时，DMS 可确保正确的数据到达正确的位置。

## <a name="distributions"></a>分发

分布区是存储和处理针对分布式数据运行的并行查询的基本单位。 Synapse SQL 运行查询时，工作会被分割成 60 个并行运行的小型查询。

每个小型查询各在一个数据分布区上运行。 每个计算节点管理其中一个或多个分布区。 具有最多计算资源的 SQL 池的每个分布区占 1 个计算节点。 具有最少计算资源的 SQL 池的所有分布区都在 1 个计算节点上。  

## <a name="hash-distributed-tables"></a>哈希分布表

哈希分布表可为大型表上的联接和聚合提供最高查询性能。

为了将数据分片到哈希分布式表中，使用哈希函数明确将 1 个行分配到 1 个分布区。 在表定义中，可以将一个列指定为分布列。 哈希函数使用分布列中的值将 1 个行分配到 1 个分布区。

下图说明了如何将完整的非分布式表存储为哈希分布表。

![分布式表](./media/massively-parallel-processing-mpp-architecture/hash-distributed-table.png "分布式表")  

- 一个行属于一个分布区。  
- 通过确定性哈希算法将一个行分配到一个分布区。  
- 不同大小的表显示，每个分布区的表行的数目各不相同。

选择分布列时需考虑到性能，例如特异性、数据倾斜，以及在系统上运行的查询类型。

## <a name="round-robin-distributed-tables"></a>轮循分布表

轮循机制表是最简单的表，在被用作负载临时表时，它可创造和提供高速性能。

轮循机制分布表在表中均匀分布数据，但不会进行进一步优化。 首先随机选择一个分布区，然后将行的缓冲区按顺序分配给分布区。 将数据加载到轮循机制表速度很快，但就查询性能而言，哈希分布式表的性能更佳。 轮循机制表上的联接要求重新安排数据，这需要花费更多时间。

## <a name="replicated-tables"></a>复制表

复制表为小型表提供最快查询性能。

复制表在每个计算节点上缓存表的完整副本。 因此复制表以后，无需在执行联接或聚合前在计算节点中间传输数据。 复制表尤为适用于小型表。 它需要额外存储并且在写入数据时会产生额外开销，因此不适用于大型表。  

下图显示会在每个计算节点的第一个分布区上缓存复制表。  

![复制表](./media/massively-parallel-processing-mpp-architecture/replicated-table.png "复制表")

## <a name="next-steps"></a>后续步骤

对 Azure Synapse 有了初步的认识后，了解如何快速[创建 SQL 池](create-data-warehouse-portal.md)和[加载示例数据](load-data-from-azure-blob-storage-using-polybase.md)。 如果不熟悉 Azure，遇到新术语时，[Azure 词汇表](../../azure-glossary-cloud-terminology.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) 可以提供帮助。 或者，查看以下一些其他 Azure Synapse 资源。  

- [客户成功案例](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [博客](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [功能请求](https://feedback.azure.com/forums/307516-sql-data-warehouse)
- [视频](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [创建支持票证](sql-data-warehouse-get-started-create-support-ticket.md)
- [Microsoft Q&A 问题页面](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html)
- [Stackoverflow 论坛](https://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
