---
title: 更改 Azure 托管磁盘的性能
description: 了解托管磁盘的性能层，并了解如何使用 Azure PowerShell 模块或 Azure CLI 更改现有托管磁盘的性能层。
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 11/11/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 923c5970183bd192ac1a2f20fb775d96dcc06865
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540631"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>托管磁盘的性能层（预览）

Azure 磁盘存储提供内置突发功能，以提供更高的性能来处理短期的意外流量。 高级 SSD 在不增加实际磁盘大小的情况下，可灵活地提高磁盘性能。 此功能使你可以匹配工作负载性能需求并降低成本。 

> [!NOTE]
> 此功能目前处于预览状态。 

此功能非常适合于那些暂时需要持续较高性能级别的事件，例如假日购物、性能测试或运行训练环境。 为了应对这些事件，你可根据需要使用级别较高的性能层。 当你不再需要额外性能时，可返回到原始层。

## <a name="how-it-works"></a>工作原理

首次部署或预配磁盘时，该磁盘的基线性能层将根据预配的磁盘大小进行设置。 可使用级别较高的性能层来满足更高的需求。 当你不再需要该性能级别时，可返回到初始基线性能层。

你的帐单会随层级的变化而变化。 例如，如果你预配了 P10 磁盘 (128 GiB)，基线性能层将设置为 P10（500 IOPS 和 100 MBps）。 将按 P10 费率计费。 可以升级该层，以匹配 P50（7500 IOPS 和 250 MBps）的性能，而不增加磁盘大小。 在升级期间，将按 P50 费率计费。 如果不再需要更高的性能，可返回到 P10 层。 磁盘将再次按 P10 费率计费。

| 磁盘大小 | 基线性能层 | 可升级到 |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P2、P3、P4、P6、P10、P15、P20、P30、P40、P50 |
| 8 GiB | P2 | P3、P4、P6、P10、P15、P20、P30、P40、P50 |
| 16 GiB | P3 | P4、P6、P10、P15、P20、P30、P40、P50 | 
| 32 GiB | P4 | P6、P10、P15、P20、P30、P40、P50 |
| 64 GiB | P6 | P10、P15、P20、P30、P40、P50 |
| 128 GiB | P10 | P15、P20、P30、P40、P50 |
| 256 GiB | P15 | P20、P30、P40、P50 |
| 512 GiB | P20 | P30、P40、P50 |
| 1 TiB | P30 | P40、P50 |
| 2 TiB | P40 | P50 |
| 4 TiB | P50 | 无 |
| 8 TiB | P60 |  P70、P80 |
| 16 TiB | P70 | P80 |
| 32 TiB | P80 | 无 |

有关计费信息，请参阅[托管磁盘定价](https://azure.microsoft.com/pricing/details/managed-disks/)。

## <a name="restrictions"></a>限制

- 目前仅高级 SSD 支持此功能。
- 必须先解除分配 VM 或从正在运行的 VM 中分离磁盘，然后才能更改磁盘层。
- P60、P70 和 P80 性能层的使用仅限于 4096 GiB 或更大的磁盘。
- 磁盘的性能层只能每24小时进行一次降级。

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>创建一个层级高于基线层的空数据磁盘

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
subscriptionId=<yourSubscriptionIDHere>
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
diskSize=<yourDiskSizeHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus

az login

az account set --subscription $subscriptionId

az disk create -n $diskName -g $resourceGroupName -l $region --sku Premium_LRS --size-gb $diskSize --tier $performanceTier
```
## <a name="create-an-os-disk-with-a-tier-higher-than-the-baseline-tier-from-an-azure-marketplace-image"></a>从 Azure 市场映像创建一个层级高于基线层的 OS 磁盘

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus
image=Canonical:UbuntuServer:18.04-LTS:18.04.202002180

az disk create -n $diskName -g $resourceGroupName -l $region --image-reference $image --sku Premium_LRS --tier $performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$subscriptionId='yourSubscriptionID'
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$diskSizeInGiB=4
$performanceTier='P50'
$sku='Premium_LRS'
$region='westcentralus'

Connect-AzAccount

Set-AzContext -Subscription $subscriptionId

$diskConfig = New-AzDiskConfig -SkuName $sku -Location $region -CreateOption Empty -DiskSizeGB $diskSizeInGiB -Tier $performanceTier
New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
```
---

## <a name="update-the-tier-of-a-disk"></a>更新磁盘层级

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName='yourResourceGroupName'
$diskName='yourDiskName'
$performanceTier='P1'

$diskUpdateConfig = New-AzDiskUpdateConfig -Tier $performanceTier

Update-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName -DiskUpdate $diskUpdateConfig
```
---

## <a name="show-the-tier-of-a-disk"></a>显示磁盘层级

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName -DiskName $diskName

$disk.Tier
```
---

## <a name="next-steps"></a>后续步骤

如果需要调整磁盘大小以利用更高的性能层，请参阅以下文章：

- [使用 Azure CLI 扩展 Linux VM 上的虚拟硬盘](linux/expand-disks.md)
- [展开附加到 Windows 虚拟机的托管磁盘](windows/expand-os-disk.md)
