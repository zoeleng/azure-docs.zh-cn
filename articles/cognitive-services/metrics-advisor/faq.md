---
title: 指标顾问常见问题
titleSuffix: Azure Cognitive Services
description: 有关指标 Advisor 服务的常见问题解答。
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 10/15/2020
ms.author: mbullwin
ms.openlocfilehash: da4dc3579630d641fcbc1d4321b56de0cc09d555
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92893571"
---
# <a name="metrics-advisor-frequently-asked-questions"></a>指标顾问常见问题

### <a name="what-is-the-cost-of-my-instance"></a>实例的成本是多少？

在预览期间，当前不需要使用实例。

### <a name="why-cant-i-create-the-resource-the-pricing-tier-is-unavailable-and-it-says-you-have-already-created-1-s0-for-this-subscription"></a>为什么无法创建资源？ "定价层" 不可用，显示 "你已为此订阅创建了1个 S0"？

:::image type="content" source="media/pricing.png" alt-text="F0 资源已存在时的消息":::

公共预览期间，只能在订阅下为每个区域创建一个指标顾问实例。

如果已在同一区域中使用相同的订阅创建了一个实例，则可以尝试不同的区域或不同的订阅来创建新的实例。 还可以删除现有实例以创建新实例。

如果已删除现有实例，但仍看到此错误，请在删除资源后等待大约20分钟，然后再创建新的实例。

## <a name="basic-concepts"></a>基本概念

### <a name="what-is-multi-dimensional-time-series-data"></a>什么是多维时序数据？

