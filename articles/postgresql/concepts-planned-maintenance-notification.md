---
title: 计划内维护通知-Azure Database for PostgreSQL-单服务器
description: 本文介绍 Azure Database for PostgreSQL-单服务器中的计划内维护通知功能
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/21/2020
ms.openlocfilehash: a0d61488fe5e7e91d025c7d3b8d3aa862b9cd9bd
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92428815"
---
# <a name="planned-maintenance-notification-in-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL 单服务器中的计划内维护通知

了解如何为 Azure Database for PostgreSQL 上的计划内维护事件做准备。

## <a name="what-is-a-planned-maintenance"></a>什么是计划内维护？

Azure Database for PostgreSQL 服务执行基础硬件、操作系统和数据库引擎的自动修补。 修补程序包括新的服务功能、安全和软件更新。 对于 PostgreSQL 引擎，次要版本升级自动进行，作为修补周期的一部分包含在内。 无需任何用户操作或配置设置即可进行修补。 此修补程序经过广泛测试，并使用安全部署实践推出。

计划内维护是将这些服务更新部署到给定 Azure 区域中的服务器时的维护时段。 在计划内维护期间，会创建通知事件，通知客户在托管其服务器的 Azure 区域中部署服务更新。 两次计划内维护之间的最短持续时间为30天。 你会提前收到下一个维护时段72小时的通知。

## <a name="planned-maintenance---duration-and-customer-impact"></a>计划内维护-持续时间和客户影响

给定 Azure 区域的计划内维护通常会运行15小时。 如果需要，该窗口还包括执行回滚计划的缓冲时间。 在计划内维护期间，可能会发生数据库服务器重启或故障转移，这可能导致最终用户的数据库服务器暂时不可用。 Azure Database for PostgreSQL 服务器在容器中运行，因此数据库服务器重启通常会迅速完成，通常在60-120 秒内完成。 由工程团队认真监视包括每个服务器重启在内的整个计划内维护事件。 服务器故障转移时间依赖于数据库恢复时间，这会导致在故障转移时，如果服务器上有大量的事务活动，数据库会变得更长。 为避免重新启动时间较长，建议避免在计划内维护事件)  (大容量加载的长时间运行的事务。

总之，虽然计划内维护事件运行15小时，但单个服务器的影响通常会持续60秒，具体取决于服务器上的事务活动。 在计划的维护开始之前，会将通知发送到72个日历小时，另一个通知将在给定区域进行维护。

## <a name="how-can-i-get-notified-of-planned-maintenance"></a>如何获得计划内维护的通知？

你可以利用计划内维护通知功能来接收即将发生的计划内维护事件的警报。 你将在事件前收到有关即将发生的维护72日历小时的通知，而另一个则在针对给定区域进行维护时进行。

### <a name="planned-maintenance-notification"></a>计划内维护通知

> [!IMPORTANT]
> 计划内维护通知当前在美国西部地区 **以外** 的所有区域中提供预览版

**计划内维护通知** 允许您将即将发生的计划内维护事件的警报接收到 Azure Database for PostgreSQL。 这些通知与[服务运行状况](../service-health/overview.md)计划内维护集成，允许你在同一位置查看你的订阅的所有计划内维护。 它还有助于将通知扩展到不同资源组的适当受众，因为你可能有不同的联系人负责不同的资源。 你将在事件之前的即将到来的维护72日历小时内收到通知。

我们将尽一切努力为所有事件提供**计划内维护通知** 72 小时通知。 但是，对于关键或安全修补程序，通知可能会在事件快要发生时更晚一点发送，或者会被忽略。

可以在 Azure 门户上检查计划内的维护通知，也可以配置警报以接收通知。 

### <a name="check-planned-maintenance-notification-from-azure-portal"></a>从 Azure 门户检查计划内维护通知

1. 在 [Azure 门户](https://portal.azure.com)中，选择 " **服务运行状况**"。
2. 选择 **计划的维护** 选项卡
3. 选择要检查计划内维护通知的 " **订阅**"、"区域" 和 " **服务** "。 
   
### <a name="to-receive-planned-maintenance-notification"></a>接收计划内维护通知

1. 在[门户](https://portal.azure.com)中，选择“服务运行状况”。
2. 在“警报”部分中，选择“运行状况警报”。
3. 选择“+ 添加服务运行状况警报”，并填写字段。
4. 填写所需的字段。 
5. 选择“事件类型”，然后选择“计划内维护”或“全选”
6. 在“操作组”中，定义接收警报的方式（获取电子邮件、触发逻辑应用等）。  
7. 确保“创建后启用规则”设置为“是”。
8. 选择“创建警报规则”以完成警报

有关如何创建服务运行状况警报的详细步骤，请参阅 [创建有关服务通知的活动日志警报](../service-health/alerts-activity-log-service-notifications.md)。

## <a name="can-i-control-or-change-planned-maintenance-for-my-servers-after-i-receive-a-notification-event"></a>收到通知事件后，是否可以控制或更改服务器的计划内维护？

维护是为了使服务器保持安全、稳定和保持最新状态。 计划内维护事件无法取消或延迟。 将通知发送到给定的 Azure 区域后，不能对该区域中的任何单个服务器进行修补计划更改。 同时为整个区域推出修补程序。 Azure Database for PostgreSQL 单服务器服务适用于不需要对服务进行精细控制或自定义的云本机应用程序。 如果你希望能够为服务器计划维护，则建议你考虑使用 [灵活的服务器](./flexible-server/overview.md)。

## <a name="are-all-the-azure-regions-patched-at-the-same-time"></a>是否同时修补了所有 Azure 区域？

不会，所有 Azure 区域都是在部署时的时间范围内进行修补。 部署时窗口通常在给定的 Azure 区域中从晚上5点到上午8点（本地时间）。 地域配对的 Azure 区域会在不同的天进行修补。 为了实现数据库服务器的高可用性和业务连续性，我们建议使用 [跨区域读取副本](./concepts-read-replicas.md#cross-region-replication) 。

## <a name="retry-logic"></a>重试逻辑

暂时性错误也称为暂时性故障，是一种可以自行解决的错误。 在维护期间可能会发生[暂时性错误](./concepts-connectivity.md#transient-errors)。 系统在 60 秒以内可自动解决其中的大部分事件。 应使用 [重试逻辑](./concepts-connectivity.md#handling-transient-errors)来处理暂时性错误。


## <a name="next-steps"></a>后续步骤

- 有关使用 Azure Database for PostgreSQL 的任何问题或建议，请将电子邮件发送到 Azure Database for PostgreSQL 团队 *AskAzureDBforMySQL@service.microsoft.com* 。
- 有关如何基于指标创建警报的指南，请参阅[如何设置警报](howto-alert-on-metric.md)。
- [排查 Azure Databases for PostgreSQL - 单一服务器的连接问题](howto-troubleshoot-common-connection-issues.md)
- [处理暂时性错误并高效连接到 Azure Database for PostgreSQL 单服务器](concepts-connectivity.md)
