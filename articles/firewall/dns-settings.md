---
title: Azure 防火墙 DNS 设置（预览版）
description: 可以使用 DNS 服务器和 DNS 代理设置来配置 Azure 防火墙。
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: efe608437f350a29147cf10989cdb6c17a23196d
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397988"
---
# <a name="azure-firewall-dns-settings-preview"></a>Azure 防火墙 DNS 设置（预览版）

> [!IMPORTANT]
> Azure 防火墙 DNS 设置目前处于公共预览状态。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

可以为 Azure 防火墙配置自定义 DNS 服务器并启用 DNS 代理。 可以在部署防火墙时配置这些设置，或者稍后从“DNS 设置”页进行配置。

## <a name="dns-servers"></a>DNS 服务器

DNS 服务器维护域名并将它解析为 IP 地址。 默认情况下，Azure 防火墙将 Azure DNS 用于名称解析。 通过“DNS 服务器”设置，你可以配置自己的 DNS 服务器来用于 Azure 防火墙名称解析。 可以配置一台或多台服务器。

> [!NOTE]
> 对于使用 Azure 防火墙管理器管理的 Azure 防火墙，会在关联的 Azure 防火墙策略中配置 DNS 设置。

### <a name="configure-custom-dns-servers-preview---azure-portal"></a> (预览版配置自定义 DNS 服务器) -Azure 门户

1. 在 Azure 防火墙的“设置”下，选择“DNS 设置” 。
2. 在“DNS 服务器”下，可以键入或添加之前在虚拟网络中指定的现有 DNS 服务器。
3. 选择“保存” 。
4. 防火墙现在将 DNS 流量定向到指定的 DNS 服务器以进行名称解析。

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="DNS 服务器":::

### <a name="configure-custom-dns-servers-preview---azure-cli"></a> (预览版配置自定义 DNS 服务器) -Azure CLI

以下示例使用 Azure CLI 更新使用自定义 DNS 服务器的 Azure 防火墙。

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --dns-servers 10.1.0.4 10.1.0.5
```

> [!IMPORTANT]
> 命令 `az network firewall` 要求安装 Azure CLI 扩展 `azure-firewall` 。 可以使用命令来安装它 `az extension add --name azure-firewall` 。 

### <a name="configure-custom-dns-servers-preview---azure-powershell"></a> (预览版配置自定义 DNS 服务器) -Azure PowerShell

以下示例使用 Azure PowerShell 更新使用自定义 DNS 服务器的 Azure 防火墙。

```azurepowershell
$dnsServers = @("10.1.0.4", "10.1.0.5")
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSServer = $dnsServers

$azFw | Set-AzFirewall
```

## <a name="dns-proxy-preview"></a>DNS 代理（预览）

可以对 Azure 防火墙进行配置来充当 DNS 代理。 DNS 代理充当从客户端虚拟机到 DNS 服务器的 DNS 请求的中介。 如果配置自定义 DNS 服务器，应启用 DNS 代理以避免 DNS 解析不匹配，并在网络规则中启用 FQDN 筛选。

如果未启用 DNS 代理，来自客户端的 DNS 请求可能会在不同的时间传到 DNS 服务器，或者返回与防火墙不同的响应。 DNS 代理将 Azure 防火墙放置在客户端请求的路径中以避免不一致。

配置 DNS 代理需要三个步骤：
1. 在 Azure 防火墙 DNS 设置中启用 DNS 代理。
2. （可选）配置自定义 DNS 服务器或使用提供的默认设置。
3. 最后，必须在虚拟网络 DNS 服务器设置中将 Azure 防火墙的专用 IP 地址配置为自定义 DNS 地址。 这可确保 DNS 流量定向到 Azure 防火墙。

### <a name="configure-dns-proxy-preview---azure-portal"></a> (预览版配置 DNS 代理) -Azure 门户

若要配置 DNS 代理，必须将你的虚拟网络 DNS 服务器设置配置为使用防火墙专用 IP 地址。 然后，在 Azure 防火墙 **dns 设置** 中启用 DNS 代理。

#### <a name="configure-virtual-network-dns-servers"></a>配置虚拟网络 DNS 服务器 

1. 选择 DNS 流量将通过 Azure 防火墙路由的虚拟网络。
2. 在“设置”下，选择“DNS 服务器”。 
3. 在“DNS 服务器”下，选择“自定义” 。
4. 输入防火墙的专用 IP 地址。
5. 选择“保存”。
6. 重启已连接到虚拟网络的 VM，以便为其分配新的 DNS 服务器设置。 VM 在重启之前，将继续使用其当前 DNS 设置。

#### <a name="enable-dns-proxy-preview"></a>启用 DNS 代理（预览）

1. 选择 Azure 防火墙。
2. 在“设置”下，选择“DNS 设置” 。
3. 默认情况下，“DNS 代理”已禁用。 启用后，防火墙会在端口 53 上进行侦听，并将 DNS 请求转发到配置的 DNS 服务器。
4. 查看“DNS 服务器”配置以确保设置适用于你的环境。
5. 选择“保存”。

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="DNS 代理":::

### <a name="configure-dns-proxy-preview---azure-cli"></a> (预览配置 DNS 代理) -Azure CLI

在 Azure 防火墙中配置 DNS 代理设置，并将 Vnet 更新为使用 Azure 防火墙作为 DNS 服务器，可以使用 Azure CLI 来实现。

#### <a name="configure-virtual-network-dns-servers"></a>配置虚拟网络 DNS 服务器

此示例将 VNet 配置为使用 Azure 防火墙作为 DNS 服务器。
 
```azurecli-interactive
az network vnet update \
    --name VNetName \ 
    --resource-group VNetRG \
    --dns-servers <firewall-private-IP>
```

#### <a name="enable-dns-proxy-preview"></a>启用 DNS 代理（预览）

此示例在 Azure 防火墙中启用 DNS 代理功能。

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --enable-dns-proxy true
```

### <a name="configure-dns-proxy-preview---azure-powershell"></a> (预览配置 DNS 代理) -Azure PowerShell

使用 Azure PowerShell 配置 DNS 代理设置和更新 Vnet 以使用 Azure 防火墙作为 DNS 服务器。

#### <a name="configure-virtual-network-dns-servers"></a>配置虚拟网络 DNS 服务器

 此示例将 VNet 配置为使用 Azure 防火墙作为 DNS 服务器。

```azurepowershell
$dnsServers = @("<firewall-private-IP>")
$VNet = Get-AzVirtualNetwork -Name "VNetName" -ResourceGroupName "VNetRG"
$VNet.DhcpOptions.DnsServers = $dnsServers

$VNet | Set-AzVirtualNetwork
```

#### <a name="enable-dns-proxy-preview"></a>启用 DNS 代理（预览）

此示例在 Azure 防火墙中启用 DNS 代理功能。

```azurepowershell
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSEnableProxy = $true

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>后续步骤

[网络规则中的 FQDN 筛选](fqdn-filtering-network-rules.md)
