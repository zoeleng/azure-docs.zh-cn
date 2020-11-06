---
title: Azure 应用配置 REST API-阻止
description: 使用 Azure 应用配置时了解限制的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 26464138b7739bb476d9d0dd1078194473ecd23e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423963"
---
# <a name="throttling"></a>遏制

配置存储对其可能服务的请求有限制。 超过配置存储的已分配配额的任何请求都将收到 HTTP 429 (太多请求) 响应。

限制分为不同的配额策略：

- **请求总数** -请求总数
- **总带宽** -出站数据（字节）
- **存储** -用户数据的存储总大小（字节）

## <a name="handling-throttled-responses"></a>处理受限响应

达到给定配额的速率限制时，服务器将使用 _429_ 状态代码响应该类型的更多请求。 _429_ 响应将包含一个 _后重试_ 的标头，该标头为客户端提供了建议的等待时间（以毫秒为单位） () 以允许请求配额进行补充。

```http
HTTP/1.1 429 (Too Many Requests)
retry-after-ms: 10
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/too-many-requests",
  "title": "Resource utilization has surpassed the assigned quota",
  "policy": "Total Requests",
  "status": 429
}
```

在上面的示例中，客户端已超过其允许的配额，并建议在尝试进一步的请求之前，等待10毫秒。 客户端还应考虑渐进式回退。

## <a name="other-retry"></a>其他重试

该服务可以确定需要客户端重试 (例如：503服务不可用) 以外的其他情况。 在所有这类情况下， `retry-after-ms` 都将提供响应标头。 为了提高可靠性，建议客户端遵循建议的间隔，然后执行重试。

```http
HTTP/1.1 503 Service Unavailable
retry-after-ms: 787
```
