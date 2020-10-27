---
title: 用于 Redis 网络隔离选项的 Azure 缓存
description: 在本文中，你将学习如何确定最佳的网络隔离解决方案以满足你的需求。 我们将深入了解 Azure 专用链接、Azure 虚拟网络 (VNet) 注入和 Azure 防火墙规则及其优点和局限性。
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 0fda0b659dd2500e811fac1f53c99a9987276185
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92537467"
---
# <a name="azure-cache-for-redis-network-isolation-options"></a>用于 Redis 网络隔离选项的 Azure 缓存 
在本文中，你将学习如何确定最佳的网络隔离解决方案以满足你的需求。 我们将深入了解 Azure 专用链接、Azure 虚拟网络 (VNet) 注入和 Azure 防火墙规则及其优点和局限性。  

## <a name="azure-private-link-public-preview"></a>Azure Private Link (公共预览版)  
Azure 专用链接提供从虚拟网络到 Azure PaaS 服务的专用连接。 它简化了网络体系结构，并通过消除公共 internet 的数据泄露来保护 Azure 中的终结点之间的连接。 

### <a name="advantages"></a>优点
* 在用于 Redis 实例的基本、标准和高级 Azure 缓存上受支持。 
* 通过使用 [Azure 专用链接](../private-link/private-link-overview.md)，你可以通过专用终结点从虚拟网络连接到 Azure 缓存实例，该终结点在虚拟网络中的子网内分配有专用 IP 地址。 使用此方法，可在 VNet 内部和公开缓存实例。  
* 创建专用终结点后，可以通过标志限制对公用网络的访问 `publicNetworkAccess` 。 `Enabled`默认情况下，此标志设置为，为你提供允许对缓存进行公共和专用链接访问的选项。 如果设置为 `Disabled` ，则它将只允许专用链接访问。 您可以使用 PATCH 请求将值设置为 `Disabled` 。 有关详细信息，请参阅 [Azure Cache For Redis With Azure Private Link (Preview) ](cache-private-link.md)。 
* 所有外部缓存依赖关系不会影响 VNet 的 NSG 规则。

### <a name="limitations"></a>限制 
* 对专用终结点禁用 (NSG) 的网络安全组。 但是，如果子网上还有其他资源，NSG 强制将应用于这些资源。
* 目前尚不支持异地复制、防火墙规则、门户控制台支持、每个群集缓存有多个终结点、永久保留到防火墙和 VNet 注入的缓存。 
* 若要连接到群集缓存， `publicNetworkAccess` 需要将设置为 `Disabled` ，并且只能有一个专用终结点连接。

> [!NOTE]
> 将专用终结点添加到缓存实例时，所有 Redis 流量都将移动到专用终结点，因为 DNS。
> 确保之前调整以前的防火墙规则。  
>
>

## <a name="azure-virtual-network-injection"></a>Azure 虚拟网络注入 
VNet 是 Azure 中专用网络的基本构建基块。 VNet 使许多 Azure 资源能够安全地与其他网络、internet 和本地网络通信。 VNet 类似于传统的网络，你可以在自己的数据中心进行操作，但有 Azure 基础结构、规模、可用性和隔离的优点。 

### <a name="advantages"></a>优点
* 使用 VNet 配置 Redis 实例的 Azure 缓存时，它无法公开寻址，只能从 VNet 中的虚拟机和应用程序进行访问。  
* 当 VNet 与受限制的 NSG 策略结合时，它有助于降低数据渗透的风险。 
* VNet 部署为 Redis 的 Azure 缓存以及子网、访问控制策略和其他功能提供增强的安全性和隔离，以进一步限制访问。 
* 支持异地复制。 

### <a name="limitations"></a>限制
* VNet 注入的缓存仅适用于 Redis 的高级 Azure 缓存。 
* 使用 VNet 注入缓存时，需要打开 VNet 来缓存依赖关系，例如 Crl/PKI、AKV、Azure 存储、Azure Monitor 等。  


## <a name="azure-firewall-rules"></a>Azure 防火墙规则
[Azure 防火墙](../firewall/overview.md) 是一种托管的基于云的网络安全服务，可保护 Azure VNet 资源。 它是一种具有内置的高可用性和不受限制的云可伸缩性的完全有状态防火墙即服务。 可以跨订阅和虚拟网络集中创建、实施和记录应用程序与网络连接策略。  

### <a name="advantages"></a>优点
* 配置防火墙规则时，仅指定 IP 地址范围内的客户端连接可以连接到缓存。 即使配置了防火墙规则，仍始终允许来自 Azure Redis 缓存监视系统的连接。 还允许您定义的 NSG 规则。  

### <a name="limitations"></a>限制
* 防火墙规则可以与 VNet 注入的缓存一起使用，但不能与当前的专用终结点一起使用。 


## <a name="next-steps"></a>后续步骤
* 了解如何为 [Redis 实例的高级 Azure 缓存配置 VNet 注入的缓存](cache-how-to-premium-vnet.md)。  
* 了解如何为 [Redis 层的所有 Azure 缓存配置防火墙规则](cache-configure.md#firewall)。 
* 了解如何为 [Redis 层的所有 Azure 缓存配置专用终结点](cache-private-link.md)。