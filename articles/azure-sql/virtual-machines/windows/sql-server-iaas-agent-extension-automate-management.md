---
title: 什么是 SQL Server IaaS 代理扩展？
description: 本文介绍 SQL Server IaaS 代理扩展如何帮助在 Azure Vm 上自动执行 SQL Server 管理特定管理任务。 其中包括自动备份、自动修补、Azure Key Vault 集成、授权管理、存储配置和所有 SQL Server VM 实例的集中管理等功能。
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 070058eae36bf4f8546cfcf4beb85ac5023e9c79
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286172"
---
# <a name="what-is-the-sql-server-iaas-agent-extension"></a>什么是 SQL Server IaaS 代理扩展？
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


SQL Server IaaS 代理扩展 (SqlIaasExtension) 在 Azure 虚拟机上的 SQL Server 上运行 (Vm) 来自动执行管理任务和管理任务。 

本文提供扩展的概述。 若要在 Azure Vm 上安装 SQL Server IaaS 扩展以 SQL Server，请批量查看 [自动安装](sql-vm-resource-provider-automatic-registration.md)、 [单个 vm](sql-vm-resource-provider-register.md)或 [vm](sql-vm-resource-provider-bulk-register.md)的文章。 

## <a name="overview"></a>概述

SQL Server IaaS 代理扩展为 Azure Vm 上的 SQL Server 提供了很多好处： 

