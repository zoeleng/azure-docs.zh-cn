---
title: 监视 Azure Monitor 中 Log Analytics 工作区的运行状况
description: 介绍如何使用操作表中的数据监视 Log Analytics 工作区的运行状况。
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/20/2020
ms.openlocfilehash: d6c29cb41d38e5473a9b24dbc89fd99d3e19c16f
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92638323"
---
# <a name="monitor-health-of-log-analytics-workspace-in-azure-monitor"></a>监视 Azure Monitor 中 Log Analytics 工作区的运行状况
若要在 Azure Monitor 中维护 Log Analytics 工作区的性能和可用性，需要能够主动检测出现的任何问题。 本文介绍如何使用 [操作](https://docs.microsoft.com/azure/azure-monitor/reference/tables/operation) 表中的数据监视 Log Analytics 工作区的运行状况。 此表包含在每个 Log Analytics 工作区中，并包含工作区中发生的错误和警告。 你应定期查看此数据，并创建警报，以便在工作区中有任何重要事件时主动收到通知。

## <a name="_logoperation-function"></a>_LogOperation 函数
Azure Monitor 日志会将有关任何问题的详细信息发送到出现问题的工作区中的 [操作](https://docs.microsoft.com/azure/azure-monitor/reference/tables/operation) 表。 **_LogOperation** 系统函数基于 **操作** 表，并提供一组简化的信息用于分析和警报。

## <a name="columns"></a>列

**_LogOperation** 函数将返回下表中的列。

| 列 | 说明 |
|:---|:---|
| TimeGenerated | 事件发生的时间（UTC）。 |
| 类别  | 操作类别组。 可用于筛选操作类型，帮助创建更精确的系统审核和警报。 有关类别的列表，请参阅下面的部分。 |
| Operation  | 操作类型的说明。 这可以指示 Log Analytics 的限制、操作类型或过程的一部分。 |
| Level | 问题的严重级别：<br>-Info：无需特别注意。<br>-警告：进程未按预期完成，需要注意。<br>-错误：进程失败，需要紧急关注。 
| 详细信息 | 操作的详细说明，包括特定的错误消息（如果存在）。 |
| _ResourceId | 与操作相关的 Azure 资源的资源 ID。  |
| Computer | 如果操作与 Azure Monitor 代理相关，则为计算机名。 |
| CorrelationId | 用于对连续相关操作进行分组。 |


## <a name="categories"></a>类别
下表描述了 _LogOperation 函数的类别。 

| 类别 | 说明 |
|:---|:---|
| 引流           | 作为数据引入过程的一部分的操作。 有关详细信息，请参阅下文。 |
| 代理               | 指示安装代理时出现问题。 |
| 数据收集     | 与数据收集过程相关的操作。 |
| 解决方案目标  | 已处理类型为 *ConfigurationScope* 的操作。 |
| 评估解决方案 | 已执行评估过程。 |


### <a name="ingestion"></a>引流
引入操作是数据引入过程中出现的问题，包括有关到达 Azure Log Analytics 工作区限制的通知。 此类别中的错误条件可能会导致数据丢失，因此它们对于监视尤其重要。 下表提供了有关这些操作的详细信息。 请参阅 [Azure Monitor 服务](../service-limits.md#log-analytics-workspaces) 限制 Log Analytics 工作区的服务限制。


| Operation | Level | 详细信息 | 相关文章 |
|:---|:---|:---|:---|
| 自定义日志 | Error   | 已达到自定义字段列限制。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) |
| 自定义日志 | Error   | 自定义日志引入失败。 | |
| 新元. | Error | 检测到配置错误。 | |
| 数据收集 | Error   | 数据已删除，因为该请求的创建时间早于设置的天数。 | [使用 Azure Monitor 日志管理使用情况和成本](manage-cost-storage.md#alert-when-daily-cap-reached)
| 数据收集 | 信息    | 检测到收集计算机配置。| |
| 数据收集 | 信息    | 数据收集因新日期而开始。 | [使用 Azure Monitor 日志管理使用情况和成本](/manage-cost-storage.md#alert-when-daily-cap-reached) |
| 数据收集 | 警告 | 由于已达到每日限制，数据收集已停止。| [使用 Azure Monitor 日志管理使用情况和成本](/manage-cost-storage.md#alert-when-daily-cap-reached) |
| 数据处理 | Error   | JSON 格式无效。 | [使用 HTTP 数据收集器 API（公共预览版）将日志数据发送到 Azure Monitor](data-collector-api.md#request-body) | 
| 数据处理 | 警告 | 已将值修整为允许的最大大小。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) |
| 数据处理 | 警告 | 已达到大小限制的字段值被剪裁。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) | 
| 引入速率 | 信息 | 引入速率限制接近70%。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) |
| 引入速率 | 警告 | 引入速率限制已达到限制。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) |
| 引入速率 | Error   | 已达到速率限制。 | [Azure Monitor 服务限制](../service-limits.md#log-analytics-workspaces) |
| 存储 | Error   | 由于使用的凭据无效，无法访问存储帐户。  |



   

## <a name="alert-rules"></a>警报规则
当 Log Analytics 工作区中检测到问题时，请使用 Azure Monitor 中的 [日志查询警报](../platform/alerts-log-query.md) 主动收到通知。 你应该使用一种策略，使你能够及时响应问题，同时最大限度地降低成本。 你的订阅将按每个警报规则收费，具体取决于它所评估的频率。

建议的策略是基于问题级别从两个警报规则开始。 使用较短的频率（例如每5分钟一次）进行错误，并使用较长的频率，如警告的24小时。 由于错误表示潜在的数据丢失，因此你希望快速响应这些错误，以最大程度地降低损失。 警告通常指示不需要立即关注的问题，因此你可以每天查看它们。

使用 "创建" [、"查看" 和 "管理" 日志 Azure Monitor 警报](../platform/alerts-log.md) 中的过程创建日志警报规则。 以下各节介绍每个规则的详细信息。


| 查询 | 阈值 | 周期 | 频率 |
|:---|:---|:---|:---|
| `_LogOperation | where Level == "Error"`   | 0 | 5 | 5 |
| `_LogOperation | where Level == "Warning"` | 0 | 1440 | 1440 |

这些警报规则将响应所有操作，并出现错误或警告。 随着更熟悉正在生成警报的操作，可能需要对特定操作做出不同的响应。 例如，你可能想要将通知发送给不同的人员进行特定操作。 

若要为特定操作创建警报规则，请使用包含 " **类别** " 和 " **操作** " 列的查询。 

以下示例在引入量速率达到80% 的限制时创建警告性警报。

- 目标：选择 Log Analytics 工作区
- 条件：
  - 信号名称：自定义日志搜索
  - 搜索查询： `_LogOperation | where Category == "Ingestion" | where Operation == "Ingestion rate" | where Level == "Warning"`
  - 依据：结果数
  - 条件：大于
  - 阈值：0
  - 时间段：5（分钟）
  - 频率：5（分钟）
- 警报规则名称：达到每日数据限制
- 严重性：警告（严重性 1）


当数据收集达到每日限制时，以下示例将创建一个警告警报。 

- 目标：选择 Log Analytics 工作区
- 条件：
  - 信号名称：自定义日志搜索
  - 搜索查询： `_LogOperation | where Category == "Ingestion" | where Operation == "Data Collection" | where Level == "Warning"`
  - 依据：结果数
  - 条件：大于
  - 阈值：0
  - 时间段：5（分钟）
  - 频率：5（分钟）
- 警报规则名称：达到每日数据限制
- 严重性：警告（严重性 1）



## <a name="next-steps"></a>后续步骤

- 详细了解 [日志警报](alerts-log.md)。
- 收集工作区的[查询审核数据](../log-query/query-audit.md)。
