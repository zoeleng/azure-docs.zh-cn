---
title: 适用于 Azure Cosmos DB Core (SQL) API 的 Azure CLI 示例
description: 适用于 Azure Cosmos DB Core (SQL) API 的 Azure CLI 示例
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/13/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7c7baf4de56edd25537f93095bfa6ae0402214e2
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93073167"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-core-sql-api"></a>适用于 Azure Cosmos DB Core (SQL) API 的 Azure CLI 示例
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

下表包括用于 Azure Cosmos DB 的示例 Azure CLI 脚本的链接。 使用右侧的链接可导航到 API 特定示例。 常见示例在所有 API 间是相同的。 [Azure CLI 参考](/cli/azure/cosmosdb)中收录了所有 Azure Cosmos DB CLI 命令的参考页。 还可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到 Azure Cosmos DB CLI 脚本示例。

这些示例要求使用 Azure CLI 2.12.1 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](/cli/azure/install-azure-cli)

有关其他 API 的 Azure CLI 示例，请参阅[适用于 Cassandra 的 CLI 示例](cli-samples-cassandra.md)、[适用于 MongoDB API 的 CLI 示例](cli-samples-mongodb.md)、[适用于 Gremlin 的 CLI 示例](cli-samples-gremlin.md)、[适用于 Table 的 CLI 示例](cli-samples-table.md)

## <a name="common-samples"></a>常见示例

这些示例适用于所有 Azure Cosmos DB API

|任务 | 说明 |
|---|---|
| [添加或故障转移区域](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | 添加区域、更改故障转移优先级、触发手动故障转移。|
| [帐户密钥和连接字符串](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | 列出帐户密钥、只读密钥，重新生成密钥并列出连接字符串。|
| [使用 IP 防火墙保护](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| 创建配置了 IP 防火墙的 Cosmos 帐户。|
| [使用服务终结点保护新帐户](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| 使用服务终结点创建 Cosmos 帐户并确保其安全。|
| [使用服务终结点保护现有帐户](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| 最终配置子网后，更新 Cosmos 帐户以使用服务终结点进行保护。|
|||

## <a name="core-sql-api-samples"></a>Core (SQL) API 示例

|任务 | 说明 |
|---|---|
| [创建 Azure Cosmos 帐户、数据库和容器](scripts/cli/sql/create.md?toc=%2fcli%2fazure%2ftoc.json)| 为 Core (SQL) API 创建 Azure Cosmos DB 帐户、数据库和容器。 |
| [创建 Azure Cosmos 帐户、数据库和容器（具有自动缩放功能）](scripts/cli/sql/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 为 Core (SQL) API 创建 Azure Cosmos DB 帐户、数据库和容器（具有自动缩放功能）。 |
| [吞吐量操作](scripts/cli/sql/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 在数据库和容器上的自动缩放和标准吞吐量之间读取、更新和迁移。|
| [锁定资源以防止将其删除](scripts/cli/sql/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用资源锁防止删除资源。|
|||
