---
title: 用于开发 Synapse SQL 功能的资源
description: Synapse SQL 的开发概念、设计决策、建议和编码技术。
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 2a24a61e26bdfaba0619e3965e95b1828380a1c8
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322111"
---
# <a name="design-decisions-and-coding-techniques-for-synapse-sql-features-in-azure-synapse-analytics"></a>Azure Synapse Analytics 中 Synapse SQL 功能的设计决策和编码方法
在本文中，你将找到专用 SQL 池的资源列表和无服务器 SQL 池 (预览) Synapse 的函数。 建议的文章分为两部分：关键设计决策和开发和编码技术。

这些文章旨在帮助您为 Synapse 分析中的 Synapse SQL 组件开发最佳的技术方法。

## <a name="key-design-decisions"></a>关键设计决策
下面的文章重点介绍了 Synapse SQL 开发的概念和设计决策：

| 文章 | 专用 SQL 池 | 无服务器 SQL 池 |
| ------- | -------- | ------------- |
| [连接](connect-overview.md)                    | 是 | 是 |
| [资源类和并发](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | 是    | 否 |
| [事务](develop-transactions.md)              | 是 | 否 |
| [用户定义的架构](develop-user-defined-schemas.md) | 是 | 是 |
| [表分布](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                 | 是 | 否 |
| [表索引](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                           | 是 | 否 |
| [表分区](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                     | 是 | 否 |
| [统计信息](develop-tables-statistics.md)            | 是 | 是 |
| [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)                                             | 是 | 否 |
| [外部表](develop-tables-external-tables.md) | 是 | 是 |
| [CETAS](develop-tables-cetas.md)                     | 是 | 是 |


## <a name="recommendations"></a>建议

下面是一些重要的文章，这些文章强调了用于开发的特定编码方法、提示和建议：

| 文章 | 专用 SQL 池 | 无服务器 SQL 池 |
| ------- | -------- | ------------- |
| [存储过程](develop-stored-procedures.md)  | 是                | 否                      |
| [标签](develop-label.md)                           | 是                | 否                      |
| [Views](develop-views.md)                             | 是                | 是                     |
| [临时表](develop-tables-temporary.md)       | 是                | 是                     |
| [动态 SQL](develop-dynamic-sql.md)                 | 是                | 是                     |
| [循环](develop-loops.md)                         | 是                | 是                     |
| [Group by 选项](develop-group-by-options.md)       | 是                | 否                      |
| [变量赋值](develop-variable-assignment.md) | 是                | 是                     |

## <a name="next-steps"></a>后续步骤
有关更多参考信息，请参阅 [SQL Pool t-sql 语句](../sql-data-warehouse/sql-data-warehouse-reference-tsql-statements.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)。

