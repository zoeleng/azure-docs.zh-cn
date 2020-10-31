---
title: 必应实体搜索 API 终结点
titleSuffix: Azure Cognitive Services
description: 必应实体搜索 API 具有一个终结点，该终结点根据查询从 Web 返回实体。 这些搜索结果以 JSON 格式返回。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 5d6b0a6be0d99815c4d14bb04e56241d011812b5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93084946"
---
# <a name="bing-entity-search-api-endpoint"></a>必应实体搜索 API 终结点

> [!WARNING]
> 必应搜索 API 将从认知服务迁移到必应搜索服务。 从 **2020 年10月 30** 日起，需要按照 [此处](https://aka.ms/cogsvcs/bingmove)所述的过程设置必应搜索的任何新实例。
> 在接下来的三年中，将支持使用认知服务进行预配的必应搜索 API，或者在企业协议结束后（以先发生者为准）。
> 有关迁移说明，请参阅 [必应搜索服务](https://aka.ms/cogsvcs/bingmigration)。


必应实体搜索 API 具有一个终结点，该终结点根据查询从 Web 返回实体。 这些搜索结果以 JSON 格式返回。

## <a name="get-entity-results-from-the-endpoint"></a>从终结点获取实体结果

若要使用必应 API  获取实体结果，请向以下终结点发送 `GET` 请求。 使用[标头](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers)和[查询参数](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters)来自定义搜索请求。 可以使用 `?q=` 参数发送搜索请求。

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [什么是必应实体搜索 API？](overview.md)

## <a name="see-also"></a>另请参阅 

有关标头、参数、市场代码、响应对象、错误等的详细信息，请参阅[必应实体搜索 API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) 参考文章。
