---
title: Azure DDoS 保护标准常见问题
description: Azure DDoS 保护标准常见问题解答，有助于提供对 DDoS 攻击的防御。
services: virtual-network
documentationcenter: na
author: yitoh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: yitoh
ms.openlocfilehash: ed524642dc53835e686f52b53cfce0c16fb56377
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579090"
---
# <a name="azure-ddos-protection-standard-frequent-asked-questions"></a>Azure DDoS 保护标准常见问题

本文解答了有关 Azure DDoS 保护标准的常见问题。 

## <a name="what-is-a-distributed-denial-of-service-ddos-attack"></a>什么是分布式拒绝服务 (DDoS) 攻击？
分布式拒绝服务（DDoS）是一种攻击，其中攻击者向应用程序发送的请求数超过了应用程序的处理能力。 产生的影响是资源耗尽，这会影响应用程序的可用性并为其客户提供服务。 在过去几年中，该行业已经明显增加了攻击，而且攻击变得越来越复杂，更大。 DDoS 攻击可能会将任何可通过 Internet 公开访问的终结点作为目标。

## <a name="what-is-azure-ddos-protection-standard-service"></a>什么是 Azure DDoS 保护标准服务？
Azure DDoS 保护标准与应用程序设计最佳做法相结合，提供增强的 DDoS 缓解功能，防范 DDoS 攻击。 它会自动进行优化，以帮助保护虚拟网络中的特定 Azure 资源。 可在任何新的或现有的虚拟网络上启用保护，且无需对应用程序或资源做出任何更改。 与基本服务相比，该服务具有多种优势，包括日志记录、警报和遥测。 请参阅 [Azure DDoS 保护标准概述](ddos-protection-overview.md)。 

## <a name="what-about-protection-at-the-service-layer-layer-7"></a> (第7层) ，服务层上的保护是怎样的？
客户可将 Azure DDoS 保护服务与 [应用程序网关 WAF SKU](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview) 结合使用，以实现 (第3层和第4层（由 Azure DDoS 保护) 服务提供）的网络层、 (第7层（由应用程序网关 WAF SKU) 提供）提供保护。

## <a name="are-services-unsafe-in-azure-without-the-service"></a>Azure 中是否不包含服务的服务不安全？
在 Azure 上运行的服务在本质上受 Azure DDoS 保护基本保护，并已准备好保护 Azure 的基础结构。 但是，保护基础结构的保护的阈值要高得多，但它并不提供遥测或警报，因此，在接收流量时，可能会对接收它的应用程序造成破坏。 

通过载入 Azure DDoS 保护标准服务，应用程序可获取专用监视以检测攻击和应用程序特定的阈值。 服务将使用一个配置文件进行保护，该配置文件会优化到其预期的流量量，从而更严格防御 DDoS 攻击。

## <a name="what-are-the-supported-protected-resource-types"></a>受支持的受保护资源类型有哪些？
基于 ARM 的 Vnet 中的公共 Ip 目前是唯一受保护的资源类型。 PaaS 服务 (不支持多租户) 。 请参阅 [Azure DDoS 保护标准参考体系结构](ddos-protection-reference-architectures.md)。

## <a name="are-classicrdfe-protected-resources-supported"></a>是否支持经典/RDFE 受保护的资源？
预览版仅支持基于 ARM 的受保护资源。 不支持经典/RDFE 部署中的 Vm。 目前尚不打算针对经典/RDFE 资源提供支持。 请参阅 [Azure DDoS 保护标准参考体系结构](ddos-protection-reference-architectures.md)。

## <a name="can-i-protect-my-on-premise-resources-using-ddos-protection"></a>能否使用 DDoS 保护来保护本地资源？
需要将服务的公共终结点关联到 Azure 中的 VNet，才能启用 DDoS 保护。 示例设计包括：
- 网站 (本地 datacenter 中的 Azure 和后端数据库中的 IaaS) 。 
- Azure 中的应用程序网关 (在本地数据中心的应用网关/WAF) 和网站上启用 DDoS 保护。

请参阅 [Azure DDoS 保护标准参考体系结构](ddos-protection-reference-architectures.md)。

## <a name="can-i-register-a-domain-outside-of-azure-and-associate-that-to-a-protected-resource-like-vm-or-elb"></a>能否在 Azure 外部注册某个域，并将其关联到受保护的资源（例如 VM 或 ELB）？
对于公共 IP 方案，DDoS 保护服务将支持任何应用程序，无论关联域在何处注册或托管，只要关联的公共 IP 托管在 Azure 上。 

## <a name="can-i-manually-configure-the-ddos-policy-applied-to-the-vnetspublic-ips"></a>是否可以手动配置应用于 Vnet/公共 Ip 的 DDoS 策略？
不可以，目前无法使用策略自定义。

## <a name="can-i-allowlistbloclist-specific-ip-addresses"></a>能否允许列表/bloclist 特定的 IP 地址？
不，此时无法进行手动配置。

## <a name="how-can-i-test-ddos-protection"></a>如何测试 DDoS 防护？
请参阅 [通过模拟进行测试](test-through-simulations.md)。

## <a name="how-long-does-it-take-for-the-metrics-to-load-on-portal"></a>在门户上加载指标需要多长时间？
指标应在5分钟内显示在门户上。 如果资源受到攻击，其他度量值将在5-7 分钟内开始显示在门户上。 
    



