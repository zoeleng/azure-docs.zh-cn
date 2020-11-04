---
title: 使用 Azure CLI 启用 VM 扩展
description: 本文介绍如何使用 Azure CLI 将虚拟机扩展部署到在混合云环境中运行的支持 Azure Arc 的服务器。
ms.date: 10/19/2020
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.openlocfilehash: 99504c86046c1ef34eeab500a703b9a028cb46fb
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93336720"
---
# <a name="enable-azure-vm-extensions-using-the-azure-cli"></a>使用 Azure CLI 启用 Azure VM 扩展

本文介绍如何使用 Azure CLI 将启用了 Azure Arc 的服务器支持的 Azure VM 扩展部署到 Linux 或 Windows 混合计算机并将其卸载。

[!INCLUDE [Azure CLI Prepare your environment](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="enable-extension"></a>启用扩展

若要在启用了 Arc 的服务器上启用 VM 扩展，请将 [az connectedmachine machine extension create](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_create) 与 `--machine-name` 、 `--extension-name` 、、 `--location` `--type` 、 `settings` 和 `--publisher` 参数一起使用。

以下示例在启用了 Arc 的 Linux 服务器上启用 Log Analytics VM 扩展：

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "OmsAgentforLinux" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.EnterpriseCloud.Monitoring" --settings "{\"workspaceId\":\"workspaceId"}" --protected-settings "{\workspaceKey\":"\workspaceKey"} --type-handler-version "1.10" --resource-group "myResourceGroup"
```

下面的示例在启用 Arc 的服务器上启用自定义脚本扩展：

```azurecli
az connectedmachine machine-extension create --machine-name "myMachineName" --name "CustomScriptExtension" --location "eastus" --type "CustomScriptExtension" --publisher "Microsoft.Compute" --settings "{\"commandToExecute\":\"powershell.exe -c \\\"Get-Process | Where-Object { $_.CPU -gt 10000 }\\\"\"}" --type-handler-version "1.10" --resource-group "myResourceGroup"
```

## <a name="list-extensions-installed"></a>列出已安装的扩展

若要获取启用了 Arc 的服务器上的 VM 扩展列表，请使用 [az connectedmachine machine extension list](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_list) ， `--machine-name` 并使用和 `--resource-group` 参数。

例如：

```azurecli
az connectedmachine machine-extension list --machine-name "myMachineName" --resource-group "myResourceGroup"
```

默认情况下，Azure CLI 命令的输出采用 JSON（JavaScript 对象表示法）格式。 若要更改列表或表的默认输出，请使用 [az configure --output](/cli/azure/reference-index)。 还可以向任何命令添加 `--output` 来对输出格式进行一次性更改。

下面的示例显示了命令中的部分 JSON 输出 `az connectedmachine machine-extension -list` ：

```json
[
  {
    "autoUpgradingMinorVersion": "false",
    "forceUpdateTag": null,
    "id": "/subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.HybridCompute/machines/SVR01/extensions/DependencyAgentWindows",
    "location": "eastus",
    "name": "DependencyAgentWindows",
    "namePropertiesInstanceViewName": "DependencyAgentWindows",
```

## <a name="remove-an-installed-extension"></a>删除已安装的扩展

若要在启用了 Arc 的服务器上删除已安装的 VM 扩展，请将 [az connectedmachine 计算机扩展删除](/cli/azure/ext/connectedmachine/connectedmachine/machine-extension#ext_connectedmachine_az_connectedmachine_machine_extension_delete) 与 `--extension-name` 、 `--machine-name` 和参数一起使用 `--resource-group` 。

例如，若要删除适用于 Linux 的 Log Analytics VM 扩展，请运行以下命令：

```azurecli
az connectedmachine machine-extension delete --machine-name "myMachineName" --name "OmsAgentforLinux" --resource-group "myResourceGroup"
```

## <a name="next-steps"></a>后续步骤

- 你可以使用 [PowerShell](manage-vm-extensions-powershell.md)、 [Azure 门户](manage-vm-extensions-portal.md)或 [Azure 资源管理器模板](manage-vm-extensions-template.md)来部署、管理和删除 VM 扩展。

- 疑难解答信息可在 [VM 扩展疑难解答指南](troubleshoot-vm-extensions.md)中找到。
