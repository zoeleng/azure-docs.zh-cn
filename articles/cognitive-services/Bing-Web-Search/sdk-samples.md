---
title: 必应 Web 搜索 SDK 示例
titleSuffix: Azure Cognitive Services
description: 使用必应 Web 搜索 SDK 将搜索功能添加到 Python、Node.js、C# 或 Java 应用程序中。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: sample
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: b31335df7c1dc48f82699aa3676561721474071d
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93076208"
---
# <a name="bing-web-search-sdk-samples"></a>必应 Web 搜索 SDK 示例

> [!WARNING]
> 必应搜索 API 将从认知服务迁移到必应搜索服务。 从 2020 年 10 月 30 日开始，必应搜索的任何新实例都需按照[此处](https://aka.ms/cogsvcs/bingmove)所述的过程进行预配。
> 使用认知服务进行预配的必应搜索 API 将在未来三年或在企业协议结束前（以先发生者为准）得到支持。
> 有关迁移说明，请参阅[必应搜索服务](https://aka.ms/cogsvcs/bingmigration)。

必应 Web 搜索 SDK 可用于 Python、Node.js、C# 和 Java。 GitHub 上提供了代码示例、先决条件和生成说明。 介绍以下方案：

* 查询单个字词并输出网页、图像、新闻文章和视频的第一个结果的名称和 URL。
* 查询短语、验证结果数，并输出第一个结果的名称和 URL。
* 在响应筛选器设置为 `news` 的情况下查询搜索词，并输出新闻结果的详细信息。
* 使用 `answerCount` 和 `promote` 参数查询搜索词，然后输出结果的详细信息。

## <a name="sdks-and-libraries"></a>SDK 和库

使用以下链接访问适用于首选语言的 SDK。

* 开始使用 [Python 示例](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)
  * 有关定义和依赖项，另请参阅 [Python 库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-websearch)。
* 开始使用 [ 示例](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
  * 另请参阅 [Node.js Web 搜索](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesWebSearch)。
* 开始使用 [.NET 示例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
  * [NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0)
  * 有关定义和依赖项，另请参阅 [.NET 库](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingWebSearch)。
* 开始使用 [Java 示例](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)
  * 有关定义和依赖项，另请参阅 [Java 库](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingWebSearch)。
