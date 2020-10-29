---
title: 监视 Azure IoT 中心
description: 从此处开始了解如何监视 Azure IoT 中心
author: robinsh
ms.author: robinsh
ms.topic: conceptual
ms.service: iot-hub
ms.date: 10/22/2020
ms.openlocfilehash: a8f9c46487422deb4513768dff04f559af952f7b
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92926251"
---
# <a name="monitoring-azure-iot-hub"></a>监视 Azure IoT 中心

当你的关键应用程序和业务流程依赖于 Azure 资源时，你需要监视这些资源的可用性、性能和操作。 本文介绍了 Azure IoT 中心生成的监视数据，以及如何使用 Azure Monitor 的功能对此数据进行分析和发出警报。

## <a name="monitor-overview"></a>Monitor 概述

每个 IoT 中心的 Azure 门户中的 " **概述** " 页包括提供某些使用情况指标的图表，如使用的消息数以及连接到 IoT 中心的设备数。

:::image type="content" source="media/monitor-iot-hub/overview-portal.png" alt-text="IoT 中心概述页上的默认指标图表。":::

此信息非常有用，但只表示可用于 IoT 中心的少量监视数据。 某些监视数据会自动收集，并可在创建 IoT 中心后立即进行分析。 你可以使用某些配置启用其他数据收集类型。

## <a name="what-is-azure-monitor"></a>说明是 Azure Monitor？

Azure IoT 中心使用 [Azure Monitor](/azure/azure-monitor/overview)创建监视数据，该服务是 azure 中的一种完整的堆栈监视服务，它提供了一组完整的功能来监视 Azure 资源以及其他云和本地的资源。

请从 [通过 Azure Monitor 监视 Azure 资源](/azure/azure-monitor/insights/monitor-azure-resource)一文开始，其中介绍了以下概念：

- 说明是 Azure Monitor？
- 与监视相关的成本
- 监视 Azure 中收集的数据
- 配置数据收集
- Azure 中用于分析监视数据并就其发出警报的标准工具

以下各节将介绍为 Azure IoT 中心收集的特定数据，并提供有关在 Azure tools 中配置数据收集和分析此数据的示例。

## <a name="monitoring-data"></a>监视数据

Azure IoT 中心会收集与 " [监视 azure 资源的数据](/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data-from-Azure-resources)" 中所述的其他 azure 资源相同的监视数据。

有关 Azure IoT 中心创建的指标和日志的详细信息，请参阅 [监视 Azure Iot 中心数据参考](monitor-iot-hub-reference.md) 。

> [!IMPORTANT]
> IoT 中心服务使用 Azure Monitor 资源日志发出的事件不保证可靠或有序。 某些事件可能会丢失或未按顺序传送。 资源日志也不是实时的，可能需要花费几分钟时间将事件记录到选择的目标。

## <a name="collection-and-routing"></a>集合和路由

平台指标和活动日志会自动收集和存储，但可以使用诊断设置将其路由到其他位置。

在创建诊断设置并将其路由到一个或多个位置之前，不会收集和存储资源日志。

指标和日志可路由到多个位置，包括：
- Azure Monitor 通过关联的 Log Analytics 工作区来记录存储。 可以使用 Log Analytics 进行分析。
- 用于存档和脱机分析的 Azure 存储 
- 外部应用程序可以读取的事件中心终结点，例如，第三方 SIEM 工具。

在 Azure 门户中，可以在 IoT 中心的左窗格中选择 " **监视** " 下的 " **诊断设置** "，然后选择 " **添加诊断设置** "，以创建作用域为 IoT 中心发出的日志和平台指标的诊断设置。

以下屏幕截图显示了用于将资源日志类型 *连接操作* 和所有平台指标路由到 Log Analytics 工作区的诊断设置。

:::image type="content" source="media/monitor-iot-hub/diagnostic-setting-portal.png" alt-text="IoT 中心概述页上的默认指标图表。" 下列出。

将 IoT 中心平台指标路由到其他位置时，请注意：

- 以下平台指标不可通过诊断设置导出：连接的 *设备 (预览)* 和 *设备总计 (预览)* 。

