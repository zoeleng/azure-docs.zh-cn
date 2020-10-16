---
title: 将网络观察程序扩展更新到最新版本
description: 了解如何将 Azure 网络观察程序扩展更新到最新版本。
services: virtual-machines-windows
documentationcenter: ''
author: damendo
manager: balar
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/23/2020
ms.author: damendo
ms.openlocfilehash: e367c348364d03cec6914c99e7ff112803fc58f6
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132425"
---
# <a name="update-the-network-watcher-extension-to-the-latest-version"></a>将网络观察程序扩展更新到最新版本

## <a name="overview"></a>概述

[Azure 网络观察](../../network-watcher/network-watcher-monitoring-overview.md) 程序是监视 Azure 网络的网络性能监视、诊断和分析服务。 网络观察程序代理虚拟机 (VM) 扩展是按需捕获网络流量以及使用 Azure Vm 上的其他高级功能的要求。 网络观察程序扩展由连接监视器、连接监视器 (预览) 、连接疑难解答和数据包捕获等功能使用。

## <a name="prerequisites"></a>必备条件

本文假设你在 VM 中安装了网络观察程序扩展。

## <a name="latest-version"></a>最新版本

最新版本的网络观察程序扩展目前正在进行 `1.4.1654.1` 。

## <a name="update-your-extension"></a>更新扩展

若要更新扩展，需要知道扩展版本。

### <a name="check-your-extension-version"></a>检查扩展版本

你可以使用 Azure 门户、Azure CLI 或 PowerShell 检查你的扩展版本。

#### <a name="usetheazureportal"></a>使用 Azure 门户

1. 在 Azure 门户中转到 VM 的 " **扩展** " 窗格。
1. 选择 **AzureNetworkWatcher** 扩展以查看详细信息窗格。
1. 在 " **版本** " 字段中找到版本号。  

#### <a name="use-the-azure-cli"></a>使用 Azure CLI

在 Azure CLI 提示符下运行以下命令：

```azurecli
az vm extension list --resource-group  <ResourceGroupName> --vm-name <VMName>
```

在输出中找到 AzureNetworkWatcher 扩展。 标识输出的 "TypeHandlerVersion" 字段中的版本号。  

#### <a name="usepowershell"></a>使用 PowerShell

在 PowerShell 提示符下运行以下命令：

```powershell
Get-AzVMExtension -ResourceGroupName <ResourceGroupName> -VMName <VMName>  
```

在输出中找到 AzureNetworkWatcher 扩展。 标识输出的 "TypeHandlerVersion" 字段中的版本号。

### <a name="update-your-extension"></a>更新扩展

如果你的版本早于 `1.4.1654.1` （当前最新版本），请使用以下任一选项更新你的扩展。

#### <a name="option-1-use-powershell"></a>选项1：使用 PowerShell

运行以下命令：

```powershell
#Linux command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"   

#Windows command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"   
```

#### <a name="option-2-use-the-azure-cli"></a>选项2：使用 Azure CLI

强制执行升级。

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

如果这不起作用，请删除并重新安装该扩展，并按照以下步骤自动添加最新版本。

删除扩展。

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

再次安装该扩展。

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"  

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" 

```

#### <a name="option-3-reboot-your-vms"></a>选项3：重新启动 Vm

如果为网络观察程序扩展将自动升级设置为 true，请将 VM 安装重新启动到最新扩展。

## <a name="support"></a>支持

如果在本文的任何位置需要更多帮助，请参阅 [Linux](./network-watcher-linux.md) 或 [Windows](./network-watcher-windows.md)的网络观察程序扩展文档。 你还可以联系 MSDN Azure 上的 Azure 专家 [并 Stack Overflow 论坛](https://azure.microsoft.com/support/forums/)。 或者，提交 Azure 支持事件。 转到 [Azure 支持站点](https://azure.microsoft.com/support/options/)并选择 " **获取支持**"。 有关使用 Azure 支持的信息，请阅读 [Microsoft Azure 支持常见问题解答](https://azure.microsoft.com/support/faq/)。