---
title: 使用 Java v4 SDK 排查 Azure Cosmos DB 服务不可用异常
description: 了解如何诊断和修复 Java v4 SDK Azure Cosmos DB 服务不可用异常。
author: kushagrathapar
ms.service: cosmos-db
ms.date: 10/28/2020
ms.author: kuthapar
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: bba6465b8978b58fa3ef7be2a7575018828eabb2
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102827"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-java-v4-sdk-service-unavailable-exceptions"></a>诊断和解决 Azure Cosmos DB Java v4 SDK 服务不可用异常
Java v4 SDK 无法连接到 Azure Cosmos DB。

## <a name="troubleshooting-steps"></a>疑难解答步骤
下面的列表包含“服务不可用”异常的已知原因和解决方案。

### <a name="the-required-ports-are-being-blocked"></a>所需端口被阻止
验证所有[必需的端口](sql-sdk-connection-modes.md#service-port-ranges)是否已启用。

### <a name="client-side-transient-connectivity-issues"></a>客户端暂时性连接问题
当存在导致超时的暂时性连接问题时，可能会出现“服务不可用”异常。 通常情况下，与此方案相关的堆栈跟踪将包含 `ServiceUnavailableException` 错误和诊断详细信息。 例如：

```java
Exception in thread "main" ServiceUnavailableException{userAgent=azsdk-java-cosmos/4.6.0 Linux/4.15.0-1096-azure JRE/11.0.8, error=null, resourceAddress='null', requestUri='null', statusCode=503, message=Service is currently unavailable, please retry after a while. If this problem persists please contact support.: Message: "" {"diagnostics"}
```

请按照[请求超时故障排除步骤](troubleshoot-request-timeout-java-sdk-v4-sql.md#troubleshooting-steps)解决此问题。

### <a name="service-outage"></a>服务中断
检查 [Azure 状态](https://status.azure.com/status)，了解是否有正在发生的问题。


## <a name="next-steps"></a>后续步骤
* [诊断并解决](troubleshoot-java-sdk-v4-sql.md) 使用 Azure Cosmos DB JAVA v4 SDK 时遇到的问题。
* 了解 [Java V4 SDK](performance-tips-java-sdk-v4-sql.md)的性能准则。