---
title: Azure Monitor 日志
description: 描述用于监视监视数据的高级分析 Azure Monitor 日志。
documentationcenter: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/22/2020
ms.author: bwren
ms.openlocfilehash: 35ed275419cc4319e7edfed608340853c986a927
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515619"
---
# <a name="azure-monitor-logs-overview"></a>Azure Monitor 日志概述
Azure Monitor 日志是 Azure Monitor 的一项功能，用于从 [受监视的资源](../monitor-reference.md)收集和组织日志和性能数据。 来自不同源的数据，例如来自 Azure 服务的 [平台日志](platform-logs-overview.md) 、来自 [虚拟机代理](agents-overview.md)的日志和性能数据，以及 [应用程序](../app/app-insights-overview.md) 的使用情况和性能数据，可以将其合并到单个工作区中，以便可以使用能够快速分析数百万条记录的复杂查询语言一起分析这些数据。 您可以执行一个简单的查询，该查询只检索一组特定的记录或执行复杂的数据分析，以确定监视数据中的关键模式。 使用 Log Analytics 以交互方式使用日志查询及其结果，在警报规则中使用这些查询来主动通知问题，或在工作簿或仪表板中将结果可视化。

> [!NOTE]
> Azure Monitor 日志是支持 Azure Monitor 的数据平台的一部分。 另一种是 [Azure Monitor 度量值](data-platform-metrics.md) ，用于将数值数据存储在时序数据库中。 这会使此数据比 Azure Monitor 日志中的数据更轻型，并支持近实时方案，使其特别适用于警报和快速检测问题。 不过，指标只能将数值数据存储在特定的结构中，而日志可以存储各种不同的数据类型，每个数据类型都有自己的结构。 你还可以使用不能用于分析指标数据的日志查询对日志数据执行复杂的分析。


## <a name="what-can-you-do-with-azure-monitor-logs"></a>可以对 Azure Monitor 日志执行哪些操作？
下表描述了可以在 Azure Monitor 中使用日志的一些不同方法：

