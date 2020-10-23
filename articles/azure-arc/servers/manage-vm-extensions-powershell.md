---
title: 使用 Azure PowerShell 启用 VM 扩展
description: 本文介绍如何使用 Azure PowerShell 将虚拟机扩展部署到在混合云环境中运行的支持 Azure Arc 的服务器。
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: 631aa323fee8db712acc975336bdbf9436833240
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462871"
---
# <a name="enable-azure-vm-extensions-using-azure-powershell"></a>使用 Azure PowerShell 启用 Azure VM 扩展

本文介绍如何使用 Azure PowerShell 将启用了 Azure Arc 的服务器支持的 Azure VM 扩展部署到 Linux 或 Windows 混合计算机并将其卸载。

## <a name="prerequisites"></a>先决条件

- 配备 Azure PowerShell 的计算机。 有关说明，请参阅[安装和配置 Azure PowerShell](/powershell/azure/)。

使用 Azure PowerShell 在启用了 Arc 的服务器管理的混合服务器上管理 VM 扩展之前，需要安装该 `Az.ConnectedMachine` 模块。 在启用 Arc 的服务器上运行以下命令：

`Install-Module -Name Az.ConnectedMachine`.

安装完成后，将返回以下消息：

`The installed extension `Az. ConnectedMachine` is experimental and not covered by customer support. Please use with discretion.`

## <a name="enable-extension"></a>启用扩展

若要在启用了 Arc 的服务器上启用 VM 扩展，请将 [AzConnectedMachineExtension](/powershell/module/az.connectedmachine/new-azconnectedmachineextension) 与 `-Name` 、、、 `-ResourceGroupName` `-MachineName` `-Location` 、 `-Publisher` 、- `ExtensionType` 和 `-Settings` 参数一起使用。

以下示例在启用了 Arc 的 Linux 服务器上启用 Log Analytics VM 扩展：

```powershell
PS C:\> $Setting = @{ "workspaceId" = "workspaceId" }
PS C:\> $protectedSetting = @{ "workspaceKey" = "workspaceKey" }
PS C:\> New-AzConnectedMachine -Name OMSLinuxAgent -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "eastus" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -TypeHandlerVersion "1.10" -Settings $Setting -ProtectedSetting $protectedSetting -ExtensionType OmsAgentforLinux"
```

下面的示例在启用 Arc 的服务器上启用自定义脚本扩展：

```powershell
PS C:\> $Setting = @{ "commandToExecute" = "powershell.exe -c Get-Process" }
PS C:\> New-AzConnectedMachine -Name custom -ResourceGroupName myResourceGroup -MachineName myMachineName -Location eastus -Publisher "Microsoft.Compute" -TypeHandlerVersion 1.10 -Settings $Setting -ExtensionType CustomScriptExtension
```

## <a name="list-extensions-installed"></a>列出已安装的扩展

若要获取启用了 Arc 的服务器上的 VM 扩展列表，请将 [AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) 与 `-MachineName` 和参数一起使用 `-ResourceGroupName` 。

示例：

```powershell
Get-AzConnectedMachineExtension -ResourceGroupName myResourceGroup -MachineName myMachineName

Name    Location  PropertiesType        ProvisioningState
----    --------  --------------        -----------------
custom  westus2   CustomScriptExtension Succeeded
```

## <a name="remove-an-installed-extension"></a>删除已安装的扩展

若要在启用了 Arc 的服务器上删除已安装的[Remove-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/remove-azconnectedmachineextension) VM 扩展，请使用带有 `-Name` 、 `-MachineName` 和参数的 AzConnectedMachineExtension `-ResourceGroupName` 。

例如，若要删除适用于 Linux 的 Log Analytics VM 扩展，请运行以下命令：

```powershell
Remove-AzConnectedMachineExtension -MachineName myMachineName -ResourceGroupName myResourceGroup -Name OmsAgentforLinux
```

## <a name="next-steps"></a>后续步骤

- 你可以使用 [Azure CLI](manage-vm-extensions-cli.md)、 [Azure 门户](manage-vm-extensions-portal.md)或 [Azure 资源管理器模板](manage-vm-extensions-template.md)来部署、管理和删除 VM 扩展。

- 疑难解答信息可在 [VM 扩展疑难解答指南](troubleshoot-vm-extensions.md)中找到。
