---
title: '用于 VM 的 Azure Monitor 来宾健康状况 (预览版疑难解答) '
description: 描述在用于 VM 的 Azure Monitor 运行状况问题时可以采取的故障排除步骤。
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/08/2020
ms.openlocfilehash: 048230bb85d2ac599efc6bd3fb326f98038cce57
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686703"
---
# <a name="troubleshoot-azure-monitor-for-vms-guest-health-preview"></a>用于 VM 的 Azure Monitor 来宾健康状况 (预览版疑难解答) 
本文介绍在用于 VM 的 Azure Monitor 运行状况问题时可以采取的故障排除步骤。

## <a name="error-message-that-no-data-is-available"></a>错误消息，指出没有可用的数据 

![无数据](media/vminsights-health-troubleshoot/no-data.png)


### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>验证虚拟机是否满足配置要求

1. 验证虚拟机是否为 Azure 虚拟机。 当前不支持服务器的 Azure Arc。
2. 验证虚拟机是否正在运行 [受支持的操作系统](vminsights-health-enable.md?current-limitations.md)。
3. 验证虚拟机是否安装在 [受支持的区域](vminsights-health-enable.md?current-limitations.md)中。
4. 验证 Log Analytics 的工作区是否安装在 [受支持的区域](vminsights-health-enable.md?current-limitations.md)中。

### <a name="verify-that-the-vm-is-properly-onboarded"></a>验证 VM 是否已正确载入
验证是否已在虚拟机上成功预配 Azure Monitor 代理扩展和来宾 VM 健康代理。 在 Azure 门户的虚拟机菜单中选择 " **扩展** "。 如果列出了两个代理，请参阅 []() 。

![VM 扩展](media/vminsights-health-troubleshoot/extensions.png)

### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>验证是否在虚拟机上启用了系统分配的标识
验证虚拟机上是否已启用系统分配的标识。 从 Azure 门户中的虚拟机菜单中选择 " **标识** "。 如果列出了两个代理，请参阅 []() 。

![系统分配标识](media/vminsights-health-troubleshoot/system-identity.png)

### <a name="verify-data-collection-rule"></a>验证数据收集规则
验证指定 health extension 作为数据源的数据收集规则是否与该虚拟机关联。

## <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>由于权限不足而导致错误请求的错误消息
此错误表示未在订阅中注册 **WorkloadMonitor** 资源提供程序。 有关注册此资源提供程序的详细信息，请参阅 [Azure 资源提供程序和类型](/azure/azure-resource-manager/management/resource-providers-and-types#register-resource-provider) 。 

![错误的请求](media/vminsights-health-troubleshoot/bad-request.png)

## <a name="next-steps"></a>后续步骤

- [获取用于 VM 的 Azure Monitor 来宾健康功能的概述](vminsights-health-overview.md)
