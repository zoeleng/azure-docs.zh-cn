---
title: 启用了 Azure Arc 的服务器的 VM 扩展管理
description: 启用 Azure Arc 的服务器可以管理虚拟机扩展的部署，这些扩展提供部署后配置和自动化任务和非 Azure Vm。
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: e9865761fd3e5897ee3f01cd3d6ca620d5ea2f4b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92460880"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>启用了 Azure Arc 服务器的虚拟机扩展管理

虚拟机 (VM) 扩展是小型应用程序，用于在 Azure Vm 上提供部署后配置和自动化任务。 例如，如果某个虚拟机需要安装软件、防病毒保护或运行脚本，便可以使用 VM 扩展。

使用启用了 azure Arc 的服务器，可以将 Azure VM 扩展部署到非 Azure Windows 和 Linux Vm，从而简化混合计算机在本地、边缘和其他云环境中的生命周期管理。 可以在混合计算机上或通过启用了 Arc 的服务器管理的服务器上使用以下方法来管理 VM 扩展：

- [Azure 门户](manage-vm-extensions-portal.md)
- [Azure CLI](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- Azure [资源管理器模板](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>主要优点

支持 Azure Arc 的服务器 VM 扩展支持提供以下主要优势：

- 使用 [Azure 自动化状态配置](../../automation/automation-dsc-overview.md) 集中存储配置，并通过 DSC VM 扩展维护启用混合连接的计算机的所需状态。

- 收集日志数据以便通过 Log Analytics 代理 VM 扩展启用 [Azure Monitor 中的日志](../../azure-monitor/platform/data-platform-logs.md) 进行分析。 这对于跨各种源中的数据执行复杂分析非常有用。

- 在 [用于 VM 的 Azure Monitor](../../azure-monitor/insights/vminsights-overview.md)中，会分析 Windows 和 Linux vm 的性能，并监视其进程和其他资源和外部进程的依赖项。 这是通过同时启用 Log Analytics 代理和依赖项代理 VM 扩展来实现的。

- 使用自定义脚本扩展在混合连接的计算机上下载和执行脚本。 此扩展适用于部署后配置、软件安装或其他任何配置或管理任务。

## <a name="availability"></a>可用性

VM 扩展功能仅在 [受支持区域](overview.md#supported-regions)的列表中提供。 确保你在其中一个区域中载入计算机。

## <a name="extensions"></a>扩展

在此版本中，我们在 Windows 和 Linux 计算机上支持以下 VM 扩展。

|扩展 |OS |Publisher |其他信息 |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Windows 自定义脚本扩展](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft PowerShell|[Windows PowerShell DSC 扩展](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics 代理 |Windows |Microsoft.EnterpriseCloud.Monitoring |[适用于 Windows 的 Log Analytics VM 扩展](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft 依赖项代理 | Windows |Microsoft.Compute | [适用于 Windows 的依赖关系代理虚拟机扩展](../../virtual-machines/extensions/agent-dependency-windows.md)|
|CustomScript|Linux |Microsoft Azure 扩展 |[Linux 自定义脚本扩展版本2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft.OSTCExtensions |[适用于 Linux 的 PowerShell DSC 扩展](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics 代理 |Linux |Microsoft.EnterpriseCloud.Monitoring |[适用于 Linux 的 Log Analytics VM 扩展](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft 依赖项代理 | Linux |Microsoft.Compute | [适用于 Linux 的依赖关系代理虚拟机扩展](../../virtual-machines/extensions/agent-dependency-linux.md) |

若要了解有关 Azure 连接的计算机代理包以及有关扩展代理组件的详细信息，请参阅 [代理概述](agent-overview.md#agent-component-details)。

## <a name="prerequisites"></a>先决条件

此功能依赖于订阅中的以下 Azure 资源提供程序：

- Microsoft.HybridCompute
- Microsoft.GuestConfiguration

如果尚未注册，请按照 [注册 Azure 资源提供程序](agent-overview.md#register-azure-resource-providers)中的步骤进行操作。

适用于 Linux 的 Log Analytics 代理 VM 扩展需要在目标计算机上安装 Python 2.x。

### <a name="connected-machine-agent"></a>Connected Machine 代理

验证计算机是否与支持 Azure 连接的计算机代理的 Windows 和 Linux 操作系统的 [支持版本](agent-overview.md#supported-operating-systems) 相匹配。

Windows 和 Linux 上此功能支持的已连接计算机代理的最低版本为1.0 版。

若要将计算机升级到所需的代理版本，请参阅 [升级代理](manage-agent.md#upgrading-agent)。

## <a name="next-steps"></a>后续步骤

你可以使用 [Azure CLI](manage-vm-extensions-cli.md) [PowerShell](manage-vm-extensions-powershell.md)、 [Azure 门户](manage-vm-extensions-portal.md)或 [Azure 资源管理器模板](manage-vm-extensions-template.md)来部署、管理和删除 VM 扩展。