|  |  |
|:---|:---|
| **分析** | 使用 Azure 门户中 [Log Analytics](../log-query/get-started-portal.md) 编写 [日志查询](../log-query/log-query-overview.md) ，并使用强大的分析引擎以交互方式分析日志数据 |
| **Alert** | 配置[日志警报规则](alerts-log.md)，以便在查询结果与特定的结果匹配时发送通知或执行[自动化操作](action-groups.md)。 |
| **可视化** | 将以表格或图表形式呈现的查询结果固定到 [Azure 仪表板](../../azure-portal/azure-portal-dashboards.md)。<br>创建一个[工作簿](../app/usage-workbooks.md)以与交互式报表中的多组数据合并。 <br>将查询结果导出到 [Power BI](powerbi.md)，以使用不同的可视化效果并与 Azure 外部的用户共享。<br>将查询结果导出到 [Grafana](grafana-plugin.md) 以利用其仪表板并与其他数据源合并。|
| **Insights** | 提供针对特定应用程序和服务的自定义监视体验的支持 [见解](../monitor-reference.md#insights-and-core-solutions) 。  |
| **检索** | 使用 [Azure CLI](/cli/azure/ext/log-analytics/monitor/log-analytics) 从命令行访问日志查询结果。<br>使用 [PowerShell cmdlet](https://docs.microsoft.com/powershell/module/az.operationalinsights) 从命令行访问日志查询结果。<br>使用 [REST API](https://dev.loganalytics.io/) 从自定义应用程序访问日志查询结果。 |
| **导出** | 配置将 [日志数据自动导出](logs-data-export.md) 到 azure 存储帐户或 Azure 事件中心。<br>构建一个工作流来检索日志数据，并使用[逻辑应用](logicapp-flow-connector.md)将其复制到外部位置。 |

![日志概述](media/data-platform-logs/logs-overview.png)


## <a name="data-collection"></a>数据收集
创建 Log Analytics 工作区后，您必须配置不同的源以发送其数据。 不自动收集任何数据。 此配置将因数据源而异。 例如， [创建诊断设置](diagnostic-settings.md) 以将资源日志从 Azure 资源发送到工作区。 [启用用于 VM 的 Azure Monitor](../insights/vminsights-enable-overview.md) 以从虚拟机收集数据。 配置 [工作区上的数据源](data-sources.md) 以收集其他事件和性能数据。

- 若要将数据发送到 Azure Monitor 日志，请参阅 ["Azure Monitor" 监视的内容](../monitor-reference.md) 。


## <a name="log-analytics-workspaces"></a>Log Analytics 工作区
Azure Monitor 日志收集的数据存储在一个 [Log Analytics 工作区](./design-logs-deployment.md)中。 工作区定义数据的地理位置、定义可访问数据的用户的访问权限，以及配置设置，如定价层和数据保留。  

必须创建至少一个工作区才能使用 Azure Monitor 日志。 单个工作区可能足以满足所有监视数据的需求，或者根据您的需求，可以选择创建多个工作区。 例如，你可能有一个用于生产数据的工作区，另一个用于测试。 

- 请参阅 [在 Azure 门户中创建 Log Analytics 工作区](../learn/quick-create-workspace.md) 以创建新的工作区。
- 有关创建多个工作区的注意事项，请参阅 [设计 Azure Monitor 日志部署](design-logs-deployment.md) 。

## <a name="data-structure"></a>数据结构
日志查询从 Log Analytics 工作区检索数据。 每个工作区都包含多个表，这些表组织到具有多行数据的单独列中。 每个表由数据源提供的数据行共享的一组唯一列定义。 

[![Azure Monitor 日志结构](media/data-platform-logs/logs-structure.png)](media/data-platform-logs/logs-structure.png#lightbox)


Application Insights 中的日志数据也存储在 Azure Monitor 日志中，但根据应用程序的配置方式，存储的数据将有所不同。 对于基于工作区的应用程序，数据存储在一组标准表中的 Log Analytics 工作区中，用于保存应用程序请求、异常和页面视图等数据。 多个应用程序可以使用同一个工作区。 对于经典应用程序，数据不会存储在 Log Analytics 工作区中。 它使用相同的查询语言，并且你使用 Azure 门户中的相同 Log Analytics 工具创建并运行查询。 不过，经典应用程序的数据彼此分开存储。 尽管表名称和列名称不同，但其常规结构与基于工作区的应用程序相同。 有关基于工作区的应用程序和经典应用程序的架构的详细比较，请参阅 [基于工作区的资源更改](../app/apm-tables.md) 。


> [!NOTE]
> 我们仍然针对 Application Insights 体验中的 Application Insights 经典资源查询、工作簿和基于日志的警报提供完全的后向兼容性。 若要根据[新的基于工作区的表结构/架构](../app/apm-tables.md)进行查询/查看，必须先导航到 Log Analytics 工作区。 在预览版期间，从 Application Insights 窗格中选择“日志”即可访问经典 Application Insights 查询体验。 有关更多详细信息，请参阅 [查询范围](../log-query/scope.md) 。


[![Azure Monitor 的日志结构 Application Insights](media/data-platform-logs/logs-structure-ai.png)](media/data-platform-logs/logs-structure-ai.png#lightbox)


## <a name="log-queries"></a>日志查询
使用作为处理数据和返回结果的只读请求的日志查询从 Log Analytics 工作区中检索数据。 日志查询以 [Kusto 查询语言编写 (KQL) ](/azure/data-explorer/kusto/query/)，这是 Azure 数据资源管理器所使用的相同查询语言。 您可以在 Log Analytics 中编写日志查询以交互方式分析它们的结果，在警报规则中使用这些查询来主动通知问题，或在工作簿或仪表板中包含它们的结果。 Insights 包括预生成的查询，以支持其视图和工作簿。

- 有关使用日志查询的位置的列表，请参阅 [Azure Monitor 日志查询](log-query/../../log-query/log-query-overview.md) ，并参考教程和其他文档以帮助你入门。

![Log Analytics](media/data-platform-logs/log-analytics.png)

## <a name="log-analytics"></a>Log Analytics
使用 Log Analytics （Azure 门户中的工具）可以编辑和运行日志查询并以交互方式分析它们的结果。 然后，可以使用所创建的查询来支持 Azure Monitor 如日志查询警报和工作簿中的其他功能。 从 "Azure Monitor" 菜单中的 " **日志** " 选项或从 Azure 门户中其他大多数服务访问 Log Analytics。

- 有关 Log Analytics 的说明，请参阅 [Azure Monitor 概述 Log Analytics](/log-query/log-analytics-overview.md) 。 
- 请参阅 [Log Analytics 教程](/log-query/log-analytics-tutorial.md) ，逐步说明如何使用 Log Analytics 功能创建简单的日志查询并分析其结果。



## <a name="relationship-to-azure-data-explorer"></a>与 Azure 数据资源管理器的关系
Azure Monitor 日志基于 Azure 数据资源管理器。 Log Analytics 工作区大致等同于 Azure 数据资源管理器中的数据库，表的结构相同，并使用相同的 Kusto 查询语言 (KQL) 。 使用 Log Analytics 在 Azure 门户中处理 Azure Monitor 查询的体验与使用 Azure 数据资源管理器 Web UI 的经验类似。 甚至可以 [在 Azure 数据资源管理器查询中包含 Log Analytics 工作区中的数据](/azure/data-explorer/query-monitor-data)。 


## <a name="next-steps"></a>后续步骤

- 了解用于从 Log Analytics 工作区检索和分析数据的 [日志查询](../log-query/log-query-overview.md) 。
- 了解 [Azure Monitor 中的指标](data-platform-metrics.md)。
- 了解适用于 Azure 中不同资源的[监视数据](data-sources.md)。
