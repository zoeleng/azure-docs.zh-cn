---
title: '使用 Azure Synapse 中的无服务器 SQL 池查询 Azure Cosmos DB 数据 (预览版) '
description: 本文介绍如何使用 Azure Synapse 中的 SQL 点播 (preview) 查询 Azure Cosmos DB。
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: c5fa326fa05a34ae5b51054b867a766489b85c16
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92670706"
---
# <a name="query-azure-cosmos-db-data-with-serverless-sql-pool-in-azure-synapse-link-preview"></a>在 Azure Synapse 链接 (预览版中利用无服务器 SQL 池查询 Azure Cosmos DB 数据) 

Synapse 无服务器 SQL 池 (以前的 SQL 点播) 使你能够以近实时的方式分析通过 [Azure Synapse 链接](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 启用的 Azure Cosmos DB 容器中的数据，而不会影响事务工作负荷的性能。 它提供了一种熟悉的 T-sql 语法，用于查询 [分析存储](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 中的数据，并通过 t-sql 接口集成到各种 BI 和即席查询工具。

对于查询 Azure Cosmos DB，可通过[OPENROWSET](develop-openrowset.md)函数（包括大多数[SQL 函数和运算符](overview-features.md)）支持完整的[SELECT](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15) surface area。 还可以存储从 Azure Cosmos DB 读取数据的查询的结果，以及 Azure Blob 存储中的数据，或使用 [create external table as select](develop-tables-cetas.md#cetas-in-sql-on-demand)Azure Data Lake Storage。 当前无法使用 [CETAS](develop-tables-cetas.md#cetas-in-sql-on-demand)将无服务器 SQL 池查询结果存储到 Azure Cosmos DB。

在本文中，你将学习如何编写包含无服务器 SQL 池的查询，该查询将从启用 Synapse 链接的 Azure Cosmos DB 容器中查询数据。 然后，你可以在 [本](./tutorial-data-analyst.md) 教程中详细了解如何通过 Azure Cosmos DB 容器构建无服务器 SQL 池视图并将其连接到 Power BI 模型。 

> [!IMPORTANT]
> 本教程使用一个容器，其中包含 [Azure Cosmos DB 定义完善的架构](../../cosmos-db/analytical-store-introduction.md#schema-representation) ，该架构提供未来将支持的查询体验。 无服务器 SQL 池为 [Azure Cosmos DB 完全保真架构](#full-fidelity-schema) 提供的查询体验是临时性的行为，将根据预览反馈进行更改。 不要依赖于 `OPENROWSET` 公共预览版期间函数为完全保真容器提供的架构，因为查询 experinece 可能会更改并与定义完善的架构对齐。 请联系 [Synapse 链接产品团队](mailto:cosmosdbsynapselink@microsoft.com) 提供反馈。

## <a name="overview"></a>概述

若要支持在 Azure Cosmos DB 分析存储中查询和分析数据，无服务器 SQL 池使用以下 `OPENROWSET` 语法：

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

Azure Cosmos DB 连接字符串指定 Azure Cosmos DB 帐户名称、数据库名称、数据库帐户主密钥以及要运行的可选区域名称 `OPENROWSET` 。 

> [!IMPORTANT]
> 请确保在后使用别名 `OPENROWSET` 。 如果未指定别名 after 函数，则会出现一个 [已知问题](#known-issues) ，导致 Synapse 无服务器 SQL 终结点出现连接问题 `OPENROWSET` 。

连接字符串具有以下格式：
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

指定了 Azure Cosmos DB 容器名称，但语法中没有引号 `OPENROWSET` 。 如果容器名称包含任何特殊字符 (例如，短划线 "-" ) ，则该名称应在 `[]` 语法的 () 方括号内进行包装 `OPENROWSET` 。

> [!NOTE]
> 无服务器 SQL 池不支持 Azure Cosmos DB 事务存储中进行查询。

## <a name="sample-data-set"></a>示例数据集

本文中的示例基于来自 [欧洲数据中心的数据， (ECDC) COVID-19 事例](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 和 [COVID-19 开放式研究数据集 (缆线) ，doi>10.1145： 10.5281/zenodo](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)。 

你可以在这些页面上查看许可证和数据的结构，并下载 [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) 和 [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) 数据集的示例数据。

若要按照本文中的说明，展示如何通过无服务器 SQL 池查询 Cosmos DB 数据，请确保创建以下资源：
* 已[启用 Synapse 链接](../../cosmos-db/configure-synapse-link.md)的 Azure Cosmos DB 数据库帐户
* 名为的 Azure Cosmos DB 数据库 `covid`
* 两个名为 `EcdcCases` 和的 Azure Cosmos DB 容器，它们 `Cord19` 加载了示例数据集。

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>利用自动架构推理浏览 Azure Cosmos DB 数据

在 Azure Cosmos DB 中浏览数据的最简单方法是利用自动架构推理功能。 通过 `WITH` 从语句中省略子句 `OPENROWSET` ，可以指示无服务器 SQL 池自动检测 (推断) Azure Cosmos DB 容器的分析存储的架构。

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
在上面的示例中，我们将指导无服务器 SQL 池连接到 `covid` Azure Cosmos DB 帐户 `MyCosmosDbAccount` 使用 Azure Cosmos DB 密钥进行身份验证的身份验证，) 上述示例中 (虚拟。 接下来，我们将访问 `EcdcCases` 该容器的分析存储区 `West US 2` 。 由于没有特定属性的投影，因此 `OPENROWSET` 函数将返回 Azure Cosmos DB 项中的所有属性。

如果需要从同一个 Azure Cosmos DB 数据库的另一个容器浏览数据，则可以使用相同的连接字符串，并将所需的容器引用为第三个参数：

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>显式指定架构

虽然中的自动架构推理功能 `OPENROWSET` 提供了简单易用的 querience，但你的业务方案可能要求你将架构从 Azure Cosmos DB 数据显式指定为只读相关属性。

`OPENROWSET` 允许您显式指定要从容器中的数据读取哪些属性以及指定其数据类型。 假设我们已从 [ECDC COVID 数据集将](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 一些数据导入到 Azure Cosmos DB 中的以下结构：

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Azure Cosmos DB 中的这些简单 JSON 文档可表示为 Synapse SQL 中的一组行和列。 `OPENROWSET` 函数可以指定要读取的属性的子集和子句中的确切列类型 `WITH` ：

```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

此查询的结果可能如下所示：

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

有关应该用于 Azure Cosmos DB 值的 SQL 类型的详细信息，请查看本文末尾的 [sql 类型映射规则](#azure-cosmos-db-to-sql-type-mappings) 。

## <a name="querying-nested-objects-and-arrays"></a>查询嵌套对象和数组

Azure Cosmos DB 允许通过将数据模型作为嵌套对象或数组进行组合来表示更复杂的数据模型。 用于 Azure Cosmos DB 的 Synapse 链接的 autosync 功能管理现成的分析存储中的架构表示形式，其中包括处理嵌套数据类型，允许从无服务器 SQL 池进行丰富的查询。

例如，" [缆线-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) " 数据集具有以下结构的 JSON 文档：

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

当函数读取 Azure Cosmos DB 中的嵌套对象和数组时，它们会在查询结果中表示为 JSON 字符串 `OPENROWSET` 。 从这些复杂类型读取值作为 SQL 列的一个选项是使用 SQL JSON 函数：

```sql
SELECT
    title = JSON_VALUE(metadata, '$.title'),
    authors = JSON_QUERY(metadata, '$.authors'),
    first_author_name = JSON_VALUE(metadata, '$.authors[0].first')
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( metadata varchar(MAX) ) AS docs;
```

此查询的结果可能如下所示：

| title | 作者 | first_autor_name |
| --- | --- | --- |
| 补充信息 a epidemi .。。 |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien |  

作为替代选项，还可以在 using 子句中指定对象中嵌套值的路径 `WITH` ：

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( title varchar(1000) '$.metadata.title',
           authors varchar(max) '$.metadata.authors'
    ) AS docs;
```

详细了解如何分析 [Synapse 链接中的复杂数据类型](../how-to-analyze-complex-schema.md) 和 [无服务器 SQL 池中的嵌套结构](query-parquet-nested-types.md)。

> [!IMPORTANT]
> 如果你的文本（而不是）中出现意外字符， `MÃƒÂ©lade` `Mélade` 则数据库排序规则不会设置为 [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 排序规则。 
> 使用某些 SQL 语句（如）[将数据库的排序规则更改](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation)为某种 UTF8 排序规则 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 。


## <a name="flattening-nested-arrays"></a>平展嵌套数组

Azure Cosmos DB 数据可能包含来自 [Cord19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) 数据集的作者数组等嵌套子：

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

在某些情况下，可能需要将顶部项中的属性 "联接" (元数据) 与数组的所有元素 (作者) 。 无服务器 SQL 池可用于通过对嵌套数组应用函数来平展嵌套结构 `OPENJSON` ：

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

此查询的结果可能如下所示：

| title | 作者 | first | last | 关系 |
| --- | --- | --- | --- | --- |
| 补充信息 a epidemi .。。 |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
补充信息 a epidemi .。。 | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | 4# |`{"laboratory":"","institution":"U…` | 
| 补充信息 a epidemi .。。 |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| 补充信息 a epidemi .。。 |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Marc-olivier | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> 如果你的文本（而不是）中出现意外字符， `MÃƒÂ©lade` `Mélade` 则数据库排序规则不会设置为 [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 排序规则。 
> 使用某些 SQL 语句（如）[将数据库的排序规则更改](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation)为某种 UTF8 排序规则 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 。

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>Azure Cosmos DB 到 SQL 类型的映射

首先要注意的是，虽然 Azure Cosmos DB 事务性存储区与架构无关，但分析存储架构化以优化分析查询性能。 利用 Synapse 链接的 autosync 功能，Azure Cosmos DB 可以在不存在的情况下管理分析存储中的架构表示形式，其中包括处理嵌套的数据类型。 由于无服务器 SQL 池查询分析存储区，因此必须了解如何将 Azure Cosmos DB 输入数据类型映射到 SQL 数据类型。

Azure Cosmos DB SQL (Core) API 的帐户支持 number、string、boolean、null、嵌套对象或数组的 JSON 属性类型。 如果在中使用子句，则需要选择与这些 JSON 类型匹配的 SQL 类型 `WITH` `OPENROWSET` 。 请参阅下面的 SQL 列类型，它们应该用于 Azure Cosmos DB 中的不同属性类型。

| Azure Cosmos DB 属性类型 | SQL 列类型 |
| --- | --- |
| 布尔 | bit |
| Integer | bigint |
| 小数 | FLOAT |
| 字符串 | varchar (UTF8 数据库排序规则)  |
|  (ISO 格式字符串的日期时间)  | varchar (30)  |
| Unix 时间戳 (日期时间)  | bigint |
| Null | `any SQL type` 
| 嵌套的对象或数组 | varchar (max)  (UTF8 数据库排序规则) ，序列化为 JSON 文本 |

## <a name="full-fidelity-schema"></a>完全保真架构

Azure Cosmos DB 完全保真架构记录容器中每个属性的值和其最佳匹配类型。
`OPENROWSET` 具有完全保真架构的容器上的函数同时提供每个单元中的类型值和实际值。 假设下面的查询从具有完全保真架构的容器中读取项：

```sql
SELECT *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) as rows
```

此查询的结果将返回格式化为 JSON 文本的类型和值： 

| date_rep | cases | geo_id |
| --- | --- | --- |
| {"date"： "2020-08-13"} | {"int32"： "254"} | {"string"： "RS"} |
| {"date"： "2020-08-12"} | {"int32"： "235"}| {"string"： "RS"} |
| {"date"： "2020-08-11"} | {"int32"： "316"} | {"string"： "RS"} |
| {"date"： "2020-08-10"} | {"int32"： "281"} | {"string"： "RS"} |
| {"date"： "2020-08-09"} | {"int32"： " | {"string"： "RS"} |
| {"string"： "2020/08/08"} | {"int32"： "312"} | {"string"： "RS"} |
| {"date"： "2020-08-07"} | {"float64"： "339.0"} | {"string"： "RS"} |

对于每个值，你都可以查看 Cosmos DB 容器项中标识的类型。 属性的大多数值 `date_rep` `date` 都包含值，但某些值在 Cosmos DB 中错误地存储为字符串。 完全保真架构将返回正确的类型 `date` 值和格式不正确的 `string` 值。
事例数是存储为值的信息 `int32` ，但有一个输入为十进制数的值。 此值的 `float64` 类型为。 如果某些值超过了最大值 `int32` ，则会将其存储为 `int64` 类型。 `geo_id`此示例中的所有值都存储为 `string` 类型。

> [!IMPORTANT]
> 完全保真架构同时公开了具有预期类型的值和输入了输入错误类型的值。
> 应该清除 Azure Cosmos DB 容器中具有错误类型的值，以便在完全保真分析存储中应用 corection。 

若要查询 Mongo DB API 类型 Azure Cosmos DB 帐户，可以在 [此处](../../cosmos-db/analytical-store-introduction.md#analytical-schema)了解有关分析存储中的完全保真架构表示形式的详细信息以及要使用的扩展属性名称。

查询完全保真架构时，需要显式指定 SQL 类型，并在子句中指定 Cosmos DB 属性类型 `WITH` 。 在下面的示例中，我们假定 `string` `geo_id` 属性的属性和正确类型的类型正确 `int32` `cases` ：

```sql
SELECT geo_id, cases = SUM(cases)
FROM OPENROWSET(
      'CosmosDB'
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string',
             cases INT '$.cases.int32'
    ) as rows
GROUP BY geo_id
```

与其他类型的值将不会在 `geo_id` 和 `cases` 列中返回，并且查询将 `NULL` 在这些单元格中返回值。 此查询将只引用 `cases` 表达式中具有指定类型的 (`cases.int32`) 。 如果具有其他类型 (的值 `cases.int64` ， `cases.float64` 则无法在 Cosmos DB 容器中清除表示的) ，则需要在子句中显式引用这些值 `WITH` 并将结果组合在一起。 下面的查询将聚合 `int32` 、 `int64` 和，并 `float64` 将其存储在 `cases` 列中：

```sql
SELECT geo_id, cases = SUM(cases_int) + SUM(cases_bigint) + SUM(cases_float)
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string', 
             cases_int INT '$.cases.int32',
             cases_bigint BIGINT '$.cases.int64',
             cases_float FLOAT '$.cases.float64'
    ) as rows
GROUP BY geo_id
```

在此示例中，事例数存储为 `int32` 、 `int64` 或 `float64` 值，并且必须提取所有值才能计算每个国家/地区的事例数。 

## <a name="known-issues"></a>已知问题

- **必须** 在函数 (之后指定别名 `OPENROWSET` ，例如 `OPENROWSET (...) AS function_alias`) 。 省略别名可能会导致连接问题，Synapse 无服务器 SQL 终结点可能暂时不可用。 此问题将在11月2020中解决。
- 无服务器 SQL 池为 [Azure Cosmos DB 完全保真架构](#full-fidelity-schema) 提供的查询体验是临时行为，将根据预览反馈进行更改。 不要依赖于 `OPENROWSET` 公共预览版期间函数提供的架构，因为查询体验可能与定义完善的架构匹配。 请联系 [Synapse 链接产品团队](mailto:cosmosdbsynapselink@microsoft.com) 提供反馈。

下表列出了可能的错误和故障排除操作：

| Error | 根本原因 |
| --- | --- |
| 语法错误：<br/> -"Openrowset" 附近有语法错误<br/> - `...` 不是已识别的大容量 OPENROWSET 提供程序选项。<br/> -附近有语法错误 `...` | 可能的根本原因<br/> -不使用 "CosmosDB" 作为第一个参数，<br/> -在第三个参数中使用字符串文字代替标识符，<br/> -未指定第三个参数 (容器名称)  |
| CosmosDB 连接字符串中存在错误 | -Account、Database、Key 未指定 <br/> -连接字符串中有一些无法识别的选项。<br/> -分号 `;` 置于连接字符串的末尾 |
| 解析 CosmosDB 路径失败，出现错误 "帐户名" 或 "数据库名称不正确" | 找不到指定的帐户名称、数据库名称或容器，或者尚未对指定的集合启用分析存储|
| 解析 CosmosDB 路径失败，出现错误 "机密值" 或 "机密为 null 或空" | 帐户密钥无效或缺失。 |
| `column name`类型的列 `type name` 与外部数据类型不兼容`type name` | 指定的列类型 in `WITH` 子句与 Cosmos DB 容器中的类型不匹配。 尝试更改列类型，如 " [Azure Cosmos DB 到 SQL 类型的映射](#azure-cosmos-db-to-sql-type-mappings) " 或 "使用类型" 一节中所述 `VARCHAR` 。 |
| 列包含 `NULL` 所有单元中的值。 | 子句中可能出现错误的列名或路径表达式 `WITH` 。 列类型) in 子句中的列类型之后 (或路径表达式 `WITH` 必须与 Cosmos DB 集合中的某些属性名称相匹配。 比较区分 **大小写**  (例如， `productCode` 和 `ProductCode`) 不同的属性。 |

可以在 [Azure Synapse 反馈页](https://feedback.azure.com/forums/307516-azure-synapse-analytics?category_id=387862)上报告建议和问题。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下文章：

- [将 Power BI 和无服务器 Synapse SQL 池与 Azure Synapse 链接一起使用](../../cosmos-db/synapse-link-power-bi.md)
- [如何按需在 SQL 中创建和使用视图](create-use-views.md) 
- [有关通过 Azure Cosmos DB 生成 SQL 点播视图并通过 DirectQuery 将它们连接到 Power BI 模型的教程](./tutorial-data-analyst.md)
