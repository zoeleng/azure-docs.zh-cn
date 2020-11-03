---
title: Web 搜索终结点
titleSuffix: Azure Cognitive Services
description: 若要获取 web 搜索结果，请将 `GET` 请求发送到以下终结点。 标头和 URL 参数定义了更多规范。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: aahi
ms.openlocfilehash: 25ecd1a753cb9a401408f7ed6605d53e5310df2b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93075664"
---
# <a name="web-search-endpoint"></a>Web 搜索终结点

> [!WARNING]
> 必应搜索 API 将从认知服务迁移到必应搜索服务。 从 **2020 年10月 30** 日起，需要按照 [此处](https://aka.ms/cogsvcs/bingmove)所述的过程设置必应搜索的任何新实例。
> 在接下来的三年中，将支持使用认知服务进行预配的必应搜索 API，或者在企业协议结束后（以先发生者为准）。
> 有关迁移说明，请参阅 [必应搜索服务](https://aka.ms/cogsvcs/bingmigration)。

“Web 搜索 API”返回网页、新闻、图像、视频和[实体](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web)。 实体包含有关人员、地点或主题的摘要信息。

## <a name="endpoint"></a>端点

若要使用必应 API 获取 Web 搜索结果，请向以下终结点发送 `GET` 请求。 标头和 URL 参数定义了更多规范。

终结点：返回与 `?q=""` 定义的用户搜索查询相关的 Web 结果。

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

终结点：有关标头、参数、市场代码、响应对象、错误等内容的详细信息，请参阅[必应 Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) 参考。

## <a name="response-json"></a>响应 JSON

对 Web 搜索请求的响应包括作为 JSON 对象的所有结果。 分析结果需要处理每种类型元素的过程。 有关示例，请参阅[教程](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app)和[源代码](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search)。

## <a name="next-steps"></a>后续步骤

必应 API 支持根据其类型返回结果的搜索操作。 所有搜索终结点均将结果作为 JSON 响应对象返回。  所有终结点均支持按经度、纬度和搜索半径返回特定语言和位置的查询。

若要完整了解每个终结点支持的参数，请参阅每种类型对应的参考页面。
有关使用 Web 搜索 API 的基本请求的示例，请参阅[搜索 Web 快速入门](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web)。
