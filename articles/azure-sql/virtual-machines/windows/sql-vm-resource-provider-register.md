---
title: 安装 SQL Server IaaS 代理扩展
description: 向 SQL VM 资源提供程序注册 Azure SQL Server 虚拟机，以便为在 Azure Marketplace 外部部署的 SQL Server 虚拟机启用功能，并提供相容性和改进的可管理性。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 8099bf87410e8359894d8b40d7637451891ad664
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286133"
---
# <a name="install-sql-server-iaas-agent-extension"></a>安装 SQL Server IaaS 代理扩展
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

向 SQL VM 资源提供程序注册 SQL Server VM 会安装 [SQL Server IaaS 代理扩展](sql-server-iaas-agent-extension-automate-management.md)。 

本文介绍如何使用 SQL VM 资源提供程序注册单个 SQL Server VM。 或者，你可以 [自动](sql-vm-resource-provider-automatic-registration.md) 注册所有 SQL Server vm 或 [批量](sql-vm-resource-provider-bulk-register.md)注册。


## <a name="overview"></a>概述

向资源提供程序注册会安装 [SQL Server IaaS 代理扩展](sql-server-iaas-agent-extension-automate-management.md)，还会在你的订阅中创建 **SQL 虚拟机**_资源_ ，该资源与虚拟机资源是 _单独_ 的资源。 从资源提供程序中取消注册 SQL Server VM 会删除 SQL 虚拟机资源，但不会删除实际虚拟机。

通过 Azure 门户部署 SQL Server VM Azure 市场映像会自动将 SQL Server VM 注册到资源提供程序。 但是，如果选择在 Azure 虚拟机上自行安装 SQL Server，或通过自定义 VHD 预配 Azure 虚拟机，则必须向 SQL VM 资源提供程序注册 SQL Server VM，才能安装 SQL IaaS 代理扩展。 

