---
author: baanders
description: Azure 数字孪生的包含文件-管理实例的方法
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 47de00780fb259e7476c16dea9b7c63bc512a211
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533869"
---
本文重点介绍如何使用 [Azure 数字孪生 .net (c # ) **SDK**](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true)完成不同的管理操作。 还可以使用 " [*操作方法：使用 Azure 数字孪生 api 和 sdk*](../articles/digital-twins/how-to-use-apis-sdks.md)" 中所述的其他语言 sdk 来创建这些相同的管理调用。

> [!TIP] 
> 请记住，所有 SDK 方法均为同步和异步版本。 对于分页调用，异步方法会在 `AsyncPageable<T>` 同步版本返回时返回 `Pageable<T>` 。

另一种管理选项是直接调用此主题区域的 Azure 数字孪生 [**REST api**](/rest/api/azure-digitaltwins/) 。

最后，你可以使用 Azure 数字孪生 **CLI** 完成相同的管理操作。 若要了解有关使用 CLI 的详细信息，请参阅 [*操作方法：使用 Azure 数字孪生 CLI*](../articles/digital-twins/how-to-use-cli.md)。