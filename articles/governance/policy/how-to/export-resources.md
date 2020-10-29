---
title: 导出 Azure Policy 资源
description: 了解如何将 Azure 策略资源导出到 GitHub，如策略定义和策略分配。
ms.date: 10/29/2020
ms.topic: how-to
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: c16ceed755cab3228b8f9e401f486a0629f3a60d
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "93025708"
---
# <a name="export-azure-policy-resources"></a>导出 Azure Policy 资源

本文提供了有关如何导出现有 Azure 策略资源的信息。 导出资源会很有用，并建议用于备份，但在使用云管理和将 [策略视为代码](../concepts/policy-as-code.md)时，这也是一项重要的一步。 Azure 策略资源可以通过 [Azure 门户](#export-with-azure-portal)、 [Azure CLI](#export-with-azure-cli)、 [Azure PowerShell](#export-with-azure-powershell)和每个受支持的 sdk 进行导出。

## <a name="export-with-azure-portal"></a>导出 Azure 门户

要从 Azure 门户导出策略定义，请按照以下步骤操作：

1. 在 Azure 门户中单击“所有服务”，然后搜索并选择“策略”，启动 Azure Policy 服务。 

1. 选择“Azure Policy”页左侧的“定义”。

1. 使用“导出定义”按钮或选择策略定义行中的省略号，然后选择“导出定义” 。

1. 选择“使用 GitHub 登录”按钮。 如果尚未通过 GitHub 进行身份验证并授权 Azure Policy 导出资源，请在打开的新窗口中查看 [GitHub 操作](https://github.com/features/actions)所需的访问权限，然后选择“授权 AzureGitHubActions”以继续执行导出流程。 完成后，新窗口会自行关闭。

1. 在“基本信息”选项卡上，设置以下选项，然后选择“策略”选项卡或页面底部的“下一步:  策略”按钮。

   - **存储库筛选器** ：设置为“我的存储库”以仅查看自己拥有的存储库，或设置为“所有存储库”以查看为 GitHub 操作授予了访问权限的所有存储库 。
   - **存储库** ：设置为要将 Azure Policy 资源导出到的存储库。
   - **分支** ：在存储库中设置分支。 在将更新进一步合并到源代码中之前，非常适合使用非默认分支来验证更新。
   - **目录** ：要将 Azure Policy 资源导出到的根级别文件夹。 此目录下的子文件夹基于导出的资源而创建。

1. 在“策略”选项卡上，选择省略号，然后选择管理组、订阅或资源组的组合，以设置搜索范围。
   
1. 使用“添加策略定义”按钮在相关范围中搜索要导出的对象。 在打开的侧窗口中，选择要导出的每个对象。 根据搜索框或类型筛选选定内容。 选择所有要导出的对象后，使用页面底部的“添加”按钮。

1. 对于每个选定对象，为策略定义选择所需的导出选项，例如“仅定义”或“定义和分配” 。 然后选择“查看 + 导出”选项卡或页面底部的“下一步: 查看 + 导出”按钮。

   > [!NOTE]
   > 如果选择“定义和分配”选项，则仅导出添加策略定义时通过筛选器设置的范围内的策略分配。

1. 在“查看 + 导出”选项卡上，检查详细信息是否匹配，然后使用页面底部的“导出”按钮 。

1. 检查 GitHub 存储库、分支和根级别文件夹，查看所选资源现是否已导出到源代码管理。

将 Azure Policy 资源导出到所选 GitHub 存储库和根级别文件夹内的以下结构中：

```text
|
|- <root level folder>/  ________________ # Root level folder set by Directory property
|  |- policies/  ________________________ # Subfolder for policy objects
|     |- <displayName>_<name>____________ # Subfolder based on policy displayName and name properties
|        |- policy.json _________________ # Policy definition
|        |- assign.<displayName>_<name>__ # Each assignment (if selected) based on displayName and name properties
|
```

## <a name="export-with-azure-cli"></a>导出 Azure CLI

每个 Azure 策略定义、计划和分配都可以 [Azure CLI](/cli/azure/install-azure-cli)导出为 JSON。 其中每个命令都使用 **name** 参数来指定要为其获取 JSON 的对象。 **Name** 属性通常是 _GUID_ ，而不是对象的 **displayName** 。

- 定义- [az policy definition show](/cli/azure/policy/definition#az-policy-definition-show)
- 计划- [az policy set-definition show](/cli/azure/policy/set-definition#az-policy-set-definition-show)
- 分配- [az policy 赋值 show](/cli/azure/policy/assignment#az-policy-assignment-show)

下面是一个示例，说明如何获取 **名** 为 _VirtualMachineStorage_ 的策略定义的 JSON：

```azurecli-interactive
az policy definition show --name 'VirtualMachineStorage'
```

## <a name="export-with-azure-powershell"></a>导出 Azure PowerShell

每个 Azure 策略定义、计划和分配都可以 [Azure PowerShell](/powershell/azure/)导出为 JSON。 其中每个 cmdlet 都使用 **Name** 参数来指定要为其获取 JSON 的对象。 **Name** 属性通常是 _GUID_ ，而不是对象的 **displayName** 。

- 定义- [AzPolicyDefinition](/powershell/module/az.resources/get-azpolicydefinition)
- 计划- [AzPolicySetDefinition](/powershell/module/az.resources/get-azpolicysetdefinition)
- 分配- [AzPolicyAssignment](/powershell/module/az.resources/get-azpolicyassignment)

下面是一个示例，说明如何获取 **名** 为 _VirtualMachineStorage_ 的策略定义的 JSON：

```azurepowershell-interactive
Get-AzPolicyDefinition -Name 'VirtualMachineStorage' | ConvertTo-Json -Depth 10
```

## <a name="next-steps"></a>后续步骤

- 在 [Azure Policy 示例](../samples/index.md)中查看示例。
- 查看 [Azure Policy 定义结构](../concepts/definition-structure.md)。
- 查看[了解策略效果](../concepts/effects.md)。
- 了解如何[以编程方式创建策略](programmatically-create.md)。
- 了解如何[修正不符合的资源](remediate-resources.md)。
- 参阅[使用 Azure 管理组来组织资源](../../management-groups/overview.md)，了解什么是管理组。