若要利用 SQL VM 资源提供程序，必须先向 [资源提供程序注册订阅](#register-subscription-with-rp)，这使资源提供程序能够在特定订阅中创建资源。

> [!IMPORTANT]
> SQL IaaS 代理扩展收集数据，以便在 Azure 虚拟机中使用 SQL Server 时，为客户提供可选的权益。 在未经客户的事先同意的情况下，Microsoft 不会将此数据用于许可审核。 有关详细信息，请参阅 [SQL Server 隐私补充](/sql/sql-server/sql-server-privacy#non-personal-data) 。

## <a name="prerequisites"></a>先决条件

若要将 SQL Server VM 注册到资源提供程序，需要： 

- 一个 [Azure 订阅](https://azure.microsoft.com/free/)。
- Azure 资源模型 [Windows Server 2008 (或更高版本) ](../../../virtual-machines/windows/quick-create-portal.md) 具有 [SQL Server 2008 (或更高) 版本 ](https://www.microsoft.com/sql-server/sql-server-downloads) 的虚拟机部署到公共或 Azure 政府云。 
- 最新版本的 [Azure CLI](/cli/azure/install-azure-cli) 或 [Azure PowerShell)  (5.0 ](/powershell/azure/install-az-ps)。 


## <a name="register-subscription-with-rp"></a>将订阅注册到 RP

若要将 SQL Server VM 注册到 SQL VM 资源提供程序，必须首先将订阅注册到资源提供程序。 这使 SQL VM 资源提供程序能够在订阅中创建资源。  为此，可以使用 Azure 门户、Azure CLI 或 Azure PowerShell。

### <a name="azure-portal"></a>Azure 门户

1. 打开 Azure 门户，转到“所有服务”。 
1. 转到“订阅”，选择感兴趣的订阅。  
1. 在“订阅”页中，转到“资源提供程序” 。 
1. 在筛选器中输入“sql”，以便显示与 SQL 相关的资源提供程序。 
1. 根据所需操作为“Microsoft.SqlVirtualMachine”提供程序选择“注册”、“重新注册”或“取消注册”   。 

   ![修改提供程序](./media/sql-vm-resource-provider-register/select-resource-provider-sql.png)


### <a name="command-line"></a>命令行

使用 Azure CLI 或 Azure PowerShell 将 SQL VM 资源提供程序注册到你的 Azure 订阅。 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

```azurecli-interactive
# Register the SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

```powershell-interactive
# Register the SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

---

## <a name="register-with-rp"></a>向 RP 注册

对于 [SQL Server IaaS 代理扩展](sql-server-iaas-agent-extension-automate-management.md#management-modes)，有三种管理模式。 

在完全管理模式下安装扩展会重新启动 SQL Server 服务，因此建议首先在轻型模式下安装扩展，然后在维护时段内 [升级到完整](#upgrade-to-full) 扩展。 

### <a name="lightweight-management-mode"></a>轻型管理模式

使用 Azure CLI 或 Azure PowerShell 将 SQL Server VM 注册到资源提供程序，并在轻型模式下安装 SQL IaaS 扩展。 这不会重新启动 SQL Server 服务。 随后可以随时升级到完整模式，但这样做会重启 SQL Server 服务，因此建议等到计划性维护时段。 

提供 SQL Server 许可证类型，形式为即用即付（`PAYG`，用于按使用情况付费）、Azure 混合权益（`AHUB`，用于使用自己的许可证）或灾难恢复（`DR`，用于激活[免费 DR 副本许可证](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure)）。

故障转移群集实例和多实例部署只能在轻型模式下的 SQL VM 资源提供程序中注册。 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

使用 Azure CLI 在轻型模式下注册 SQL Server VM： 

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode
  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```


# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

使用 Azure PowerShell 在轻型模式下注册 SQL Server VM：  


  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  # Register SQL VM with 'Lightweight' SQL IaaS agent
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType LightWeight  
  ```

---

### <a name="full-management-mode"></a>完整管理模式

以完整模式注册 SQL Server VM 将重新启动 SQL Server 服务。 请谨慎操作。 

若要以完全模式直接注册 SQL Server VM (并可能重新启动 SQL Server 服务) ，请使用以下 Azure PowerShell 命令： 

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

### <a name="noagent-management-mode"></a>无代理管理模式 

在 Windows Server 2008（非 R2）上安装的 SQL Server 2008 和 2008 R2 可以在[无代理模式](sql-server-iaas-agent-extension-automate-management.md#management-modes)下注册到 SQL VM 资源提供程序。 此选项可确保符合性，并允许使用有限功能在 Azure 门户中监视 SQL Server VM。

将 sqlLicenseType 指定为 `AHUB`、`PAYG` 或 `DR`，并将 sqlImageOffer 指定为 `SQL2008-WS2008` 或 `SQL2008R2-WS2008` 。 

若要在 Windows Server 2008 实例上注册 SQL Server 2008 或 2008 R2，请使用以下 Azure CLI 或 Azure PowerShell 代码片段： 


# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

在 NoAgent 模式下向 Azure CLI 注册 SQL Server 2008 虚拟机： 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008
 ```
 
 
在 NoAgent 模式下向 Azure CLI 注册 SQL Server 2008 R2 虚拟机： 

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008R2-WS2008
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

在 NoAgent 模式下向 Azure PowerShell 注册 SQL Server 2008 虚拟机： 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008-WS2008
  ```
  

在 NoAgent 模式下向 Azure PowerShell 注册 SQL Server 2008 R2 虚拟机： 

  ```powershell-interactive
  # Get the existing compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
  New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
    -LicenseType PAYG -SqlManagementType NoAgent -Sku Standard -Offer SQL2008R2-WS2008
  ```

---

## <a name="verify-mode"></a>验证模式

可以通过使用 Azure PowerShell 查看 SQL Server IaaS 代理的当前模式： 

```powershell-interactive
# Get the SqlVirtualMachine
$sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
$sqlvm.SqlManagementType
```

## <a name="upgrade-to-full"></a>升级到完整  

如果 SQL Server 安装了 *轻型* IaaS 扩展的 vm，则可以使用 Azure 门户、Azure CLI 或 Azure PowerShell 将模式升级到 _完整_ 模式。 在 OS 升级到 Windows 2008 R2 及更高版本之后，无代理模式下的 SQL Server VM 可以升级到完整模式 。 无法进行降级 - 若要执行此操作，需要 从 SQL VM 资源提供程序[取消注册](#unregister-from-rp) SQL Server VM。 这样做会删除 SQL 虚拟机资源，但不会删除实际虚拟机。 


### <a name="azure-portal"></a>Azure 门户

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 转到 [SQL 虚拟机](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource)资源。 
1. 选择 SQL Server VM，然后选择 " **概述** "。 
1. 对于具有无代理或轻型 IaaS 模式的 SQL Server VM，请选择“SQL IaaS 扩展仅提供许可证类型和版本更新”消息。

   ![用于从门户更改模式的选项](./media/sql-vm-resource-provider-register/change-sql-iaas-mode-portal.png)

1. 选中“我同意重启虚拟机上的 SQL Server 服务”复选框，然后选择“确认”以将 IaaS 模式升级到完整模式 。 

    ![用于同意重启虚拟机上的 SQL Server 服务的复选框](./media/sql-vm-resource-provider-register/enable-full-mode-iaas.png)

### <a name="command-line"></a>命令行

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)

运行以下 Azure CLI 代码片段：

  ```azurecli-interactive
  # Update to full mode
  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

运行以下 Azure PowerShell 代码片段：

  ```powershell-interactive
  # Get the existing  Compute VM
  $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
  # Register with SQL VM resource provider in full mode
  Update-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -SqlManagementType Full
  ```

---

## <a name="verify-registration-status"></a>验证注册状态
可以通过使用 Azure 门户、Azure CLI 或 Azure PowerShell 来验证是否已向 SQL VM 资源提供程序注册了 SQL Server VM。 

### <a name="azure-portal"></a>Azure 门户 

1. 登录到 [Azure 门户](https://portal.azure.com)。 
1. 中转到 [SQL Server vm](manage-sql-vm-portal.md)。
1. 从列表中选择 SQL Server VM。 如果 SQL Server VM 未在此处列出，则可能尚未注册到 SQL VM 资源提供程序。 
1. 查看“状态”下的值。 如果“状态”为“成功”，则 SQL Server VM 已成功注册到 SQL VM 资源提供程序 。 

   ![通过 SQL RP 注册验证状态](./media/sql-vm-resource-provider-register/verify-registration-status.png)

### <a name="command-line"></a>命令行

使用 Azure CLI 或 Azure PowerShell 验证当前 SQL Server VM 注册状态。 如果注册成功，`ProvisioningState` 将显示 `Succeeded`。 

# <a name="azure-cli"></a>[Azure CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzSqlVM -Name <vm_name> -ResourceGroupName <resource_group>
  ```

---

错误表明 SQL Server VM 尚未注册到资源提供程序。 


## <a name="unregister-from-rp"></a>从 RP 注销

若要使用 SQL VM 资源提供程序取消注册 SQL Server VM，请使用 Azure 门户或 Azure CLI 删除 SQL 虚拟机 *资源* 。 删除 SQL 虚拟机 *资源* 不会删除 SQL Server VM。 但是，请小心谨慎并仔细执行以下步骤，因为在尝试删除资源时，可能会意外删除虚拟机。 

需要在 SQL VM 资源提供程序中注销 SQL 虚拟机，以使管理模式完全降级。 

### <a name="azure-portal"></a>Azure 门户

若要使用 Azure 门户从资源提供程序取消注册 SQL Server VM，请执行以下步骤：

1. 登录到 [Azure 门户](https://portal.azure.com)。
1. 导航到 SQL VM 资源。 
  
   ![SQL 虚拟机资源](./media/sql-vm-resource-provider-register/sql-vm-manage.png)

1. 选择“删除”。 

   ![在顶部导航栏中选择 "删除"](./media/sql-vm-resource-provider-register/delete-sql-vm-resource-provider.png)

1. 键入 SQL 虚拟机的名称，并 **清除该虚拟机旁边的复选框** 。

   ![取消选中 VM 以阻止删除实际虚拟机，然后选择 "删除" 以继续删除 SQL VM 资源](./media/sql-vm-resource-provider-register/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > 如果未能清除虚拟机名称的复选框，会导致彻底删除虚拟机。 清除该复选框可从资源提供程序取消注册 SQL Server VM，但不会删除实际虚拟机。 

1. 选择 " **删除** " 以确认删除 SQL 虚拟机 *资源* ，而不是 SQL Server VM。 

### <a name="command-line"></a>命令行

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
若要使用 Azure CLI 从资源提供程序中注销 SQL Server VM，请使用 [az SQL VM delete](/cli/azure/sql/vm?view=azure-cli-latest&preserve-view=true#az-sql-vm-delete) 命令。 这会删除 SQL Server VM *资源* ，但不会删除虚拟机。 


```azurecli-interactive
az sql vm delete 
  --name <SQL VM resource name> |
  --resource-group <Resource group name> |
  --yes 
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
若要使用 Azure PowerShell 从资源提供程序中注销 SQL Server VM，请使用 [AzSqlVM](/powershell/module/az.sqlvirtualmachine/remove-azsqlvm)命令。 这会删除 SQL Server VM *资源* ，但不会删除虚拟机。 

```powershell-interactive
Remove-AzSqlVM -ResourceGroupName <resource_group_name> -Name <VM_name>
```

---



## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下文章： 

* [Windows VM 上的 SQL Server 概述](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 上的 SQL Server 常见问题解答](frequently-asked-questions-faq.md)  
* [Windows VM 上的 SQL Server 定价指南](pricing-guidance.md)
* [Windows VM 上的 SQL Server 发行说明](../../database/doc-changes-updates-release-notes.md)