请参阅术语表中的 [多维指标](glossary.md#multi-dimensional-metric)  定义。

### <a name="how-much-data-is-needed-for-metrics-advisor-to-start-anomaly-detection"></a>指标顾问启动异常检测需要多少数据？

至少一个数据点可以触发异常情况检测。 不过，这并不能带来最佳准确性。 该服务将使用您在数据馈送创建期间指定为 "填充间隔" 规则的值来假定以前数据点的窗口。

建议在要检测的时间戳之前包含一些数据。
根据数据的粒度，推荐的数据量会根据下面的情况而变化。

| 粒度 | 用于检测的建议数据量 |
| ----------- | ------------------------------------- |
| 不到5分钟 | 4天的数据 |
| 5分钟到1天 | 28天的数据 |
| 超过1天，到31天 | 4年的数据 |
| 超过31天 | 48年的数据 |

### <a name="why-metrics-advisor-doesnt-detect-anomalies-from-historical-data"></a>为什么指标顾问不会从历史数据中检测到异常？

指标顾问旨在检测实时流式处理数据。 此服务将查看并运行异常检测的历史数据的最大长度限制。 这意味着，只有在某些最早时间戳之后的数据点才会有异常检测结果。 最早的时间戳取决于数据的粒度。

根据数据的粒度，会有异常检测结果的历史数据的长度如下所示。

| 粒度 | 异常情况检测的历史数据的最大长度 |
| ----------- | ------------------------------------- |
| 不到5分钟 | 板载时间-13 小时 |
| 5分钟到小于1小时 | 板载时间-4 天  |
| 1小时到1天以内 | 板载时间-14 天  |
| 1 天 | 板载时间-28 天  |
| 超过1天，小于31天 | 板载时间-2 年  |
| 超过31天 | 板载时间-24 年   |

### <a name="more-concepts-and-technical-terms"></a>更多概念和技术术语

有关详细信息，另请参阅 [词汇表](glossary.md) 。

###  <a name="how-do-i-write-a-valid-query-for-ingesting-my-data"></a>如何实现编写有效的查询以引入我的数据吗？  

要使指标顾问引入数据，您需要创建一个查询，该查询将以单个时间戳返回数据的尺寸。 指标顾问将多次运行此查询，以从每个时间戳获取数据。 

请注意，查询应在给定时间戳为每个维度组合最多返回一条记录。 返回的所有记录必须具有相同的时间戳。 查询不应返回重复记录。

例如，假设你在下面创建了一个每日度量值的查询： 
 
`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(DAY, 1, @StartTime)`

确保为时序使用正确的粒度。 对于每小时指标，你应使用： 

`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(hour, 1, @StartTime)`

请注意，这些查询仅以单个时间戳返回数据，并包含由度量顾问引入的所有维度组合。 

:::image type="content" source="media/query-result.png" alt-text="F0 资源已存在时的消息" lightbox="media/query-result.png":::


### <a name="how-do-i-detect-spikes--dips-as-anomalies"></a>如何实现检测峰值 & 异常？

如果预定义了硬阈值，实际上可以在 [异常检测配置](how-tos/configure-metrics.md#anomaly-detection-methods)中手动设置 "硬阈值"。
如果没有阈值，可以使用 AI 提供支持的 "智能检测"。 有关详细信息，请参阅 [优化检测配置](how-tos/configure-metrics.md#tune-the-detecting-configuration) 。

### <a name="how-do-i-detect-inconformity-with-regular-seasonal-patterns-as-anomalies"></a>如何实现检测具有常规 (季节性的 inconformity 是否为异常) 模式？

"智能检测" 可以了解包含季节性模式的数据的模式。 然后，它将检测不符合常规模式的数据点作为异常。 有关详细信息，请参阅 [优化检测配置](how-tos/configure-metrics.md#tune-the-detecting-configuration) 。

### <a name="how-do-i-detect-flat-lines-as-anomalies"></a>如何实现将平面线条作为异常检测？

如果数据通常非常不稳定，并且您希望在其变得太稳定或甚至变成平面线时收到警报，则可以将 "更改阈值" 配置为在更改太小时检测此类数据点。
有关详细信息，请参阅 [异常情况检测配置](how-tos/configure-metrics.md#anomaly-detection-methods) 。

## <a name="advanced-concepts"></a>高级概念

### <a name="how-does-metric-advisor-build-an-incident-tree-for-multi-dimensional-metrics"></a>指标顾问如何为多维度量值生成事件树？

度量值可以按维度拆分为多个时序。 例如， `Response latency` 对团队拥有的所有服务监视指标。 `Service`类别可以用作维度来丰富指标，因此，我们将 `Response latency` 按 `Service1` 、等拆分 `Service2` 。 每个服务可以部署在多个数据中心的不同计算机上，因此，指标可以进一步由 `Machine` 和拆分 `Data center` 。

|服务| 数据中心| 计算机  | 
|----|------|----------------   |
| S1 |  DC1 |   M1 |
| S1 |  DC1 |   M2 |
| S1 |  DC2 |   M3 |
| S1 |  DC2 |   M4 |
| S2 |  DC1 |   M1 |
| S2 |  DC1 |   M2 |
| S2 |  DC2 |   M5 |
| S2 |  DC2 |   M6 |
| ...|      |      |

从总体开始 `Response latency` ，我们可以向下钻取和的度量 `Service` 值 `Data center` `Machine` 。 不过，如果服务所有者使用该路径，可能更有意义 `Service`  ->  `Data center`  ->  `Machine` ，或者可能更适合基础结构工程师使用该路径 `Data Center`  ->  `Machine`  ->  `Service` 。 这一切都取决于用户的各个业务要求。 

在指标顾问中，用户可以指定要向下钻取或从分层拓扑的一个节点中汇总的任何路径。 更准确地说，分层拓扑是一个定向非循环图形，而不是树结构。 有一个完整的分层拓扑，其中包括所有可能的维度组合，如下所示： 

:::image type="content" source="media/dimension-combinations-view.png" alt-text="F0 资源已存在时的消息" lightbox="media/dimension-combinations-view.png":::

理论上，如果维度 `Service` 具有 `Ls` 非重复值，维度具有非重复值， `Data center` `Ldc` 并且维度 `Machine` 具有不同的值 `Lm` ，则 `(Ls + 1) * (Ldc + 1) * (Lm + 1)` 层次结构拓扑中可能存在维度组合。 

但通常并非所有维度组合都是有效的，这可能会显著降低复杂性。 目前，如果用户自行聚合度量值，则不会限制维度数。 如果需要使用指标顾问提供的汇总功能，则维度数量不应超过6。 但是，我们会将度量值扩展的时序数量限制为小于10000。

"诊断" 页中的 **事件树** 工具仅显示检测到异常的节点，而不是整个拓扑。 这是为了帮助你专注于当前问题。 它也可能不会显示指标内的所有异常，而会根据贡献显示最常见的异常。 通过这种方式，我们可以快速找出异常数据的影响、范围和传播路径。 这可以显著减少需要重点关注的异常的数量，并可帮助用户了解并找到其关键问题。 
 
例如，当发生异常时 `Service = S2 | Data Center = DC2 | Machine = M5` ，异常的偏差会影响父节点 `Service= S2` ，该节点也已检测到异常，但异常不会影响 `DC2` 和所有服务上的整个数据中心 `M5` 。 事件树的构建方式如下：下面的屏幕截图中捕获了最常见的异常 `Service = S2` ，可以在两个路径中分析根本原因，这两个路径都将导致 `Service = S2 | Data Center = DC2 | Machine = M5` 。

 :::image type="content" source="media/root-cause-paths.png" alt-text="5个标记的顶点，其中的两个不同路径由边缘和一个标记为 S2 的常见节点连接。最常见的异常在服务 = S2 上捕获，根本原因可以通过两个路径来进行分析，这两个路径会导致服务 = S2 |数据中心 = DC2 |计算机 = M5" lightbox="media/root-cause-paths.png":::

## <a name="next-steps"></a>后续步骤
- [指标顾问概述](overview.md)
- [使用 Web 门户](quickstarts/web-portal.md)