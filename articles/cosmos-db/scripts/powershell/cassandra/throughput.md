---
title: 用于 Azure Cosmos DB Cassandra API 资源的吞吐量 (RU/s) 操作的 PowerShell 脚本
description: 用于 Azure Cosmos DB Cassandra API 资源的吞吐量 (RU/s) 操作的 PowerShell 脚本
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 3bb69843e3035e1cf59a9e341ae39ef249b6a9fe
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282778"
---
# <a name="throughput-rus-operations-with-powershell-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>使用 PowerShell 实现的用于 Azure Cosmos DB - Cassandra API 的密钥空间或表的吞吐量 (RU/s) 操作

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>获取吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-ru-get.ps1 "Get throughput on a keyspace or table for Cassandra API")]

## <a name="update-throughput"></a>更新吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-ru-update.ps1 "Update throughput on a keyspace or table for Cassandra API")]

## <a name="migrate-throughput"></a>迁移吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a keyspace or table for Cassandra API")]

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBCassandraKeyspaceThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandrakeyspacethroughput) | 获取 Cassandra API 密钥空间的吞吐量值。 |
| [Get-AzCosmosDBCassandraTableThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbcassandratablethroughput) | 获取 Cassandra API 表的吞吐量值。 |
| [Update-AzCosmosDBCassandraKeyspaceThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbcassandrakeyspacethroughput) | 更新 Cassandra API 密钥空间的吞吐量值。 |
| [Update-AzCosmosDBCassandraTableThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbcassandratablethroughput) | 更新 Cassandra API 表的吞吐量值。 |
| [Invoke-AzCosmosDBCassandraKeyspaceThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbcassandrakeyspacethroughputmigration) | 迁移 Cassandra API 密钥空间的吞吐量。 |
| [Invoke-AzCosmosDBCassandraTableThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbcassandratablethroughputmigration) | 迁移 Cassandra API 表的吞吐量。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。
