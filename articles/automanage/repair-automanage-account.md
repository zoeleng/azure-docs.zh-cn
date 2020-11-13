---
title: 修复损坏的 Azure Automanage 帐户
description: 了解如何修复损坏的 Automanage 帐户
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: alsin
ms.openlocfilehash: ad54b37da8a4945162b507232f33083890ec1fff
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557569"
---
# <a name="repair-a-broken-automanage-account"></a>修复损坏的 Automanage 帐户
[Automanage 帐户](./automanage-virtual-machines.md#automanage-account)是安全上下文或用于执行自动操作的标识。 如果最近将包含 Automanage 帐户的订阅移动到新租户，则需要重新配置 Automanage 帐户。 若要重新配置你的 Automanage 帐户，你将需要重置标识类型并为该帐户分配适当的角色。

## <a name="step-1-reset-automanage-account-identity-type"></a>步骤1：重置 Automanage 帐户标识类型
请在下面的 "Azure 资源管理器 (ARM) " 模板中重置 Automanage 帐户标识类型。 将该文件保存在本地 `armdeploy.json` 或类似的。 记下你的 Automanage 帐户名称和位置，因为这些是 ARM 模板中的必需参数。

1. 使用下面的模板创建新的 ARM 部署，并使用 `identityType = None`
    * 你可以使用 Azure CLI 执行此操作 `az deployment sub create` 。 在此处了解有关 `az deployment sub` 命令[here](https://docs.microsoft.com/cli/azure/deployment/sub)的详细信息。
    * 您也可以使用该模块通过 PowerShell 执行此操作 `New-AzDeployment` 。 在此处了解有关 `New AzDeployment` 模块[here](https://docs.microsoft.com/powershell/module/az.resources/new-azdeployment)的详细信息。

1. 再次运行同一 ARM 模板 `identityType = SystemAssigned`

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "identityType": {
            "type": "string",
            "allowedValues": [ "None", "SystemAssigned" ]
        }
    },
    "resources": [
        {
            "apiVersion": "2020-06-30-preview",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Automanage/accounts",
            "identity": {
                "type": "[parameters('identityType')]"
            }
        }
    ]
}

```

## <a name="step-2-assign-appropriate-roles-for-the-automanage-account"></a>步骤2：为 Automanage 帐户分配适当的角色
Automanage 帐户需要订阅上包含 Automanage 所管理的 Vm 的参与者和资源策略参与者角色。 你可以使用 Azure 门户、ARM 模板或 Azure CLI 分配这些角色。

如果你使用的是 ARM 模板或 Azure CLI，则你将需要 "主体 ID" (（也称为 "对象 ID") 为你的 Automanage 帐户） (如果你使用 Azure 门户) ，则不需要这样做。 你可以使用以下方法查找 Automanage 帐户的主体 ID (对象 ID) ：

- [Azure CLI](https://docs.microsoft.com/cli/azure/ad/sp)：使用命令 `az ad sp list --display-name <name of your Automanage Account>` 。

- Azure 门户：导航到 **Azure Active Directory** 并按名称搜索你的 Automanage 帐户。 在 " **企业应用程序** " 下，选择显示的 Automanage 帐户名称。

### <a name="azure-portal"></a>Azure 门户
1. 在 " **订阅** " 下，导航到包含 Automanaged vm 的订阅。
1. 导航到 **(IAM) 的访问控制** 。
1. 单击 " **添加角色分配** "。
1. 选择 " **参与者** " 角色，然后键入 Automanage 帐户的名称。
1. 按“保存”。
1. 重复步骤3-5，这一次请与 **资源策略参与者** 角色一起工作。

### <a name="arm-template"></a>ARM 模板
运行以下 ARM 模板。 你将需要 Automanage 帐户的主体 ID-上述用于获取主体 ID 的步骤。 在出现提示时输入。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        }
    },
    "variables": {
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Resource Policy Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '36243c78-bf99-498c-9df9-86d9f8d28608')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Resource Policy Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Resource Policy Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

### <a name="azure-cli"></a>Azure CLI
运行以下命令：

```azurecli
az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Contributor" --scope /subscriptions/<your subscription id>

az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Resource Policy Contributor" --scope /subscriptions/<your subscription id>
```

## <a name="next-steps"></a>后续步骤
[在此处](./automanage-virtual-machines.md)了解有关 Azure Automanage 的详细信息。
