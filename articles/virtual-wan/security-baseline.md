---
title: 适用于虚拟 WAN 的 Azure 安全基准
description: 虚拟 WAN 安全基线为实现 Azure 安全基准中指定的安全建议提供过程指南和资源。
author: msmbaldwin
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 409c8d163b52e56a1eef68a23e4db79956a68722
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328668"
---
# <a name="azure-security-baseline-for-virtual-wan"></a>适用于虚拟 WAN 的 Azure 安全基准

此安全基准应用 [Azure 安全基准2.0 版](../security/benchmarks/overview.md) 中的指南，以 MICROSOFT AZURE 虚拟 WAN。 Azure 安全基准提供有关如何在 Azure 上保护云解决方案的建议。 内容由 Azure 安全基准定义的 **安全控制** 和适用于虚拟 WAN 的相关指南进行分组。 排除了不适用于虚拟 WAN 的 **控件** 。

若要查看虚拟 WAN 如何完全映射到 Azure 安全基准，请参阅 [完整的虚拟 wan 安全基线映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

## <a name="network-security"></a>网络安全

*有关详细信息，请参阅 [Azure 安全基准：网络安全](/azure/security/benchmarks/security-controls-v2-network-security)。*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1：实现内部流量的安全性

**指南** ： AZURE 虚拟 WAN 不支持直接部署到虚拟网络。 但是，你仍然可以对轮辐资源应用网络安全组规则，使用 Azure 防火墙的保护，或者创建自定义路由表以防止或允许专用流量。

- [自定义路由方案](scenario-any-to-any.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ns-2-connect-private-networks-together"></a>NS-2：将专用网络连接在一起 

**指南** ：使用 azure ExpressRoute 或 azure 虚拟专用网络 (VPN) 在归置环境中的 Azure 数据中心和本地基础结构之间创建专用连接。 ExpressRoute 连接不通过公共 internet，提供更高的可靠性、更快的速度和比典型 internet 连接更低的延迟。 

对于点到站点 VPN 和站点到站点 VPN，请使用这些 VPN 选项和 ExpressRoute 的任意组合将本地设备或网络连接到虚拟网络。 若要将 Azure 中的两个或多个虚拟网络连接在一起，请使用虚拟网络对等互连。 对等互连虚拟网络之间的网络流量是专用的，且保留在 Azure 主干网络上。

- [虚拟 WAN 中的 ExpressRoute](virtual-wan-expressroute-portal.md)

- [站点到站点 VPN 概述](virtual-wan-site-to-site-portal.md)

- [点到站点 VPN 概述](virtual-wan-point-to-site-portal.md)

- [专用链接](howto-private-link.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：共享

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3：建立对 Azure 服务的专用网络访问

**指南** ：使用 Azure 专用链接，无需跨 internet 即可从虚拟网络对 AZURE 虚拟 WAN 进行专用访问。 除了 Azure 服务提供的身份验证和流量安全性外，专用访问还是另一项深层防御措施。

- [了解 Azure 专用链接](../private-link/private-link-overview.md)

- [虚拟 Wan 专用链接](howto-private-link.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4：保护应用程序和服务不受外部网络攻击

**指南** ：针对外部网络的攻击保护 AZURE 虚拟 WAN 资源，包括分布式拒绝服务 (DDoS) 攻击、特定于应用程序的攻击，以及未经请求和潜在恶意的 internet 流量。 

使用 Azure 防火墙保护应用程序和服务免受来自 Internet 和其他外部位置的潜在恶意流量的侵害。 针对 Azure 虚拟网络上的攻击，选择适用于你的资产的 Azure DDoS 保护。 使用 Azure 安全中心来检测与网络相关资源相关的错误配置风险。

- [Azure 防火墙文档](/azure/firewall)

- [使用 Azure 门户管理 Azure DDoS 防护标准](/azure/virtual-network/manage-ddos-protection) 

- [Azure 安全中心建议](../security-center/recommendations-reference.md#recs-network)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5：部署入侵检测/入侵防护系统 (IDS/IPS)

**指南** ：在 AZURE 虚拟 WAN 中部署入侵检测或防护系统的最佳方式是在连接到虚拟中心的辐射网络中使用网络虚拟设备。  有关详细信息，请访问引用链接上的路由方案。 

- [场景 - 通过 NVA 路由流量](scenario-route-through-nva.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ns-6-simplify-network-security-rules"></a>NS-6：简化网络安全规则

**指南** ：使用 Azure 虚拟网络服务标记定义为虚拟 WAN 资源配置的 azure 网络安全组或 azure 防火墙上的网络访问控制。 

创建安全规则时，使用服务标记代替特定的 IP 地址。 通过在规则的相应 "源" 或 "目标" 字段中指定服务标记名称 (例如： {VirtualWAN： Virtual Network} ) ，可以允许或拒绝相应服务的流量。 Microsoft 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7：安全域名服务 (DNS)

**指南** ： AZURE 虚拟 WAN 为点到站点 VPN 网关提供自定义 DNS 服务器。 

遵循 DNS 安全的最佳做法来防范常见攻击，例如无关联 DNS、DNS amplifications 攻击、DNS 中毒和欺骗等。

当 Azure DNS 用作你的权威 DNS 服务时，请确保使用 Azure RBAC) 和资源锁 (的 azure RBAC 访问控制保护 DNS 区域和记录，以防意外或恶意修改。

- [Azure DNS 概述](../dns/dns-overview.md) 

- [安全域名系统 (DNS) 部署指南](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) 

- [阻止无关联的 DNS 项并避免子域接管](../security/fundamentals/subdomain-takeover.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

## <a name="identity-management"></a>标识管理

*有关详细信息，请参阅 [Azure 安全基准：标识管理](/azure/security/benchmarks/security-controls-v2-identity-management)。*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1：标准化 Azure Active Directory 作为中心标识和身份验证系统

**指南** ： AZURE 虚拟 WAN 的点到站点 VPN 与 Azure Active Directory (Azure AD) ，后者是 Azure 的默认标识和访问管理服务。 应将 Azure AD 标准化，以便在中管理组织的标识和访问管理：

- Microsoft 云资源，如 Azure 门户、Azure 存储、Azure 虚拟机 (Linux 和 Windows) 、Azure Key Vault、PaaS 和 SaaS 应用程序。
- 你的组织的资源，例如 Azure 上的应用程序，或公司网络资源。

在组织的云安全实践中，安全 Azure AD 优先级高。 通过 Azure 安全中心的安全评分功能评估你的身份和安全状况，以衡量你的配置与最佳做法建议的匹配程度。 实现 Microsoft 的最佳实践建议，以提高安全状况。

Azure AD 还支持外部标识，这允许没有 Microsoft 帐户的用户使用其外部标识登录到其应用程序和资源。 有关在点到站点 VPN 方案中使用 Azure AD 的信息，请参阅引用的链接。

- [为 P2S OpenVPN 协议连接创建 Azure Active Directory (AD) 租户](openvpn-azure-ad-tenant-multi-app.md)

- [为用户 VPN 配置 Azure Active Directory 身份验证](virtual-wan-point-to-site-azure-ad.md)

- [Azure Active Directory 中的租赁](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [如何创建和配置 Azure AD 实例](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [使用应用程序的外部标识提供程序](/azure/active-directory/b2b/identity-providers)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4：对基于 Azure Active Directory 的所有访问使用强身份验证控制

**指南** ： AZURE 虚拟 WAN 的点到站点 VPN 与 Azure Active Directory (Azure AD) 集成，支持具有多重身份验证的强身份验证控件和无密码方法。

- 多重身份验证-启用 Azure AD 多重身份验证，并遵循 Azure 安全中心的标识和访问管理建议，获取多重身份验证设置的最佳实践。 多重身份验证可在所有用户上强制执行，根据登录条件和风险因素，选择用户或按用户级别。

- 无密码 authentication –三个无密码 authentication 选项可用。 其中包括 Windows Hello 企业版、Microsoft Authenticator 应用和本地身份验证方法（如智能卡）。

对于管理员和特权用户，请确保使用强身份验证方法的最高级别，然后向其他用户推出适当的强身份验证策略。

- [如何在 P2S VPN 中为虚拟广域网启用 MFA](openvpn-azure-ad-mfa.md)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6：根据条件限制 Azure 资源访问

**指南** ： AZURE 虚拟 WAN 的点到站点 VPN 支持通过基于用户定义的条件进行更精细的访问控制 Azure AD 条件性访问。 例如，来自某些 IP 范围的用户登录名必须使用多重身份验证登录。 精细身份验证会话管理策略还可用于不同的用例。

- [常见的条件访问策略](../active-directory/conditional-access/concept-conditional-access-policy-common.md) 

- [使用条件访问配置身份验证会话管理](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

- [虚拟 Wan P2S VPN 条件性访问](openvpn-azure-ad-mfa.md#conditional)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7：消除意外的凭据公开

**指南** ：实施凭据扫描程序来识别代码中的凭据。 凭据扫描程序还会建议将发现的凭据转移到更安全的位置，例如 Azure Key Vault。

对于 GitHub，你可以使用原生的机密扫描功能来识别代码中的凭据或其他形式的机密。

- [如何设置凭据扫描程序](https://secdevtools.azurewebsites.net/helpcredscan.html) 

- [GitHub 机密扫描](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

## <a name="privileged-access"></a>特权访问

*有关详细信息，请参阅 [Azure 安全基准：特权访问](/azure/security/benchmarks/security-controls-v2-privileged-access)。*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2：限制对关键业务型系统的管理访问权限

**指南** ： AZURE 虚拟 WAN 使用 azure 基于角色的访问控制 (azure RBAC) 通过限制授予了对订阅和管理组的特权访问权限的帐户，隔离对关键系统的访问。

还将访问权限限制为对业务关键访问具有管理访问权限的管理、标识和安全系统，如 Active Directory 域控制器、安全工具以及安装在业务关键系统上的代理的系统管理工具。 入侵这些管理和安全系统的攻击者可以立即将它们用作损害业务关键型资产的武器。

所有类型的访问控制都应符合企业分段策略，确保访问控制保持一致。

- [Azure 组件和参考模型](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [访问管理组](../governance/management-groups/overview.md#management-group-access) 

- [Azure 订阅管理员](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6：使用特权访问工作站

**指南** ：安全的隔离工作站对于敏感角色（如管理员、开发人员和关键服务操作员）的安全性至关重要。 使用高度安全的用户工作站或 Azure 堡垒执行管理任务。 使用 Azure Active Directory (Azure AD) ，Microsoft Defender 高级威胁防护 (ATP) 或 Microsoft Intune 部署用于管理任务的安全管理用户工作站。 可以集中管理安全的工作站来强制实施安全配置，包括强身份验证、软件和硬件基线、受限的逻辑和网络访问。

- [了解特权访问工作站](../active-directory/devices/concept-azure-managed-workstation.md)

- [部署特权访问工作站](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

## <a name="data-protection"></a>数据保护

*有关详细信息，请参阅 [Azure 安全基准：数据保护](/azure/security/benchmarks/security-controls-v2-data-protection)。*

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4：加密传输中的敏感信息

**指南** ：对外部网络和公用网络上的流量进行加密至关重要。

- 使用访问控制时，应保护传输中的数据不受 "带外" 攻击 (例如，使用加密的流量捕获) ，以确保攻击者无法轻松读取或修改数据。

- 确保 HTTP 流量，连接到 Azure 资源的任何客户端都可以协商 TLS 1.2 或更高版本。

- 对于远程管理，请使用适用于 Linux 的 SSH () 或适用于) Windows 的 RDP/TLS (，而不是使用未加密的协议。 应禁用已过时的 SSL/TLS/SSH 版本、协议和弱密码。

- 在底层基础结构中，Azure 默认为 Azure 数据中心之间的数据流量提供传输加密数据。

通常，我们在安全的 Microsoft 骨干和机会上提供加密，以通过 Azure ExpressRoute 和点到站点用户 VPN 加密站点到站点 VPN、站点到站点 VPN 中的流量。

- [了解 Azure 传输中的加密](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [有关 TLS 安全性的信息](/security/engineering/solving-tls1-problem)

- [传输中的 Azure 数据的双重加密](../security/fundamentals/double-encryption.md#data-in-transit)

**Azure 安全中心监视** ：是

**责任** ：共享

## <a name="asset-management"></a>资产管理

*有关详细信息，请参阅 [Azure 安全基准：资产管理](/azure/security/benchmarks/security-controls-v2-asset-management)。*

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2：确保安全团队有权访问资产清单和元数据

**指南** ：将标记应用于 Azure 资源、资源组和订阅，以逻辑方式将它们组织到分类。 每个标记均由名称和值对组成。 例如，可以对生产中的所有资源应用名称“Environment”和值“Production”。

Azure 虚拟 WAN 还支持基于 Azure 资源管理器的资源部署和 Azure 资源 Graph 查询。 

- [有关标记资产的详细信息，请参阅资源命名和标记决策指南](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

- [如何使用 Azure Resource Graph 浏览器创建查询](../governance/resource-graph/first-query-portal.md) 

- [Azure 安全中心资产清单管理](../security-center/asset-inventory.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="am-3-use-only-approved-azure-services"></a>AM-3：仅使用已批准的 Azure 服务

**指南** ：使用 Azure 策略限制可在环境中预配的服务。 使用 Azure 资源关系图查询和发现订阅中的资源，并使用 Azure Monitor 创建在检测到未批准的服务时触发警报的规则。

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [如何在 Azure 策略中拒绝特定的资源类型](../governance/policy/samples/built-in-policies.md#general)

- [如何使用 Azure Resource Graph 浏览器创建查询](../governance/resource-graph/first-query-portal.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5：限制用户与 Azure 资源管理器进行交互的能力

**指南** ：通过为 "Microsoft Azure 管理" 应用配置 "阻止访问"，使用 Azure 条件访问限制用户与 Azure 资源管理器进行交互的能力。

- [如何配置条件访问以阻止访问 Azure 资源管理器](../role-based-access-control/conditional-access-azure-management.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

## <a name="logging-and-threat-detection"></a>日志记录和威胁检测

*有关详细信息，请参阅 [Azure 安全基准：日志记录和威胁检测](/azure/security/benchmarks/security-controls-v2-logging-threat-protection)。*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2：启用 Azure 标识和访问管理的威胁检测

**指南** ： Azure Active Directory (Azure AD) 提供以下用户日志，可在 Azure AD 报告中查看这些用户日志，或与 Azure Monitor、Azure SENTINEL、SIEM 或监视工具集成，以实现更复杂的监视和分析用例。 其中包括：

- 登录– "登录" 报表提供有关托管应用程序和用户登录活动的使用情况的信息。
- 审核日志 - 通过日志为 Azure AD 中的各种功能所做的所有更改提供可跟踪性。 审核日志的示例包括对 Azure AD 中的任何资源所做的更改，如添加或删除用户、应用、组、角色和策略。
- 有风险的登录-有风险登录是指可能已由非用户帐户合法所有者执行的登录尝试。
- 已标记为存在风险的用户 - 风险用户是指可能已泄露的用户帐户。

使用 Azure 安全中心来创建有关某些可疑活动的警报，如过多失败的身份验证尝试，包括订阅中不推荐使用的帐户。 除了基本的安全卫生监视之外，还可以使用安全中心的威胁防护模块从单个 Azure 计算资源收集更详细的安全警报， (虚拟机、容器、应用服务) 、SQL) 数据库 (的数据资源以及 Azure 服务层。 此功能可让你查看单个资源内的帐户异常情况。

- [Azure Active Directory 中的审核活动报表](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [启用 Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3：为 Azure 网络活动启用日志记录

**指南** ：使用 VPN 数据包捕获工具记录在 AZURE 虚拟 WAN 资源之间传输的网络数据包。 这可以帮助与混合网络相关的调试过程。 尽管不能在虚拟 WAN 上部署网络安全组，但可以将其部署到辐射虚拟网络资源。

为流量审核启用网络安全组流日志。

启用 Azure 流量分析功能以处理保留在存储帐户中的流日志，并将它们发送到 Log Analytics 工作区。 流量分析提供有关 Azure 网络流量的更深入见解。 流量分析的优势包括能够可视化网络活动、识别热点、识别安全威胁、了解流量流模式，以及查明网络不当配置。

- [如何启用网络安全组流日志](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [如何启用和使用流量分析](../network-watcher/traffic-analytics.md) 

虚拟 Wan 不会生成或处理需要启用的 DNS 查询日志。

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4：为 Azure 资源启用日志记录

**指南** ： azure 活动日志（自动启用）包含 AZURE 虚拟 WAN 资源 (PUT、POST、DELETE) 的所有写入操作，但读取 (获取) 操作。 活动日志可用于在故障排除过程中查找错误或监视组织中的用户如何修改资源。 不过，虚拟 WAN 不会生成 Azure 资源日志。

- [如何使用 Azure Monitor 收集平台日志和指标](../azure-monitor/platform/diagnostic-settings.md) 
- [了解 Azure 中的日志记录和不同的日志类型](../azure-monitor/platform/platform-logs-overview.md)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5：集中管理和分析安全日志

**指南** ：集中记录存储和分析以启用关联。 对于每个日志源，请确保已分配数据所有者、访问指南、存储位置、用于处理和访问数据的工具以及数据保留要求。 确保将 Azure 活动日志集成到中央日志记录系统。 

通过 Azure Monitor 引入日志，以聚合终结点设备、网络资源和其他安全系统生成的安全数据。 在 Azure Monitor 中，使用 Log Analytics 工作区来查询和执行分析，并使用 Azure 存储帐户长期和存档存储。 此外，启用数据并将数据集成到 Azure Sentinel 或第三方 SIEM。

许多组织选择使用 Azure Sentinel 作为 "热" 数据，这些数据经常使用，而 Azure 存储则用于不频繁使用的 "冷" 数据。

- [如何使用 Azure Monitor 收集平台日志和指标](../azure-monitor/platform/diagnostic-settings.md) 

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：共享

## <a name="incident-response"></a>事件响应

*有关详细信息，请参阅 [Azure 安全基准：事件响应](/azure/security/benchmarks/security-controls-v2-incident-response)。*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1：准备–更新 Azure 的事件响应过程

**指南** ：确保你的组织具有响应安全事件的流程，已为 Azure 更新这些流程，并定期运用这些流程来确保准备情况。 如果适用，请确保服务产品包含在事件响应过程中。

- [在企业环境中实现安全性](https://aka.ms/AzSec4) 
- [事件响应参考指南](https://aka.ms/IRRG)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2：准备–安装事件通知

**指南** ：在 Azure 安全中心中设置安全事件联系人信息。 如果 Microsoft 安全响应中心 (MSRC) 发现非法或未经授权的一方访问了你的数据，Microsoft 将使用此联系信息来与你取得联系。 还可以选择基于事件响应需求在不同的 Azure 服务中自定义事件警报和通知。

如果 Microsoft 安全响应中心 (MSRC) 发现非法或未经授权的一方访问了你的数据，Microsoft 将使用安全事件联系人信息来与你取得联系。 在事实后查看事件，以确保完全解决问题。

- [如何设置 Azure 安全中心安全联系人](../security-center/security-center-provide-security-contact-details.md) 

- [如何设置 Azure 安全中心安全联系人](../security-center/security-center-provide-security-contact-details.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3：检测和分析-基于高质量警报创建事件

**指导** ：Azure 安全中心为每条警报分配严重性，方便你根据优先级来确定应该最先调查的警报。 严重性取决于安全中心在查找或用于发出警报的分析中的置信度，以及导致警报的活动的恶意意图的置信度。

此外，还可以清楚地标记订阅 (例如，生产、非生产) ，并创建一个命名系统来明确识别和分类 Azure 资源。

- [Azure 安全中心中的安全警报](../security-center/security-center-alerts-overview.md) 

- [使用标记整理 Azure 资源](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4：检测和分析-调查事件

**指南** ：为组织制定事件响应指南。 执行练习来测试系统的事件响应功能是否正常。 识别弱点和差距，并根据需要修改计划。

确保在书面的事件响应计划中定义人员职责，以及事件处理/管理从检测到事件后审查的各个阶段。

- [如何在 Azure 安全中心配置工作流自动化](../security-center/security-center-planning-and-operations-guide.md) 

- [关于建立自己的安全事件响应流程的指南](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [Microsoft 安全响应中心事件分析](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [请参阅 NIST 的刊物：Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)（IT 规划和功能的测试、培训与演练计划指南）

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR 5：检测和分析–优先级事件

**指导** ：Azure 安全中心为每条警报分配严重性，方便你根据优先级来确定应该最先调查的警报。 严重性取决于安全中心在发出警报时所依据的检测结果和分析结果的置信度，以及导致发出警报的活动的恶意企图的置信度。

此外，还可以清楚地标记订阅 (例如生产、非生产) ，并创建一个命名系统来明确识别和分类 Azure 资源。

- [Azure 安全中心中的安全警报](../security-center/security-center-alerts-overview.md) 

- [使用标记整理 Azure 资源](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视** ：是

**责任** ：共享

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6：包含、根除和恢复-自动化事件处理

**指南** ：若要在安全警报和建议上通过 "Azure 逻辑应用" 自动触发响应，请使用 Azure 安全中心中的工作流自动化功能。

- [如何配置工作流自动化和逻辑应用](../security-center/workflow-automation.md)

**Azure 安全中心监视** ：是

**责任** ：客户

## <a name="posture-and-vulnerability-management"></a>状况和漏洞管理

*有关详细信息，请参阅 [Azure 安全基准：状况和漏洞管理](/azure/security/benchmarks/security-controls-v2-vulnerability-management)。*

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8：执行定期攻击模拟

**指南** ：根据你的要求，对 Azure 资源进行渗透测试或 red 团队活动，并确保对所有关键安全发现的修正。

请遵循 Microsoft 云渗透测试互动规则，确保你的渗透测试不违反 Microsoft 政策。 使用 Microsoft 托管的云基础结构、服务和应用程序的策略、执行红组和实时站点渗透测试。

- [Azure 中的渗透测试](../security/fundamentals/pen-testing.md)

- [参与的渗透测试规则](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft 云红色组合](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="endpoint-security"></a>终结点安全

*有关详细信息，请参阅 [Azure 安全基准：终结点安全性](/azure/security/benchmarks/security-controls-v2-endpoint-security)。*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1：使用终结点检测和响应 (EDR)

**指南** ：不明确允许客户配置终结点检测和响应设置。 但是，Azure 虚拟 WAN 产品中使用的虚拟机确实使用这些功能。 在引用的链接上了解有关这些常规功能的详细信息。 

- [Microsoft Defender 高级威胁防护概述](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [适用于 Windows Server 的 Microsoft Defender ATP 服务](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [适用于非 Windows Server 的 Microsoft Defender ATP 服务](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Azure 安全中心监视** ：是

**责任** ：共享

## <a name="governance-and-strategy"></a>治理和策略

*有关详细信息，请参阅 [Azure 安全基准：管理和策略](/azure/security/benchmarks/security-controls-v2-governance-strategy)。*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1：定义资产管理和数据保护策略 

**指南** ：确保为系统和数据的持续监视和保护记录并传达清晰的策略。 确定业务关键数据和系统的发现、评估、保护和监视优先级。 

此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 与业务风险相符的数据分类标准
- 安全组织对风险和资产清单的洞察力 
- 安全组织对 Azure 服务使用的审批 
- 资产在其生命周期中的安全性
- 与组织数据分类相符的必需访问控制策略
- 使用 Azure 原生的和第三方的数据保护功能
- 传输中数据用例和静态数据用例的数据加密要求
- 合适的加密标准

查看引用的链接上提供的其他信息。

- [Azure 安全体系结构建议 - 存储、数据和加密](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure 安全基础知识 - Azure 数据安全、加密和存储](../security/fundamentals/encryption-overview.md)

- [云采用框架 - Azure 数据安全和加密最佳做法](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2：定义企业分段策略 

**指南** ：建立企业范围的策略，以便使用标识、网络、应用程序、订阅、管理组和其他控件的组合来细分对资产的访问。 仔细权衡安全分离需求与为需要彼此通信并访问数据的系统启用日常操作的需求。

确保跨控件类型（包括网络安全、标识和访问模型、应用程序权限或访问模型以及人机处理控件）一致地实现分段策略。

- [有关 Azure 中的分段策略的指南（视频）](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [有关 Azure 中的分段策略的指南（文档）](/security/compass/governance#enterprise-segmentation-strategy)

- [使网络分段与企业分段策略相匹配](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3：定义安全状况管理策略

**指南** ：持续衡量和缓解你的个人资产以及托管这些资产的环境的风险。 确定高价值资产和高度暴露的攻击面，例如已发布的应用程序、网络入口和出口点、用户和管理员终结点等。

- [Azure 安全基准 - 状况和漏洞管理](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4：协调组织角色、职责和责任

**指南** ：确保为安全组织中的角色和责任记录并传达清晰的策略。 根据安全决策的明确责任，为你的工作设置优先级，为每个人提供共享责任模型的培训，并向技术团队培训用于保护云安全的技术团队。

- [Azure 安全最佳做法 1-人员：教育团队开展云安全旅程](https://aka.ms/AzSec1)

- [Azure 安全最佳做法 2 - 人员：针对云安全技术培训团队](https://aka.ms/AzSec2)

- [Azure 安全最佳做法 3 - 流程：针对云安全决策分配责任](https://aka.ms/AzSec3)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-5-define-network-security-strategy"></a>GS-5：定义网络安全策略

**指南** ：建立 Azure 网络安全方法，作为组织整体安全访问控制策略的一部分。 此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 集中化的网络管理和安全职责
- 符合企业分段策略的虚拟网络分段模型
- 各种威胁和攻击场景中的补救策略
- Internet 边缘及入口和出口策略
- 混合云和本地互连策略
- 最新的网络安全项目（例如网络关系图、参考网络体系结构）

查看引用的链接上提供的其他信息。

- [Azure 安全最佳做法 11 - 体系结构。单一的统一安全策略](https://aka.ms/AzSec11)

- [Azure 安全基准 - 网络安全](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure 网络安全概述](../security/fundamentals/network-overview.md)

- [企业网络体系结构策略](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6：定义标识和特权访问策略

**指南** ：建立 Azure 标识和特权访问方法，作为组织整体安全访问控制策略的一部分。 此策略应包括针对以下元素的记录在案的指南、策略和标准： 

- 集中化的标识和身份验证系统及其与其他内部和外部标识系统的互连
- 各种用例和条件中的强身份验证方法
- 保护权限高的用户
- 异常用户活动监视和处理  
- 用户标识和访问评审及协调流程

查看引用的链接上提供的其他信息。

- [Azure 安全基准 - 标识管理](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure 安全基准 - 特权访问](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure 安全最佳做法 11 - 体系结构。单一的统一安全策略](https://aka.ms/AzSec11)

- [Azure 标识管理安全概述](../security/fundamentals/identity-management-overview.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7：定义日志记录和威胁响应策略

**指南** ：建立日志记录和威胁响应策略，以快速检测和修正威胁，同时满足合规性要求。 优先为分析师提供高质量警报和无缝体验，以便他们能够专注于威胁，而不是执行集成和手动步骤。 

此策略应包含以下元素的文档指南、策略和标准： 

- 安全运营 (SecOps) 组织的角色和职责
- 符合 NIST 或其他行业框架要求的明确定义的事件响应流程
- 日志捕获和保留，用于支持威胁检测、事件响应和合规性需求
- 集中查看和关联有关威胁的信息，使用 SIEM、本机 Azure 功能和其他源-与客户、供应商和感兴趣的各方之间的通信和通知计划
- 使用 Azure 原生的和第三方的平台进行事件处理，例如日志记录和威胁检测、取证以及攻击补救和根除
- 处理事件和事件后活动的流程，例如经验教训和证据保留

查看引用的链接上提供的其他信息。
- [Azure 安全基准 - 日志记录和威胁检测](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure 安全基准 - 事件响应](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure 安全最佳做法 4 - 流程。更新云的事件响应流程](https://aka.ms/AzSec4)

- [Azure 采用框架、日志记录和报告决策指南](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

## <a name="next-steps"></a>后续步骤

- 请参阅 [Azure 安全基准 V2 概述](/azure/security/benchmarks/overview)
- 详细了解 [Azure 安全基线](/azure/security/benchmarks/security-baselines-overview)
