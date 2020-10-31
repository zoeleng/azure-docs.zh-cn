---
title: Azure Cosmos DB SQL SDK 连接模式
description: 了解 Azure Cosmos DB SQL Sdk 上可用的不同连接模式。
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/14/2020
ms.author: maquaran
ms.custom: devx-track-dotnet
ms.openlocfilehash: 95396f28fd835091258bccbfdb0a0c0eafebea91
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93093548"
---
# <a name="azure-cosmos-db-sql-sdk-connectivity-modes"></a>Azure Cosmos DB SQL SDK 连接模式
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

客户端连接到 Azure Cosmos DB 的方式对性能有重大影响，尤其是在观察到的客户端延迟方面。 Azure Cosmos DB 通过 HTTPS （称为网关模式）提供简单的开放 RESTful 编程模型。 此外，它还提供了高效的 TCP 协议，该协议在其通信模型中也是 RESTful，并使用 TLS 进行初始身份验证和加密通信（称为直接模式）。

## <a name="available-connectivity-modes"></a>可用连接模式

两种可用的连接模式为：

  * 网关模式
      
    所有 SDK 平台都支持网关模式。 如果应用程序在有严格防火墙限制的企业网络中运行，则网关模式是最佳选择，因为它使用标准 HTTPS 端口与单个 DNS 终结点。 但是，对于性能的影响是：每次在 Azure Cosmos DB 中读取或写入数据时，网关模式都涉及到额外的网络跃点。 在套接字连接数量有限的环境中运行应用程序时，我们也建议使用网关连接模式。

    在 Azure Functions 中使用 SDK 时，尤其是在[消耗计划](../azure-functions/functions-scale.md#consumption-plan)中使用时，请注意当前的[连接限制](../azure-functions/manage-connections.md)。

  * 直接模式

    直接模式通过 TCP 协议支持连接性，并提供更好的性能，因为网络跃点更少。 应用程序直接连接到后端副本。 直接模式目前仅在 .NET 和 Java SDK 平台上受支持。
     
:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Azure Cosmos DB 连接模式" border="false":::

这些连接模式实质上是指数据平面请求的路由-文档读取和写入-从客户端计算机到 Azure Cosmos DB 后端的分区。 直接模式是最佳性能的首选选项，它允许客户端直接打开 TCP 连接 Azure Cosmos DB 后端和发送请求 *直接* ly 与无中介。 与此相反，在网关模式下，客户端发出的请求会路由到 Azure Cosmos DB 前端中所谓的 "网关" 服务器，这反过来会将请求发送到 Azure Cosmos DB 后端中的相应分区 (s) 。

## <a name="service-port-ranges"></a>服务端口范围

使用直接模式时，除了网关端口外，还需确保端口范围为10000到20000，因为 Azure Cosmos DB 使用动态 TCP 端口。 当对 [专用终结点](./how-to-configure-private-endpoints.md)使用直接模式时，TCP 端口的完整范围应为0到65535。 如果这些端口未打开且您尝试使用 TCP 协议，则可能会收到503服务不可用错误。

下表显示了可用于各种 Api 的连接模式和用于每个 API 的服务端口的摘要：

|连接模式  |支持的协议  |支持的 SDK  |API/服务端口  |
|---------|---------|---------|---------|
|网关  |   HTTPS    |  所有 SDK    |   SQL (443)、MongoDB（10250、10255、10256）、表 (443)、Cassandra (10350)、Graph (443) <br> 端口 10250 映射到没有异地复制功能的默认 Azure Cosmos DB API for MongoDB 实例。 而端口 10255 和 10256 映射到具有异地复制功能的实例。   |
|直接    |     TCP    |  .NET SDK    | 使用公共/服务终结点时：端口介于 10000 到 20000 之间<br>使用专用终结点时：端口介于 0 到 65535 之间 |

## <a name="next-steps"></a>后续步骤

对于特定的 SDK 平台性能优化：

* [.NET V2 SDK 性能提示](performance-tips.md)

* [.NET V3 SDK 性能提示](performance-tips-dotnet-sdk-v3-sql.md)
 
* [Java V4 SDK 性能提示](performance-tips-java-sdk-v4-sql.md)