- **功能优势** ：扩展功能使你能够更好地利用一些自动化功能权益，例如门户管理、许可证灵活性、自动备份、自动修补等。 有关详细信息，请参阅本文后面的 [功能权益](#feature-benefits) 。 

- **符合性** ：该扩展提供了一种简化的方法，使您能够在产品条款中指定 Azure 混合权益已启用。 此过程无需为每个资源管理许可注册表单。  

- **免费** ：所有三个可管理性模式中的扩展都是完全免费的。 资源提供程序或更改管理模式不会产生相关联的额外成本。 

- **简化的许可证管理** ：扩展可简化 SQL Server 许可证管理，并使你能够使用 [Azure 门户](manage-sql-vm-portal.md)、Azure CLI 或 PowerShell，使用 Azure 混合权益来快速确定 SQL Server vm： 

   # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---


> [!IMPORTANT]
> SQL IaaS 代理扩展收集数据，以便在 Azure 虚拟机中使用 SQL Server 时，为客户提供可选的权益。 在未经客户的事先同意的情况下，Microsoft 不会将此数据用于许可审核。 有关详细信息，请参阅 [SQL Server 隐私补充](/sql/sql-server/sql-server-privacy#non-personal-data) 。


## <a name="feature-benefits"></a>功能优势 

SQL Server IaaS 代理扩展将为管理 SQL Server VM 提供多种功能优势。 

下表详细说明了这些优点： 


| Feature | 说明 |
| --- | --- |
| **门户管理** | 在 [门户](manage-sql-vm-portal.md)中取消锁定管理，以便你可以在一个位置查看所有 SQL Server vm，以便可以直接从门户中启用和禁用 SQL 特定功能。 
| **自动备份** |对 VM 中的 SQL Server 默认实例或已[正确安装](frequently-asked-questions-faq.md#administration)的命名实例自动执行所有数据库的备份计划。 有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的自动备份 (Resource Manager)](automated-backup-sql-2014.md)。 |
| **自动修补** |配置维护时段，在这种情况下，可能会发生重要的 Windows 和 SQL Server VM 的安全更新，因此可以避免在工作负荷高峰时间进行更新。 有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的自动修补 (Resource Manager)](automated-patching.md)。 |
| **Azure 密钥保管库集成** |可让你在 SQL Server VM 上自动安装和配置 Azure 密钥保管库。 有关详细信息，请参阅 [为 Azure 虚拟机上的 SQL Server 配置 Azure 密钥保管库集成 (Resource Manager)](azure-key-vault-integration-configure.md)。 |
| **灵活的许可** | [无缝地](licensing-model-azure-hybrid-benefit-ahb-change.md)从自带许可证 (（也称为 Azure 混合权益) ）过渡到 "即用即付" 许可模式，并再次返回。 | 
| **灵活版本/版本** | 如果决定更改[SQL Server 的](change-sql-server-edition.md)[版本](change-sql-server-version.md)，则可以更新 Azure 门户中的元数据，而无需重新部署整个 SQL Server VM。  | 


## <a name="management-modes"></a>管理模式

SQL IaaS 扩展包含三种管理模式：

- “轻型”模式不需要重启 SQL Server，但仅支持更改 SQL Server 的许可证类型和版本。 对于具有多个实例或参与故障转移群集实例 (FCI) 的 SQL Server VM，请使用此选项。 此管理模式在计算机上保留 SQL IaaS 代理扩展二进制文件，但不安装代理。 当使用 [自动注册](sql-vm-resource-provider-automatic-registration.md) 功能时，或者当手动注册期间未指定管理类型时，轻型模式是默认的管理模式。 使用轻型模式时，不会对内存或 CPU 产生任何影响，并且没有关联成本。 建议先在轻型模式下注册 SQL Server VM，然后在计划性维护时段内升级到完整模式。

- 完整模式提供所有功能，但需要 SQL Server 重启和系统管理员权限。 使用它管理具有单个实例的 SQL Server VM。 完整模式安装两个 Windows 服务，它们对内存和 CPU 的影响最小 - 可通过任务管理器进行监视。 使用完整可管理性模式时没有关联成本。 

- 无代理模式专用于在 Windows Server 2008 上安装的 SQL Server 2008 和 SQL Server 2008 R2。 使用无代理模式时，不会对内存或 CPU 产生任何影响。 使用无代理可管理性模式时没有关联成本。 

可以通过使用 Azure PowerShell 查看 SQL Server IaaS 代理的当前模式： 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="installation"></a>安装

将 SQL Server Vm 注册到 SQL VM 资源提供程序时，会安装 SQL Server IaaS 代理扩展。 向资源提供程序注册会在你的订阅中创建 **SQL 虚拟机**_资源_ ，该资源与虚拟机资源是 _单独_ 的资源。 从资源提供程序中取消注册 SQL Server VM 会删除 SQL 虚拟机资源，但不会删除实际虚拟机。

通过 Azure 门户部署 SQL Server VM Azure 市场映像会自动将 SQL Server VM 注册到资源提供程序。 但是，如果选择在 Azure 虚拟机上自行安装 SQL Server，或通过自定义 VHD 预配 Azure 虚拟机，则必须向 SQL VM 资源提供程序注册 SQL Server VM，才能安装 SQL IaaS 代理扩展。 

若要安装该扩展，请向资源提供程序注册 SQL Server VM：
- [对于订阅中的所有当前和未来 Vm 自动执行](sql-vm-resource-provider-automatic-registration.md)
- [对于单个 VM](sql-vm-resource-provider-register.md)
- [大容量多个 Vm](sql-vm-resource-provider-bulk-register.md)

### <a name="named-instance-support"></a>命名实例支持

如果是虚拟机上唯一可用的 SQL Server 实例，则 SQL Server IaaS 扩展将与 SQL Server 的命名实例一起使用。 该扩展将无法安装在具有多个 SQL Server 实例的 Vm 上。 

若要使用 SQL Server 的命名实例，请部署 Azure 虚拟机，将名为的单个 SQL Server 实例安装到该虚拟机，然后将其注册到 [SQL VM 资源提供程序](sql-vm-resource-provider-register.md) 以安装扩展。

或者，若要将命名实例用于 Azure Marketplace SQL Server 映像，请执行以下步骤： 

   1. 从 Azure 市场部署 SQL Server VM。 
   1. 从 SQL VM 资源提供程序中[注销](sql-vm-resource-provider-register.md#unregister-from-rp)SQL Server VM。 
   1. 完全卸载 SQL Server VM 中的 SQL Server。
   1. 使用 SQL Server VM 中的命名实例安装 SQL Server。 
   1. 通过 [向 SQL VM 资源提供程序注册 SQL Server VM](sql-vm-resource-provider-register.md#register-with-rp)来安装 Sql IaaS 代理扩展。 

## <a name="verify-status-of-extension"></a>验证扩展状态

使用 Azure 门户或 Azure PowerShell 检查扩展的状态。 


### <a name="azure-portal"></a>Azure 门户

验证是否已在 Azure 门户中安装了该扩展。 

在 "虚拟机" 窗格中选择 " **所有设置** "，然后选择 " **扩展** "。 应看到列出“SqlIaasExtension”扩展。

![Azure 门户中 SQL Server IaaS 代理扩展的状态](./media/sql-server-iaas-agent-extension-automate-management/azure-rm-sql-server-iaas-agent-portal.png)


### <a name="azure-powershell"></a>Azure PowerShell

也可以使用 **Get-AzVMSqlServerExtension** Azure PowerShell cmdlet：

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

上一个命令确认已安装代理并提供常规状态信息。 还可使用以下命令获取有关自动备份和修补的特定状态信息：

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```


## <a name="limitations"></a>限制

SQL IaaS 代理扩展仅支持： 

- 通过 Azure 资源管理器部署的 SQL Server VM。 不支持通过经典模型部署的 SQL Server VM。 
- 部署到公有云或 Azure 政府云的 SQL Server VM。 不支持部署到其他私有云或政府云。 


## <a name="frequently-asked-questions"></a>常见问题 

是否应从 Azure 市场中的 SQL Server 映像预配的 SQL Server VM？

不是。 Microsoft 会自动注册从 Azure 市场中的 SQL Server 映像预配的 VM。 仅当 VM 不是从 Azure 市场中的 SQL Server 映像预配，并且 SQL Server 是自行安装时，才需要注册到 SQL VM 资源提供程序注册。

**SQL VM 资源提供程序是否适用于所有客户？** 

是的。 如果客户不使用 Azure 市场中的 SQL Server 映像，而是自行安装的 SQL Server，或者如果他们提供自定义 VHD，则应该将其 SQL Server VM 注册到 SQL VM 资源提供程序。 所有类型的订阅（直接、企业协议和云解决方案提供商）拥有的 VM 都可以注册到 SQL VM 资源提供程序。

注册到 SQL VM 资源提供程序时，默认管理模式是什么？

向 SQL VM 资源提供程序注册时的默认管理模式为 *轻型* 。 如果在向 SQL VM 资源提供程序注册时未设置 "SQL Server 管理" 属性，则该模式将设置为轻型，你的 SQL Server 服务将不会重新启动。 建议先在轻型模式下注册到 SQL VM 资源提供程序，然后在维护时段内升级到完整模式。 同样，使用 [自动注册功能](sql-vm-resource-provider-automatic-registration.md)时，默认管理也是轻型。

注册到 SQL VM 资源提供程序的先决条件是什么？

除了在虚拟机上安装 SQL Server 以外，不需要向 SQL VM 资源提供程序注册任何先决条件。 请注意，如果在完整模式下安装 SQL IaaS 代理扩展，则 SQL Server 服务将重新启动，因此建议在维护时段执行此操作。

注册到 SQL VM 资源提供程序是否会在我的 VM 上安装代理？

是，使用完全可管理性模式向 SQL VM 资源提供程序注册会将代理安装到 VM。 以轻型或 NoAgent 模式注册不会。 

在轻型模式下向 SQL VM 资源提供程序注册只会将 SQL IaaS 代理扩展 *二进制文件* 复制到 VM，而不会安装代理。 然后，将使用这些二进制文件在管理模式升级到完整版时安装代理。


**是否向 VM 上的 SQL VM 资源提供程序重启 SQL Server 注册？**

这取决于在注册期间指定的模式。 如果指定了轻型模式或 NoAgent 模式，则 SQL Server 服务将不会重新启动。 但是，将管理模式指定为 full 将导致 SQL Server 服务重新启动。 自动注册功能将以轻型模式注册 SQL Server Vm，除非 Windows Server 版本为2008，在这种情况下，将在 NoAgent 模式下注册 SQL Server VM。 

**向 SQL VM 资源提供程序注册时，轻型模式和 NoAgent 管理模式之间的区别是什么？** 

在 Windows Server 2008 上，NoAgent 管理模式是 SQL Server 2008 和 SQL Server 2008 R2 的唯一可用管理模式。 对于所有更高版本的 Windows Server，两种可用的可管理性模式为轻型和 full。 

NoAgent 模式需要客户设置 SQL Server 版本和版本属性。 轻型模式会查询 VM 以查找 SQL Server 实例的版本。

是否可以注册到 SQL VM 资源提供程序而不指定 SQL Server 许可证类型？

不是。 注册到 SQL VM 资源提供程序注册，SQL Server 许可证类型不是可选属性。 在所有可管理性模式下注册 SQL VM 资源提供程序时，必须将 SQL Server 许可证类型设置为即用即付或 Azure 混合权益 (NoAgent、轻型和完整) 。 如果安装了任何免费版本的 SQL Server 例如开发人员或评估版，则必须注册 "即用即付" 许可。 Azure 混合权益仅适用于付费版本的 SQL Server 例如 Enterprise edition 和 Standard edition。

**是否可以将 SQL Server IaaS 扩展从 NoAgent 模式升级到完整模式？**

不能。 在 NoAgent 模式下，不能将可管理性模式升级为 full 或轻型。 这是 Windows Server 2008 的技术限制。 需要首先将 OS 升级到 Windows Server 2008 R2 或更高版本，然后才能升级到完整管理模式。 

是否可以将 SQL Server IaaS 扩展从轻型模式升级到完整模式？

是的。 通过 Azure PowerShell 或 Azure 门户，支持从轻型升级到完整的可管理性模式。 这会触发 SQL Server 服务的重新启动。

**是否可以将 SQL Server IaaS 扩展从完整模式降级到 NoAgent 或轻型管理模式？**

不是。 不支持降级 SQL Server IaaS 扩展可管理性模式。 可管理性模式无法从完整模式降级为轻型模式或 NoAgent 模式，并且无法从轻型模式降级到 NoAgent 模式。 

若要从完全可管理性更改可管理性模式，请在 SQL VM 资源提供程序中 [注销](sql-vm-resource-provider-register.md#unregister-from-rp) SQL Server VM，方法是删除 sql 虚拟机 _资源_ ，然后在不同的管理模式下再次向 sql vm 资源提供程序重新注册 SQL Server VM。

是否可以从 Azure 门户注册到 SQL VM 资源提供程序？

不是。 无法在 Azure 门户中注册到 SQL VM 资源提供程序能。 仅 Azure CLI 或 Azure PowerShell 支持在 SQL VM 资源提供程序中注册。 

是否可以在安装 SQL Server 之前将 VM 注册到 SQL VM 资源提供程序？

不是。 VM 必须至少有一个 SQL Server (数据库引擎) 实例才能成功注册到 SQL VM 资源提供程序。 如果 VM 上没有 SQL Server 实例，则新的 Microsoft.SqlVirtualMachine 资源会处于失败状态。

如果有多个 SQL Server 实例，是否可以将 VM 注册到 SQL VM 资源提供程序？

是的。 SQL VM 资源提供程序将只注册一个 SQL Server（数据库引擎）实例。 在多个实例的情况下，SQL VM 资源提供程序将注册默认 SQL Server 实例。 如果没有默认实例，则仅支持在轻型模式下注册。 若要从轻型升级到完整可管理性模式，应存在默认 SQL Server 实例，或者 VM 应只有一个命名 SQL Server 实例。

是否可以将 SQL Server 故障转移群集实例注册到 SQL VM 资源提供程序？

是的。 Azure VM 上的 SQL Server 故障转移群集实例只能在轻型模式下注册到 SQL VM 资源提供程序。 但是，SQL Server 故障转移群集实例无法升级到完整可管理性模式。

如果配置了 Always On 可用性组，是否可以将 VM 注册到 SQL VM 资源提供程序？

是的。 如果在参与 Always On 可用性组配置，则将 Azure VM 上的 SQL Server 实例注册到 SQL VM 资源提供程序时没有任何限制。

注册到 SQL VM 资源提供程序或升级到完整可管理性模式的成本是多少？

无。 注册到 SQL VM 资源提供程序或使用三种可管理性模式中的任何一种都没有关联费用。 使用资源提供程序管理 SQL Server VM 完全免费。 

使用不同的可管理性模式会产生哪些性能影响？

使用无代理和轻型可管理性模式时，不会产生任何影响 。 从安装到 OS 的两个服务使用完整可管理性模式时，影响最小。 可以通过任务管理器监视这些服务，并可以在内置 Windows 服务控制台中查看。 

这两个服务名称为：
- `SqlIaaSExtensionQuery`（显示名称 - `Microsoft SQL Server IaaS Query Service`）
- `SQLIaaSExtension`（显示名称 - `Microsoft SQL Server IaaS Agent`）

**删除扩展如何实现？**

通过从 SQL VM 资源提供程序中 [注销](sql-vm-resource-provider-register.md#unregister-from-rp) SQL Server VM 来删除扩展。 

## <a name="next-steps"></a>后续步骤

若要在 Azure Vm 上安装 SQL Server IaaS 扩展以 SQL Server，请批量查看 [自动安装](sql-vm-resource-provider-automatic-registration.md)、 [单个 vm](sql-vm-resource-provider-register.md)或 [vm](sql-vm-resource-provider-bulk-register.md)的文章。

有关在 Azure 虚拟机中运行 SQL Server 的详细信息，请参阅[什么是 Azure 虚拟机中的 SQL Server？](sql-server-on-azure-vm-iaas-what-is-overview.md)。
