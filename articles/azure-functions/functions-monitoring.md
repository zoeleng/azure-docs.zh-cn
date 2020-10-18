---
title: 监视 Azure Functions
description: 了解如何将 Azure Application Insights 和 Azure Functions 结合使用来监视函数执行。
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.topic: conceptual
ms.date: 10/14/2020
ms.custom: devx-track-csharp, fasttrack-edit
ms.openlocfilehash: bb6f2769ef675ac18fe8d2b58e17f9c10f457a6f
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164893"
---
# <a name="monitor-azure-functions"></a>监视 Azure Functions

[Azure Functions](functions-overview.md) 提供与 [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) 的内置集成来监视函数。 本文概述了 Azure 提供的监视功能 Azure Functions。

Application Insights 可收集日志、性能和错误数据。 通过自动检测性能异常并提供强大的分析工具，你可以更轻松地诊断问题并更好地了解函数的使用方式。 这些工具旨在帮助您不断提高函数的性能和可用性。 你甚至可以在本地函数应用项目开发过程中使用 Application Insights。 有关详细信息，请参阅[什么是 Application Insights？](../azure-monitor/app/app-insights-overview.md)。

当 Azure Functions 中内置 Application Insights 检测时，需要使用有效的检测密钥将函数应用连接到 Application Insights 资源。 在 Azure 中创建 function app 资源时，会将检测密钥添加到应用程序设置中。 如果函数应用还没有此密钥，则可以进行[手动设置](configure-monitoring.md#enable-application-insights-integration)。  

## <a name="application-insights-pricing-and-limits"></a>Application Insights 定价和限制

可以试用 Application Insights 与 Azure Functions 免费集成，其中每日限制为免费处理的数据量。

如果在开发过程中启用 Application Insights，则可能会在测试过程中达到此限制。 当你达到每日限制时，Azure 会提供门户和电子邮件通知。 如果未收到这些警报并达到限制，则新日志不会显示在 Application Insights 查询中。 请注意限制，以免在故障排除上花费不必要的时间。 有关详细信息，请参阅[在 Application Insights 中管理定价和数据量](../azure-monitor/app/pricing.md)。

> [!IMPORTANT]
> Application Insights 具有[采样](../azure-monitor/app/sampling.md)功能，可以防止在峰值负载时为已完成的执行生成过多的遥测数据。 采样功能默认处于启用状态。 如果你看起来缺少数据，则可能需要调整采样设置来适应你的具体监视方案。 若要了解详细信息，请参阅[配置采样](configure-monitoring.md#configure-sampling)。

[适用于 Azure Functions 的 Application Insights 支持的功能](../azure-monitor/app/azure-functions-supported-features.md)中详细介绍了函数应用可用的 Application Insights 功能的完整列表。

## <a name="application-insights-integration"></a>Application Insights 集成

通常，在创建函数应用时创建 Application Insights 实例。 在这种情况下，集成所需的检测密钥已设置为名为 *APPINSIGHTS_INSTRUMENTATIONKEY*的应用程序设置。 如果由于某种原因，函数应用未设置检测密钥，则需要 [启用 Application Insights 集成](configure-monitoring.md#enable-application-insights-integration)。  

## <a name="collecting-telemetry-data"></a>收集遥测数据

启用 Application Insights 集成后，遥测数据将发送到连接的 Application Insights 实例。 此数据包括函数主机生成的日志、从函数代码写入的跟踪和性能数据。 

>[!NOTE]
>除了函数和函数宿主的数据以外，还可以从 [函数缩放控制器](#scale-controller-logs)收集数据。   

### <a name="log-levels-and-categories"></a>日志级别和类别

当你从应用程序代码中编写跟踪时，应为跟踪分配一个日志级别。 日志级别为你提供了一种方法，用于限制从跟踪收集的数据量。  

[!INCLUDE [functions-log-levels](../../includes/functions-log-levels.md)]

若要了解有关日志级别的详细信息，请参阅 [配置日志级别](configure-monitoring.md#configure-log-levels)。

通过将记录的项分配到类别，可以更好地控制从函数应用中的特定源生成的遥测。 类别使你可以更轻松地对收集的数据运行分析。 从函数代码写入的跟踪根据函数名称分配给各个类别。 若要了解有关类别的详细信息，请参阅 [配置类别](configure-monitoring.md#configure-categories)。

### <a name="custom-telemetry-data"></a>自定义遥测数据

在 [c #](functions-dotnet-class-library.md#log-custom-telemetry-in-c-functions) 和 [JavaScript](functions-reference-node.md#log-custom-telemetry)中，可以使用 Application Insights SDK 来编写自定义遥测数据。

### <a name="dependencies"></a>依赖项

从版本1.x 开始，运行时将自动收集有关使用特定客户端 Sdk 的绑定的依赖项的数据。 Application Insights 收集以下依赖项的数据：

+ Azure Cosmos DB 
+ Azure 事件中心
+ Azure 服务总线
+ Azure 存储服务 (Blob、队列和表) 

还会捕获使用的 HTTP 请求和数据库调用 `SqlClient` 。 有关 Application Insights 支持的依赖项的完整列表，请参阅 [自动跟踪的依赖项](../azure-monitor/app/asp-net-dependencies.md#automatically-tracked-dependencies)。

Application Insights 生成收集的依赖项数据的 _应用程序映射_ 。 下面是包含队列存储输出绑定的 HTTP 触发器函数的应用程序映射示例。  

![具有依赖关系的应用程序映射](./media/functions-monitoring/app-map.png)

依赖项在级别上写入 `Information` 。 如果你在 `Warning` 或更高版本中进行筛选，你将看不到依赖关系数据。 此外，自动收集依赖项在非用户范围内进行。 若要捕获依赖关系数据，请确保将级别设置为至少在 `Information` 用户范围外 (`Function.<YOUR_FUNCTION_NAME>.User` 主机) 。

除了自动依赖项数据收集外，还可以使用特定于语言的 Application Insights Sdk 将自定义的依赖关系信息写入日志。 有关如何编写自定义依赖项的示例，请参阅以下特定于语言的示例之一：

+ [在 C# 函数中记录自定义遥测](functions-dotnet-class-library.md#log-custom-telemetry-in-c-functions)
+ [在 JavaScript 函数中记录自定义遥测](functions-reference-node.md#log-custom-telemetry) 

## <a name="writing-to-logs"></a>写入日志 

写入日志的方式和使用的 Api 取决于 function app 项目的语言。   
请参阅适用于你的语言的开发人员指南，详细了解如何从函数编写日志。

+ [C # ( .NET 类库) ](functions-dotnet-class-library.md#logging)
+ [Java](functions-reference-java.md#logger)
+ [JavaScript](functions-reference-node.md#write-trace-output-to-logs) 
+ [PowerShell](functions-reference-powershell.md#logging)
+ [Python](functions-reference-python.md#logging)

## <a name="streaming-logs"></a>流式处理日志

开发应用程序时，通常需要了解在 Azure 中运行时近实时地写入日志的内容。

可以通过两种方式查看由函数执行生成的日志数据的流。

* **内置日志流式处理**：借助应用服务平台即可查看应用程序日志文件流。 此流等效于在 [本地开发](functions-develop-local.md) 期间调试函数时以及在门户中使用 " **测试** " 选项卡时所显示的输出。 此时将显示所有基于日志的信息。 有关详细信息，请参阅[流式处理日志](../app-service/troubleshoot-diagnostic-logs.md#stream-logs)。 这种流式处理方法仅支持单个实例，不能用于在消耗计划中的 Linux 上运行的应用。

* **实时指标流**：当函数应用 [连接到 Application Insights](configure-monitoring.md#enable-application-insights-integration)时，你可以使用 [实时指标流](../azure-monitor/app/live-stream.md)以近乎实时的 Azure 门户查看日志数据和其他指标。 当监视在多个实例或消耗计划中的 Linux 上运行的函数时，请使用此方法。 此方法使用[抽样数据](configure-monitoring.md#configure-sampling)。

可以在门户和大多数本地开发环境中查看日志流。 若要了解如何启用日志流，请参阅 [在 Azure Functions 中启用流式处理执行日志](streaming-logs.md)。

## <a name="diagnostic-logs"></a>诊断日志

_此功能为预览版。_ 

Application Insights 允许你将遥测数据导出到长期存储或其他 analysis services。  

由于函数还与 Azure Monitor 集成，因此还可以使用诊断设置将遥测数据发送到各种目标，包括 Azure Monitor 日志。 若要了解更多信息，请参阅[使用 Azure Monitor 日志监视 Azure Functions](functions-monitor-log-analytics.md)。

## <a name="scale-controller-logs"></a>缩放控制器日志

_此功能为预览版。_ 

[Azure Functions 缩放控制器](./functions-scale.md#runtime-scaling)监视运行应用的 Azure Functions 主机的实例。 此控制器根据当前性能决定何时添加或删除实例。 可以将缩放控制器发出日志到 Application Insights，以便更好地了解缩放控制器为 function app 做出的决策。 你还可以将生成的日志存储在 Blob 存储中以供其他服务分析。 

若要启用此功能，请将名为的应用程序设置添加 `SCALE_CONTROLLER_LOGGING_ENABLED` 到 function app 设置。 若要了解如何操作，请参阅 [配置缩放控制器日志](configure-monitoring.md#configure-scale-controller-logs)。

## <a name="report-issues"></a>报告问题

若要报告 Functions 中的 Application Insights 集成问题，或提出建议或请求，请[在 GitHub 中创建问题](https://github.com/Azure/Azure-Functions/issues/new)。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下资源：

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core 日志记录](/aspnet/core/fundamentals/logging/)
