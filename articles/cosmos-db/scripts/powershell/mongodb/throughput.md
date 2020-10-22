---
title: 用于 MongoDB 的 Azure Cosmos DB API 的吞吐量 (RU/s) 操作的 PowerShell 脚本
description: 用于 MongoDB 的 Azure Cosmos DB API 的吞吐量 (RU/s) 操作的 PowerShell 脚本
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 0b3c755415185324d427bcd3ae222774bca31284
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282709"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-collection-for-azure-cosmos-db-api-for-mongodb"></a>用于 MongoDB 的 Azure Cosmos DB API 的数据库或集合的通过 PowerShell 实现的吞吐量 (RU/s) 操作

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>获取吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-get.ps1 "Get throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="update-throughput"></a>更新吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-update.ps1 "Update throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="migrate-throughput"></a>迁移吞吐量

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

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
| [Get-AzCosmosDBMongoDBDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabasethroughput) | 获取指定的 MongoDB 数据库的 Azure Cosmos DB API 的吞吐量值。 |
| [Get-AzCosmosDBMongoDBCollectionThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollectionthroughput) | 获取指定的 MongoDB 集合的 Azure Cosmos DB API 的吞吐量值。 |
| [Update-AzCosmosDBMongoDBDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbdatabasethroughput) | 更新 MongoDB 数据库的 Azure Cosmos DB API 的吞吐量值。 |
| [Update-AzCosmosDBMongoDBCollectionThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbcollectionthroughput) | 更新 MongoDB 集合的 Azure Cosmos DB API 的吞吐量值。 |
| [Invoke-AzCosmosDBMongoDBDatabaseThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbdatabasethroughputmigration) | 迁移 MongoDB 集合的 Azure Cosmos DB API 的吞吐量。 |
| [Invoke-AzCosmosDBMongoDBCollectionThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbcollectionthroughputmigration) | 迁移 MongoDB 集合的 Azure Cosmos DB API 的吞吐量。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。
