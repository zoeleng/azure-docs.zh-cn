---
title: 虚拟机和磁盘性能
description: 详细了解 Vm 及其附加的磁盘如何结合使用以提高性能
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 092368bb66784a00d5116da0b6be6513f8ebb261
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518041"
---
# <a name="virtual-machine-and-disk-performance"></a>虚拟机和磁盘性能
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>虚拟机未缓存 vs 缓存限制
同时启用高级存储和高级存储缓存的虚拟机具有两个不同的存储带宽限制。 让我们看看 Standard_D8s_v3 虚拟机作为示例。 下面是有关 [Dsv3 系列](../dv3-dsv3-series.md) 和 Standard_D8s_v3 的文档：

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

让我们对此虚拟机和创建 IO 活动的磁盘组合运行基准测试。 若要了解如何在 Azure 上对存储 IO 进行基准测试，请参阅 [Azure 磁盘存储上的应用程序基准](disks-benchmarks.md)。 通过基准测试工具，可以看到 VM 和磁盘组合可以实现 22800 IOPS：

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
