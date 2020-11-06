---
title: 用无服务器 SQL 池处理的数据
description: 本文档介绍使用无服务器 SQL 池查询 Azure 存储中的数据时如何计算数据处理方式。
services: synapse analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 11/05/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 06eb02aa3dd4d5fc8bd3605dac480d5afa52d5fa
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423951"
---
# <a name="data-processed-with-serverless-sql-pool-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中用无服务器 SQL 池处理的数据

处理的数据是指在执行查询时暂时存储在系统中的数据量，其中包含：

- 从存储中读取的数据量–其中包括：
  - 读取数据时读取的数据量
  - 读取包含元数据的文件格式 (的数据时读取的数据量，如 Parquet) 
- 中间结果中的数据量–查询执行期间在节点之间传输的数据，包括以未压缩格式传输到终结点的数据。 
- 写入存储的数据量–如果使用 CETAS 将结果集导出到存储，则需要为已写出的字节和为 CETAS 的 SELECT 部分处理的数据量收费。

从存储读取文件是高度优化的，使用：

- 预提取-这可能会增加读取数据量的少量开销。 如果查询读取整个文件，则不会产生任何开销。 如果文件部分读取（如前 N 个查询中的文件），将使用预提取读取更多的数据。
- 优化 CSV 分析器–如果使用 PARSER_VERSION = ' 2.0 ' 来读取 CSV 文件，将导致从存储中读取的数据量略有增加。  优化的 CSV 分析器以大小相等的块以并行方式读取文件。 不保证块区将包含整行。 为了确保分析所有行，还将读取相邻块区的小片段，并添加少量的开销。

## <a name="statistics"></a>统计信息

无服务器 SQL 池查询优化器根据统计信息生成最佳查询执行计划。 可以手动创建统计信息，也可以通过无服务器 SQL 池自动创建统计信息。 无论采用哪种方式，都可以执行单独的查询来创建统计信息，该查询将返回提供的采样率的特定列。 此查询已处理关联的数据量。

如果你运行的是相同的或任何其他将从创建的统计信息中受益的查询，则会在可能的情况下重复使用统计信息，而不会为创建统计信息而处理其他数据。

为 Parquet 列创建统计信息将导致仅读取文件中的相关列。 为 CSV 列创建统计信息将导致读取和分析全部文件。

## <a name="rounding"></a>舍入

处理的数据量将向上舍入到最接近的每个查询 MB，每个查询至少处理 10 MB 的数据。

## <a name="what-is-not-included-in-data-processed"></a>处理的数据中未包含的内容

- 服务器级别的元数据 (例如登录名、角色、服务器级凭据) 
- 在终结点中创建的数据库，因为这些数据库仅包含元数据 (例如用户、角色、架构、视图、内联 Tvf、存储过程、数据库范围的凭据、外部数据源、外部文件格式、外部表) 
  - 如果使用架构推理，将读取文件的片段以推断列名称和数据类型
- DDL 语句（创建统计信息，因为它将基于指定的样本百分比从存储处理数据）
- 仅元数据查询

## <a name="reduce-amount-of-data-processed"></a>减少处理的数据量

你可以优化每个查询处理的数据量，并通过将数据分区和转换为压缩的列式格式（例如 Parquet）来获得更好的性能。

## <a name="examples"></a>示例

假设有两个表，其中每个表都在五个大小相同的列中具有相同的数据：

- 由 5 TB CSV 文件支持 population_csv 表
- 由 1 TB 的 Parquet 文件支持 population_parquet 表–此表小于上一个表，因为 Parquet 包含压缩数据
- 100 KB 的 CSV 文件支持 very_small_csv 表

**查询 #1** ：从 POPULATION_CSV 选择总计 (填充) 

此查询将读取并分析整个文件，以获取填充列的值。 节点将处理此表的片段，每个片段的总体总和将在节点之间传输，最终的总和将转移到你的终结点。 此查询将处理 5 TB 的数据，并处理用于传输碎片总和的小开销。

**查询 #2** ：从 POPULATION_PARQUET 选择总计 (填充) 

查询压缩和面向列的格式（如 Parquet）会导致读取的数据比上一个查询中的数据少，因为无服务器 SQL 池将读取单个压缩列而不是整个文件。 在这种情况下，将读取 0.2 TB (五个大小相等的列，每) 0.2 TB。 节点将处理此表的片段，每个片段的总体总和将在节点之间传输，最终的总和将转移到你的终结点。 此查询将处理 0.2 TB 加上碎片的总和的少量开销。

**查询 #3** ： SELECT * FROM population_parquet

此查询将读取所有列并以未压缩格式传输所有数据。 如果压缩格式为5:1，它将处理 6 TB，因为它将读取 1 TB + 传输 5 TB 未压缩的数据。

**查询 #4** ：从 very_small_csv 中选择计数 ( * ) 

此查询将读取整个文件。 此表的存储中文件的总大小为 100 KB。 节点将处理此表的片段，每个片段的总和将在节点之间传输，最终的总和将转移到你的终结点。 此查询将处理的数据略超过 100 KB。 对于此查询，处理的数据量将向上舍入到 [舍入](#rounding)中指定的 10 MB。

## <a name="next-steps"></a>后续步骤

若要了解如何优化查询的性能，请查看 [无服务器 SQL 池的最佳实践](best-practices-sql-on-demand.md)。