- 多维度量值（例如某些 [路由度量值](monitor-iot-hub-reference.md#routing-metrics)）当前导出为跨维度值聚合的平展单维度指标。 有关更多详细信息，请参阅 [将平台指标导出到其他位置](/azure/azure-monitor/platform/metrics-supported#exporting-platform-metrics-to-other-locations)。

## <a name="analyzing-metrics"></a>分析指标

你可以使用指标资源管理器通过从 " **Azure Monitor** " 菜单打开 **指标** 来使用指标资源管理器来分析 azure IoT 中心与其他 azure 服务的指标。 有关使用此工具的详细信息，请参阅 [Azure 指标资源管理器入门](/azure/azure-monitor/platform/metrics-getting-started)。

在 Azure 门户中，可以在 IoT 中心的左窗格中选择 " **监视** " 下的 " **指标** "，默认情况下，将指标资源管理器范围打开为 iot 中心发出的平台指标：

:::image type="content" source="media/monitor-iot-hub/metrics-portal.png" alt-text="IoT 中心概述页上的默认指标图表。":::

有关为 Azure IoT 中心收集的平台指标列表，请参阅 [监视 Azure Iot 中心数据参考中的指标](monitor-iot-hub-reference.md#metrics)。 有关为所有 Azure 服务收集的平台指标列表，请参阅 [Azure Monitor 支持的指标](/azure/azure-monitor/platform/metrics-supported)。

对于以计数单元收集的 IoT 中心平台指标，某些聚合可能不可用或不可用。 若要了解详细信息，请参阅 [监视 Azure IoT 中心数据参考中的支持的聚合](monitor-iot-hub-reference.md#supported-aggregations)。

某些 IoT 中心指标（如 [路由指标](monitor-iot-hub-reference.md#routing-metrics)）是多维的。 对于这些指标，可以根据维度对图表应用 [筛选器](/azure-monitor/platform/metrics-charts#apply-filters-to-charts) 和 [拆分](/azure/azure-monitor/platform/metrics-charts#apply-splitting-to-a-chart) 。

## <a name="analyzing-logs"></a>分析日志

Azure Monitor 日志中的数据存储在表中，其中每个表都具有自己的唯一属性集。 这些表中的数据与 Log Analytics 工作区相关联，并且可以在 Log Analytics 中进行查询。 若要详细了解 Azure Monitor 日志，请参阅 Azure Monitor 文档中的 [Azure Monitor 日志概述](/azure/azure-monitor/platform/data-platform-logs) 。 

若要将数据路由到 Azure Monitor 日志，必须创建一个诊断设置，以便将资源日志或平台指标发送到 Log Analytics 工作区。 若要了解详细信息，请参阅 [集合和路由](#collection-and-routing)。

在 Azure 门户中，你可以在 IoT 中心的左窗格中选择 " **监视** " 下的 " **日志** "，以对 Iot 中心的 Azure Monitor 日志中收集的日志和指标执行 Log Analytics 查询。

:::image type="content" source="media/monitor-iot-hub/logs-portal.png" alt-text="IoT 中心概述页上的默认指标图表。":::

有关 Log Analytics Azure Monitor 日志和可查询的表的列表，请参阅 [Monitoring Azure IoT 中心数据参考中的 Azure Monitor 日志表](monitor-iot-hub-reference.md#azure-monitor-logs-tables)。

Azure Monitor 中的所有资源日志都具有与服务特定字段相同的字段。 [Azure Monitor 资源日志架构](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-schema#top-level-resource-logs-schema)中概述了常见架构。 可以在 [监视 Azure Iot 中心数据参考的资源日志](monitor-iot-hub-reference.md#resource-logs)中找到为 Azure iot 中心收集的资源日志的架构和类别。

[活动日志](/azure/azure-monitor/platform/activity-log)是 Azure 中的一个平台日志，可用于深入了解订阅级事件。 您可以单独查看它或将其路由到 Azure Monitor 日志，您可以使用 Log Analytics 执行更复杂的查询。  

将 IoT 中心平台指标路由到 Azure Monitor 日志时，请注意：

- 以下平台指标不可通过诊断设置导出：连接的 *设备 (预览)* 和 *设备总计 (预览)* 。

- 多维度量值（例如某些 [路由度量值](monitor-iot-hub-reference.md#routing-metrics)）当前导出为跨维度值聚合的平展单维度指标。 有关更多详细信息，请参阅 [将平台指标导出到其他位置](/azure/azure-monitor/platform/metrics-supported#exporting-platform-metrics-to-other-locations)。

有关 IoT 中心的一些常见查询，请参阅 [Sample Kusto 查询](#sample-kusto-queries)。 有关使用 Log Analytics 查询的详细信息，请参阅 [Azure Monitor 中的日志查询概述](/azure/azure-monitor/log-query/log-query-overview)。

### <a name="sdk-version-in-iot-hub-logs"></a>IoT 中心日志中的 SDK 版本

IoT 中心资源日志中的一些操作在 `sdkVersion` 其对象中返回一个属性 `properties` 。 对于这些操作，当设备或后端应用程序使用其中一个 Azure IoT Sdk 时，此属性包含有关所使用的 SDK、SDK 版本和运行 SDK 的平台的信息。 下面的示例演示 `sdkVersion` [`deviceConnect`](monitor-iot-hub-reference.md#connections) 使用 Node.js 设备 SDK 时为操作发出的属性： `"azure-iot-device/1.17.1 (node v10.16.0; Windows_NT 10.0.18363; x64)"` 。 下面是一个示例，其中显示了 .NET (c # ) SDK 的值： `".NET/1.21.2 (.NET Framework 4.8.4200.0; Microsoft Windows 10.0.17763 WindowsProduct:0x00000004; X86)"` 。

下表显示了用于不同 Azure IoT Sdk 的 SDK 名称：

| SdkVersion 属性中的 SDK 名称 | 语言 |
|----------|----------|
| .NET | .NET (C#) |
| microsoft azure. 设备 | .NET (c # ) 服务 SDK |
| microsoft. client | .NET (c # ) 设备 SDK |
| iothubclient | C 或 Python v1 (弃用) 设备 SDK |
| iothubserviceclient | C 或 Python v1 (弃用) service SDK |
| azure iot-iothub-py | Python 设备 SDK |
| azure-iot-device | Node.js 设备 SDK |
| azure-iothub | Node.js 服务 SDK |
| iothub-客户端-客户端 | Java 设备 SDK |
| iothub （sdk）。 | Java 服务 SDK |
| ".com"-设备-客户端 | Java 设备 SDK |
| ".com"-服务-客户端 | Java 服务 SDK |
| C | Embedded C |
| C + (OSSimplified = Azure RTO)  | Azure RTOS |

对 IoT 中心资源日志执行查询时，可以提取 SDK 版本属性。 例如，下面的查询从连接操作返回的属性中提取 SDK 版本属性 (和设备 ID) 。 这两个属性将与操作的时间以及设备所连接到的 IoT 中心的资源 ID 一起写入结果。

```kusto
// SDK version of devices
// List of devices and their SDK versions that connect to IoT Hub
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
| where Category == "Connections"
| extend parsed_json = parse_json(properties_s) 
| extend SDKVersion = tostring(parsed_json.sdkVersion) , DeviceId = tostring(parsed_json.deviceId)
| distinct DeviceId, SDKVersion, TimeGenerated, _ResourceId
```

### <a name="sample-kusto-queries"></a>示例 Kusto 查询

> [!IMPORTANT]
> 从 IoT 中心菜单中选择 **日志** 时，会打开 Log Analytics 并将查询范围设置为当前 IoT 中心。 这意味着日志查询只包含来自该资源的数据。 如果要运行包含来自其他 IoT 中心或其他 Azure 服务数据的查询，请从 " **Azure Monitor** " 菜单中选择 " **日志** "。 有关详细信息，请参阅 [Azure Monitor Log Analytics 中的日志查询范围和时间范围](/azure/azure-monitor/log-query/scope/)。

下面是一些可用于帮助监视 IoT 中心的查询。

- 连接错误：确定设备连接错误。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where Category == "Connections" and Level == "Error"
    ```

- 限制错误：标识导致最多请求导致限制错误的设备。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where ResultType == "429001"
    | extend DeviceId = tostring(parse_json(properties_s).deviceId)
    | summarize count() by DeviceId, Category, _ResourceId
    | order by count_ desc
    ```

- 死终结点：通过报告问题的次数来确定死或不正常的终结点，以及原因。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where Category == "Routes" and OperationName in ("endpointDead", "endpointUnhealthy")
    | extend parsed_json = parse_json(properties_s)
    | extend Endpoint = tostring(parsed_json.endpointName), Reason = tostring(parsed_json.details) 
    | summarize count() by Endpoint, OperationName, Reason, _ResourceId
    | order by count_ desc
    ```

- 错误摘要：按类型跨所有操作的错误计数。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where Level == "Error"
    | summarize count() by ResultType, ResultDescription, Category, _ResourceId
    ```

- 最近连接的设备： IoT 中心在指定时间段内看到连接的设备的列表。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where Category == "Connections" and OperationName == "deviceConnect"
    | extend DeviceId = tostring(parse_json(properties_s).deviceId)
    | summarize max(TimeGenerated) by DeviceId, _ResourceId
    ```

- SDK 版本的设备：设备的列表及其 SDK 版本，适用于设备连接或设备到云克隆操作。

    ```kusto
    AzureDiagnostics
    | where ResourceProvider == "MICROSOFT.DEVICES" and ResourceType == "IOTHUBS"
    | where Category == "Connections" or Category == "D2CTwinOperations"
    | extend parsed_json = parse_json(properties_s)
    | extend SDKVersion = tostring(parsed_json.sdkVersion) , DeviceId = tostring(parsed_json.deviceId)
    | distinct DeviceId, SDKVersion, TimeGenerated, _ResourceId
    ```

### <a name="read-logs-from-azure-event-hubs"></a>从 Azure 事件中心读取日志

通过诊断设置来设置事件日志记录后，可以创建应用程序以读出日志，从而可以根据日志中的信息采取措施。 以下示例代码从事件中心检索日志：

```csharp
class Program
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}";
    static string monitoringEndpointName = "{your AMS event hub endpoint name}";
    static EventHubClient eventHubClient;
    //This is the Diagnostic Settings schema
    class AzureMonitorDiagnosticLog
    {
        string time { get; set; }
        string resourceId { get; set; }
        string operationName { get; set; }
        string category { get; set; }
        string level { get; set; }
        string resultType { get; set; }
        string resultDescription { get; set; }
        string durationMs { get; set; }
        string callerIpAddress { get; set; }
        string correlationId { get; set; }
        string identity { get; set; }
        string location { get; set; }
        Dictionary<string, string> properties { get; set; }
    };

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }
        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));
            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
                var deserializer = new JavaScriptSerializer();
                //deserialize json data to azure monitor object
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result);
            }
        }
    }
}
```

## <a name="alerts"></a>警报

在监视数据中发现重要情况时，Azure Monitor 警报会主动通知你。 它们允许你确定和解决你的系统中的问题，然后客户就会注意到它们。 你可以对 [指标](/azure/azure-monitor/platform/alerts-metric-overview)、 [日志](/azure/azure-monitor/platform/alerts-unified-log)和 [活动日志](/azure/azure-monitor/platform/activity-log-alerts)设置警报。 不同类型的警报具有优点和缺点。

基于平台指标创建警报规则时，请注意，对于以计数单位收集的 IoT 中心平台指标，某些聚合可能不可用或不可用。 若要了解详细信息，请参阅 [监视 Azure IoT 中心数据参考中的支持的聚合](monitor-iot-hub-reference.md#supported-aggregations)。

## <a name="next-steps"></a>后续步骤

- 请参阅 [监视 Azure IoT 中心数据参考](monitor-iot-hub-reference.md) ，了解 [service name] 创建的指标、日志和其他重要值的引用。

- 有关监视 Azure 资源的详细信息，请参阅[使用 Azure Monitor 监视 Azure 资源](/azure/azure-monitor/insights/monitor-azure-resource)。
