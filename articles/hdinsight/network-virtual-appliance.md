---
title: 在 Azure HDInsight 中配置网络虚拟设备
description: 了解如何在 Azure HDInsight 中为网络虚拟设备配置一些其他功能。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: c831e099eca3cd6e6da20f55ad19980ae8e9ddc5
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545916"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>在 Azure HDInsight 中配置网络虚拟设备

> [!Important]
> 仅当你想要将网络虚拟设备配置为除 [Azure Firewall](./hdinsight-restrict-outbound-traffic.md)外 (NVA) 时， **才** 需要以下信息。

Azure Firewall FQDN 标记自动配置为允许很多常见重要 Fqdn 的流量。 使用另一个网络虚拟设备将需要配置一些其他功能。 配置网络虚拟设备时请注意以下因素：

* 可以使用服务终结点配置支持服务终结点的服务，这通常会出于成本或性能考虑因素而导致绕过 NVA。
* 如果 "ResourceProviderConnection" 设置为 " *出站* "，则可以将专用终结点用于元存储的存储和 SQL server，而无需将其添加到 NVA。
* IP 地址依赖项适用于非 HTTP/S 流量（TCP 和 UDP 流量）。
* 可以在 NVA 设备中批准 FQDN HTTP/HTTPS 终结点。
* 将创建的路由表分配到 HDInsight 子网。

## <a name="service-endpoint-capable-dependencies"></a>支持服务终结点的依赖项

可以选择启用一个或多个以下服务终结点，这将导致绕过 NVA。 此选项可用于大量数据传输，以便节省成本和优化性能。 

| **终结点** |
|---|
| Azure SQL |
| Azure 存储 |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP 地址依赖项

| **终结点** | **详细信息** |
|---|---|
| [此处](hdinsight-management-ip-addresses.md)发布的 IP | 这些 Ip 适用于 HDInsight 资源提供程序，应包含在 UDR 中以避免非对称路由。 仅当 ResourceProviderConnection 设置为 " *入站* " 时，才需要此规则。 如果 ResourceProviderConnection 设置为 " *出站* "，则 UDR 中不需要这些 ip。  |
| AAD-DS 专用 IP | 仅当 Vnet 不是对等互连时，才需要 ESP 群集。|


### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS 依赖项

若要 [在此](https://github.com/Azure-Samples/hdinsight-fqdn-lists/)存储库中配置 NVA，可以获取 fqdn 依赖项列表 (主要是 azure 存储和 Azure 服务总线) 。 有关区域列表，请参阅 [此处](https://github.com/Azure-Samples/hdinsight-fqdn-lists/tree/master/Regional)。 这些依赖项由 HDInsight 资源提供程序 (RP) 使用，以便成功创建和监视/管理群集。 其中包括遥测/诊断日志、预配元数据、群集相关配置、脚本等。此 FQDN 依赖关系列表可能会随发布 HDInsight 更新而发生更改。

以下列表仅提供了在创建群集 *后* 和群集操作的生存期内，操作系统和安全修补或证书验证可能需要的几个 fqdn：

| **运行时依赖项 Fqdn**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| microsoft.com:80                                                      |
|login.windows.net:443                                                  |
|login.microsoftonline.com:443                                          |

## <a name="next-steps"></a>后续步骤

* [使用防火墙限制出站流量](./hdinsight-restrict-outbound-traffic.md)
* [Azure HDInsight 虚拟网络体系结构](hdinsight-virtual-network-architecture.md)