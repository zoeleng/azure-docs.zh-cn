---
title: 资源可用性（按区域）
description: Azure 容器实例服务的计算和内存资源在不同 Azure 区域的可用性。
ms.topic: article
ms.date: 04/27/2020
ms.custom: references_regions
ms.openlocfilehash: 1ed3f50198c0410d9c893fe87523fa214ca03d88
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521452"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure 容器实例在 Azure 区域的资源可用性

本文按目标操作系统详细介绍了 Azure 容器实例计算、内存和存储资源在 Azure 区域中的可用性。 有关 Azure 容器实例可用区域的一般列表，请参阅 [可用区域](https://azure.microsoft.com/regions/services/)。

提供的值是指部署一个[容器组](container-instances-container-groups.md)时可以使用的最大资源。 在本文发布时，值是最新的。

> [!NOTE]
> 在这些资源限制内创建的容器组受部署区域内可用性的限制。 某个区域负载较重时，部署实例时可能会失败。 若要减少此类部署失败，请尝试部署具有较低资源设置的实例，或稍后尝试部署，或在具有可用资源的其他区域中进行部署。

若要了解部署中的配额和其他限制，请参阅 [Azure 容器实例的配额和限制](container-instances-quotas.md)。

## <a name="linux-container-groups"></a>Linux 容器组

以下区域和最大资源可用于具有常规部署中的 Linux 容器的容器组、 [Azure 虚拟网络](container-instances-vnet.md) 部署和使用 [GPU 资源](container-instances-gpu.md) (预览版) 的部署。

> [!IMPORTANT]
> 区域中的最大资源因部署而异。 例如，在 Azure 虚拟网络部署中，某个区域可能有不同于一般部署的最大 CPU 和内存大小。 对于具有 GPU 资源的部署，同一区域还可能具有一组不同的最大值。 在检查下表中的最大值之前验证你的部署类型。

| Region | 最大 CPU | 最大内存 (GB) | VNET 最大 CPU | VNET 最大内存 (GB)  | 存储器 (GB) | GPU Sku (预览)  |
| -------- | :---: | :---: | :----: | :-----: | :-------: | :----: |
| 澳大利亚东部 | 4 | 16 | 4 | 16 | 50 | 空值 |
| Brazil South | 4 | 16 | 2 | 8 | 50 | 空值 |
| 加拿大中部 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 印度中部 | 4 | 16 | 空值 | 空值 | 50 | V100 |
| 美国中部 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 东亚 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 美国东部 | 4 | 16 | 4 | 16 | 50 | K80、P100、V100 |
| 美国东部 2 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 日本东部 | 2 | 8 | 4 | 16 | 50 | 空值 |
| 韩国中部 | 4 | 16 | 空值 | 空值 | 50 | 空值 |
| 美国中北部 | 2 | 3.5 | 4 | 16 | 50 | 空值 |
| 北欧 | 4 | 16 | 4 | 16 | 50 | K80 |
| 美国中南部 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 东南亚 | 4 | 16 | 4 | 16 | 50 | P100、V100 |
| 印度南部 | 4 | 16 | 空值 | 空值 | 50 | 空值 |
| 英国南部 | 4 | 16 | 4 | 16 | 50 | 空值 |
| 美国中西部| 4 | 16 | 4 | 16 | 50 | K80、P100、V100 |
| 西欧 | 4 | 16 | 4 | 16 | 50 | K80、P100、V100 |
| 美国西部 | 4 | 16 | 2 | 4 | 16| 空值 |
| 美国西部 2 | 4 | 16 | 4 | 16 | 50 | K80、P100、V100 |

以下最大资源可用于使用 [GPU 资源](container-instances-gpu.md) (预览) 部署的容器组。

| GPU Sku | GPU 计数 | 最大 CPU | 最大内存 (GB) | 存储器 (GB) |
| --- | --- | --- | --- | --- |
| K80 | 1 | 6 | 56 | 50 |
| K80 | 2 | 12 | 112 | 50 |
| K80 | 4 | 24 | 224 | 50 |
| P100、V100 | 1 | 6 | 112 | 50 |
| P100、V100 | 2 | 12 | 224 | 50 |
| P100、V100 | 4 | 24 | 448 | 50 |

## <a name="windows-container-groups"></a>Windows 容器组

以下区域和最大资源可用于 [支持和预览](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server 容器的容器组。

| Region | Windows Server 2016 最大 CPU | Windows Server 2016 最大内存 (GB)  | Windows Server 2019 LTSC 最大 CPU | Windows Server 2019 LTSC (GB) 的最大内存 | 存储器 (GB) |
| -------- | :---: | :---: | :----: | :-----: | :-------: |
| 澳大利亚东部 | 2 | 3.5 | 4 | 16 | 20 |
| Brazil South | 4 | 16 | 4 | 16 | 20 |
| 加拿大中部 | 2 | 3.5 | 4 | 16 | 20 |
| 印度中部 | 2 | 3.5 | 4 | 16 | 20 |
| 美国中部 | 2 | 3.5 | 4 | 16 | 20 |
| 东亚 | 2 | 3.5 | 4 | 16 | 20 |
| 美国东部 | 2 | 8 | 4 | 16 | 20 |
| 美国东部 2 | 2 | 3.5 | 2 | 3.5 | 20 |
| 法国中部 | 4 | 16 | 4 | 16 | 20 |
| 日本东部 | 4 | 16 | 4 | 16 | 20 |
| 韩国中部 | 4 | 16 | 4 | 16 | 20 |
| 美国中北部 | 2 | 3.5 | 4 | 16 | 20 |
| 北欧 | 2 | 3.5 | 4 | 16 | 20 |
| 美国中南部 | 2 | 3.5 | 4 | 16 | 20 |
| 印度南部 | 2 | 3.5 | 4 | 16 | 20 |
| 东南亚 | 2 | 3.5 | 4 | 16 | 20 |
| 英国南部 | 2 | 3.5 | 4 | 16 | 20 |
| 美国中西部 | 4 | 16 | 4 | 16 | 20 |
| 西欧 | 4 | 16 | 4 | 16 | 20 |
| 美国西部 | 4 | 14 | 空值 | 空值 | 20 |
| 美国西部 2 | 2 | 3.5 | 2 | 3.5 | 20 |

## <a name="next-steps"></a>后续步骤

如果希望看到更多的区域，或者希望提高资源可用性，请通过 [aka.ms/aci/feedback](https://aka.ms/aci/feedback) 告知我们的团队。

有关容器实例部署故障排除的信息，请参阅[排查 Azure 容器实例的部署问题](container-instances-troubleshooting.md)。


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
