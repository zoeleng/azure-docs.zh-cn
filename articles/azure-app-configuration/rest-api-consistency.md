---
title: Azure 应用配置 REST API-一致性
description: 使用 Azure 应用配置确保实时一致性的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 4f11e6edcd4bc128f815db7e93b00b72bf990ea8
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423979"
---
# <a name="real-time-consistency"></a>实时一致性

由于某些分布式系统的性质，请求之间的实时一致性难以隐式强制执行。 解决方法是允许以多个 **同步令牌** 的形式提供协议支持。 同步令牌是可选的。

## <a name="initial-request"></a>初始请求

为了保证不同客户端实例和请求之间的实时一致性，请使用可选的 `Sync-Token` 请求/响应标头。

语法：

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|参数|说明|
|--|--|
| `<id>` | 标记 ID (不透明)  |
| `<value>` | 标记值 (不透明) 。 允许 base64 编码的字符串 |
| `<sn>` | 版本)  (的令牌序列号。 更高表示相同令牌的较新版本。 允许更好地进行并发和客户端缓存。 由于令牌版本是包含的，客户端可以选择仅使用令牌的最后一个版本。 请求不需要。 |

## <a name="response"></a>响应

服务为 `Sync-Token` 每个响应提供标头。

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>后续请求

任何后续请求都保证与提供的进行 **实时** 一致的响应 `Sync-Token` 。

```http
Sync-Token: <id>=<value>
```

如果 `Sync-Token` 请求中省略了标头，则服务可能会在一段较短的时间内使用缓存数据进行响应， (多达几秒钟) ，然后在内部进行侦听。 如果在读取之前立即发生更改，则此行为可能导致不一致的读取。

## <a name="multiple-sync-tokens"></a>多个同步令牌

服务器可能会响应单个请求的多个同步令牌。 若要保持下一次请求的 **实时** 一致性，客户端必须通过收到的所有同步令牌进行响应。 根据 RFC，多个标头值必须以逗号分隔。

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
