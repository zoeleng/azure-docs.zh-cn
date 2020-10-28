---
title: 适用于 Site Recovery 的 Azure 安全基线
description: Site Recovery 安全基线为实现 Azure 安全基准中指定的安全建议提供过程指南和资源。
author: msmbaldwin
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 9ded8e989572d83b4761dfaaaa681505952b375d
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754348"
---
# <a name="azure-security-baseline-for-site-recovery"></a>适用于 Site Recovery 的 Azure 安全基线

此安全基线将 [Azure 安全基准版本 1.0](../security/benchmarks/overview-v1.md) 中的指南应用到 Site Recovery。 Azure 安全基准提供有关如何在 Azure 上保护云解决方案的建议。 内容由 Azure 安全基准定义的 **安全控制** 和适用于 Site Recovery 的相关指南进行分组。 排除了不适用于 Site Recovery 的 **控件** 。 

若要查看 Site Recovery 完全映射到 Azure 安全基准，请参阅 [完整的 Site Recovery 安全基线映射文件](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)。

## <a name="network-security"></a>网络安全

*有关详细信息，请参阅 [Azure 安全基准：网络安全](../security/benchmarks/security-control-network-security.md)。*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1：保护虚拟网络中的 Azure 资源

**指南** ： Microsoft Azure Site Recovery 不支持部署到 Azure 虚拟网络。 使用 Azure 专用终结点配置 Site Recovery 服务，以强制通过网络进行安全通信。

- [Azure Site Recovery 专用链接支持](azure-to-azure-how-to-enable-replication-private-endpoints.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：最大程度地降低网络安全规则的复杂性和管理开销

**指南** ： Site Recovery service 支持服务标记，这些标记允许客户仅向特定服务和端口打开流量。 客户必须允许其防火墙或网络安全组上的 "AzureSiteRecovery" 服务标记允许对 Site Recovery 服务的出站访问。

- [使用服务标记的出站连接](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="110-document-traffic-configuration-rules"></a>1.10：阐述流量配置规则

**指南** ：对网络安全组和与网络安全和流量流相关的其他资源使用资源标记。 对于单独的网络安全组规则，请使用 "说明" 字段记录允许进出网络流量的规则。 

合并与标记相关的任何内置 Azure 策略定义，如 "需要标记和值"，以确保所有资源都是用标记创建的，并向您通知现有未标记的资源。 

可使用 Azure PowerShell 或 Azure CLI，基于资源的标记查找资源或对其执行操作。 

- [如何创建和使用标记](/azure/azure-resource-manager/resource-group-using-tags) 

- [如何创建 Azure 虚拟网络](../virtual-network/quick-create-portal.md) 

- [如何使用网络安全组规则筛选网络流量](../virtual-network/tutorial-filter-network-traffic.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自动化工具来监视网络资源配置和检测更改

**指南** ：使用 Azure 活动日志监视与 Site Recovery 服务相关的网络资源配置的任何更改。 在 Azure Monitor 中创建警报，以便在关键 Site Recovery 网络资源发生更改时通知你。

- [查看和检索 Azure 活动日志事件](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [使用 Azure Monitor 创建、查看和管理活动日志警报](../azure-monitor/platform/alerts-activity-log.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="logging-and-monitoring"></a>日志记录和监视

*有关详细信息，请参阅 [Azure 安全基准：日志记录和监视](../security/benchmarks/security-control-logging-monitoring.md)。*

### <a name="22-configure-central-security-log-management"></a>2.2：配置中心安全日志管理

**指南** ：启用 Azure 活动日志诊断设置以进行审核日志记录，并将日志发送到 Log Analytics 工作区、azure 存储帐户或 Azure 事件中心进行存档。

使用 Azure 活动日志数据来确定对 Azure 资源执行任何写入操作 (PUT、POST、DELETE) 的 "内容、人员和时间"。

引入 Azure Monitor Site Recovery 日志以聚合生成的安全数据。 在 Azure Monitor 中，使用 Log Analytics 工作区查询和执行分析，并使用存储帐户进行长期或存档存储。 此外，还可以在 SIEM) 解决方案中启用数据并将其提供给 Azure Sentinel 或第三方安全事件和事件管理 (。

- [如何启用 Azure 活动日志的诊断设置](../azure-monitor/platform/activity-log.md)

- [使用 Azure Monitor 日志监视 Site Recovery](monitor-log-analytics.md)

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：为 Azure 资源启用审核日志记录

**指南** ：启用 Azure 活动日志诊断设置以进行审核日志记录，并将日志发送到 Log Analytics 工作区、azure 存储帐户或 Azure 事件中心进行存档。 

使用 Azure 活动日志数据来确定对 Azure 资源执行任何写入操作 (PUT、POST、DELETE) 的 "内容、人员和时间"。

引入 Site Recovery 日志，Azure Monitor 聚合生成的安全数据。 在 Azure Monitor 中，使用 Log Analytics 工作区查询和执行分析，并使用存储帐户进行长期/存档存储。 在 SIEM) 解决方案中，启用和数据到 Azure Sentinel 或第三方安全事件与事件管理 (。

- [如何启用 Azure 活动日志的诊断设置](../azure-monitor/platform/activity-log.md)

- [使用 Azure Monitor 日志监视 Site Recovery](monitor-log-analytics.md)

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="25-configure-security-log-storage-retention"></a>2.5：配置安全日志存储保留期

**指南** ：根据组织的符合性法规，使用 Azure Monitor 为与 Azure 恢复服务保管库关联的 Log Analytics 工作区设置日志保持期。 

- [如何设置日志保留参数](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="26-monitor-and-review-logs"></a>2.6：监视和审查日志

**指导** ：启用 Azure 活动日志诊断设置，并将日志发送到 Log Analytics 工作区。 

在 Log Analytics 中执行查询，以搜索术语、识别趋势、分析模式，以及有关从恢复服务保管库收集的活动日志数据的见解。

- [监视 Site Recovery](site-recovery-monitor-and-troubleshoot.md)

- [如何启用 Azure 活动日志的诊断设置](../azure-monitor/platform/activity-log.md)

- [如何收集和分析 Azure Monitor 的 Log Analytics 工作区中的 Azure 活动日志](../azure-monitor/platform/activity-log.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7：针对异常活动启用警报

**指南** ：通过使用 Azure Monitor 日志和 Log Analytics 来监视 Azure Site Recovery 复制的计算机。 使用 Azure Monitor 中的 Log Analytics 来编写和测试日志查询，并以交互方式分析日志数据。 Azure Monitor 收集活动和资源日志以及其他监视数据。 

可视化和查询日志结果，并配置警报以根据监视的数据采取措施。 将 Log Analytics 工作区上的警报设置为 Azure Sentinel，因为它提供了安全业务流程自动响应 (之忠诚度) 解决方案。 这允许自动解决方案，如要创建的行动手册，并用于修正安全问题。 使用 Azure Monitor 在 Log Analytics 工作区中创建自定义日志警报。 

- [监视 Site Recovery](site-recovery-monitor-and-troubleshoot.md)

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

- [使用 Azure Monitor 创建、查看和管理日志警报](../azure-monitor/platform/alerts-log.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="identity-and-access-control"></a>标识和访问控制

*有关详细信息，请参阅 [Azure 安全基准：标识和访问控制](../security/benchmarks/security-control-identity-access-control.md)。*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：维护管理帐户的清单

**指南** ：默认情况下不分配任何角色。 需要根据业务需求显式分配它们。 可以通过 PowerShell CLI 或 Azure Active Directory (Azure AD) 检查任何角色分配，以发现作为管理组成员的帐户。

- [如何使用 PowerShell 获取 Azure AD 中的目录角色](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [如何使用 PowerShell 获取 Azure AD 中目录角色的成员](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用专用管理帐户

**指南** ：围绕专用管理帐户的使用创建标准操作程序。 使用安全中心的标识和访问管理功能来监视管理帐户的数量。

此外，为了帮助你跟踪专用管理帐户，请使用安全中心或内置 Azure 策略中的建议，例如： 

- 应该为你的订阅分配了多个所有者 
- 应从订阅中删除拥有所有者权限的已弃用帐户 

- 应从订阅中删除拥有所有者权限的外部帐户

创建一个进程来跟踪管理帐户的标识和访问控制，并定期查看。

- [如何使用 Azure 安全中心监视标识和访问](../security-center/security-center-identity-access.md)

- [如何使用 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4：将单一登录 (SSO) 与 Azure Active Directory 配合使用

**指南** ：将 Azure 应用注册与服务主体一起使用，以检索用于通过 API 调用与恢复服务保管库进行交互的令牌。

- [如何调用 Azure REST API](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [如何将客户端应用程序（服务主体）注册到 Azure AD](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Azure 恢复服务 API 信息](/rest/api/recoveryservices)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：对所有基于 Azure Active Directory 的访问使用多重身份验证

**指南** ：启用 Azure AD 的多重身份验证，并遵循安全中心的标识和访问建议。 
- [计划 Azure 多重身份验证部署](../active-directory/authentication/howto-mfa-getstarted.md)

- [监视标识和访问](../security-center/security-center-identity-access.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6：对所有管理任务使用专用计算机（特权访问工作站）

**指南** ：使用安全的 Azure 托管的工作站 (也称为特权访问工作站 (PAW) # A3，其中包含用于管理任务的 Azure 多重身份验证，以及对 Site Recovery 资源执行特权操作。

- [特权访问工作站](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [规划基于云的 Azure 多重身份验证部署](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7：记录来自管理帐户的可疑活动并对其发出警报

**指南** ：在环境中发生可疑或不安全活动时，使用 Azure AD 的 PRIVILEGED IDENTITY MANAGEMENT (PIM) 功能来生成日志和警报。
查看警报和有关具有 Azure AD 风险检测功能的风险用户行为的报告。

- [如何部署 Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [了解 Azure AD 风险检测](../active-directory/identity-protection/overview-identity-protection.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="38-manage-azure-resources-only-from-approved-locations"></a>3.8：仅从批准的位置管理 Azure 资源

**指南** ：使用条件访问命名位置，以仅允许从 IP 地址范围、区域或国家/地区的特定逻辑分组访问 Azure 门户。
- [如何在 Azure 中配置命名位置](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指南** ：使用 Azure AD 作为 Site Recovery 的中央身份验证和授权系统。 Azure AD 通过对静态数据使用强加密，在传输过程中以及 salts、哈希和安全存储用户凭据来保护数据。 

- [如何创建和配置 Azure AD 实例](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期审查和协调用户访问

**指南** ：使用 Azure AD 日志来帮助发现陈旧的帐户。 

使用 Azure AD 的标识和访问评审有效地管理组成员身份、访问企业应用程序和角色分配。 

创建一个流程来定期查看用户访问权限，以确保只有具有已完成访问评审的用户才能继续访问。 

- [了解 Azure AD 报告](/azure/active-directory/reports-monitoring/)

- [如何使用 Azure 标识访问评审](../active-directory/governance/access-reviews-overview.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11：监视尝试访问已停用凭据的行为

**指南** ：将 Azure AD 用作 Site Recovery 资源的集中身份验证和授权系统。 Azure AD 通过对静态数据和传输中的数据使用强加密来保护数据，还可以 salts、哈希和安全地存储用户凭据。

你有权访问 Azure AD 登录活动、审核和风险事件日志源，它们允许你将它们与 azure 应用商店中提供的 Azure Sentinel 或任何 SIEM 或监视工具集成。

通过创建 Azure AD 用户帐户的诊断设置并将审核和登录日志发送到 Log Analytics 工作区，进一步简化了此过程。 可以在 Log Analytics 工作区中配置所需的警报。

- [如何将 Azure 活动日志集成到 Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12：针对帐户登录行为偏差发出警报

**指南** ：对于恢复服务保管库，使用 Azure AD 作为中心身份验证和授权系统。 

使用 Azure AD 的标识保护功能进行帐户登录行为检测，并为检测到的可疑操作配置自动响应，与用户标识相关。 还将数据引入 Azure Sentinel 以便进一步调查。

- [如何查看 Azure AD 风险登录](../active-directory/identity-protection/overview-identity-protection.md)

- [如何配置和启用标识保护风险策略](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [如何加入 Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="data-protection"></a>数据保护

*有关详细信息，请参阅 [Azure 安全基准：数据保护](../security/benchmarks/security-control-data-protection.md)。*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：维护敏感信息的清单

**指导** ：使用标记可以帮助跟踪存储或处理敏感信息的 Azure 资源。

- [如何创建和使用标记](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔离存储或处理敏感信息的系统

**指南** ：为开发、测试和生产恢复服务保管库实现单独的订阅或管理组。 使用虚拟网络或子网分隔资源，对其进行适当标记，并通过网络安全组或 Azure 防火墙进行保护。 

关闭未使用的、存储或处理敏感数据的虚拟机。 实施策略和过程，以使其成为重复性过程。 

- [如何创建其他 Azure 订阅](../cost-management-billing/manage/create-subscription.md)

- [如何创建管理组](../governance/management-groups/create-management-group-portal.md)

- [Site Recovery 概述](site-recovery-overview.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：监视和阻止未经授权的敏感信息传输

**指南** ：使用专用链接或专用终结点、网络安全组和服务标记，缓解 Site Recovery 启用的虚拟机的数据渗透的任何机会。

Microsoft 管理 Site Recovery 使用的基础平台，并将所有客户内容视为敏感数据，并防范客户数据丢失和公开。 Microsoft 实施并维护了一套强健的数据保护控制和功能，以确保 Azure 中的客户数据保持安全。 

- [了解 Azure 中的客户数据保护](../security/fundamentals/protection-customer-data.md)

- [将虚拟机复制到 Azure 专用终结点](azure-to-azure-how-to-enable-replication-private-endpoints.md)

- [将虚拟机与 Azure Site Recovery 服务标记一起复制](azure-to-azure-about-networking.md)

**Azure 安全中心监视** ：不适用

**责任** ：共享

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密传输中的所有敏感信息

**指南** ： Site Recovery 使用安全 https 通道，该通道使用高级加密标准 (AES 256) ，从 Azure 工作负荷服务器加密到在恢复服务保管库后托管的 Site Recovery 服务。

Site Recovery 支持的当前 TLS 版本为 TLS 1.0、TLS 1.1、TLS 1.2 in 区域，这些区域在2019结束时处于活动。 TLS 1.2 是适用于任何新区域的唯一受支持的 TLS 版本。

- [了解 Azure Site Recovery 传输的加密](physical-azure-set-up-source.md)

**Azure 安全中心监视** ：不适用

**责任** ：共享

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用有效的发现工具识别敏感数据

**指南** ：目前尚不 Site Recovery 支持数据标识、分类和丢失防护功能。 

根据需要实现第三方解决方案，以实现法规遵从性。

Microsoft 管理 Site Recovery 使用的基础平台，并将所有客户内容视为敏感数据，并防范客户数据丢失和公开。 它已经实现并维护一套可靠的数据保护控制和功能，以确保 Azure 中的客户数据保持安全。 

- [了解 Azure 中的客户数据保护](../security/fundamentals/protection-customer-data.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4.6：使用 Azure RBAC 管理对资源的访问

**指南** ：使用 azure RBAC)  (azure 基于角色的访问控制来管理对 Site Recovery 资源相关的数据和资源的访问权限。 

使用 Azure RBAC 分隔工作职责，并授予其所需的适当访问权限。 使用内置 Site Recovery 角色控制 Site Recovery 管理操作。

- [如何配置 Azure RBAC](../role-based-access-control/role-assignments-portal.md)

- [使用 Role-Based 访问控制来管理 Azure Site Recovery](site-recovery-role-based-linked-access-control.md)

**Azure 安全中心监视** ：目前不可用

**责任** ：客户

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：静态加密敏感信息

**指南** ：使用平台和客户托管的密钥启用双加密。 Site Recovery 中提供了此功能。 

Site Recovery 支持数据的静态加密。 对于 Azure IaaS 工作负荷，使用存储服务加密 (SSE) 静态加密数据。 

当使用使用客户管理的密钥加密的恢复服务保管库时，只有客户才能访问加密密钥。 Microsoft 从不维护副本，没有密钥的访问权限，也不会将从主站点传输的数据解密到灾难恢复位置。 

- [Azure Site Recovery 的客户托管密钥支持](azure-to-azure-how-to-enable-replication-cmk-disks.md)

**Azure 安全中心监视** ：不适用

**责任** ：共享

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：记录对关键 Azure 资源的更改并对此类更改发出警报

**指南** ：将 Azure Monitor 与 Azure 活动日志结合使用，以便在对关键资源进行更改时创建警报。 这些资源可能包括恢复服务保管库的生产实例、Site Recovery 服务和相关资源的资源。
- [如何针对 Azure 活动日志事件创建警报](../azure-monitor/platform/alerts-activity-log.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="inventory-and-asset-management"></a>清单和资产管理

*有关详细信息，请参阅 [Azure 安全基准：清单和资产管理](../security/benchmarks/security-control-inventory-asset-management.md)。*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1：使用自动化资产发现解决方案

**指南** ：使用 Azure 资源关系图可查询或发现订阅中的所有资源（包括恢复服务保管库）。 请确保租户中具有相应的读取权限，并枚举所有 Azure 订阅以及订阅中的资源。

尽管可以通过 Resource Graph 发现经典 Azure 资源，但我们强烈建议你今后还是创建并使用 Azure 资源管理器资源。

- [如何使用 Azure Graph 创建查询](../governance/resource-graph/first-query-portal.md)

- [如何查看 Azure 订阅](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [了解 Azure RBAC](../role-based-access-control/overview.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="62-maintain-asset-metadata"></a>6.2：维护资产元数据

**指南** ：将标记应用到恢复服务保管库和其他相关资源，这些资源由 Site Recovery 用于元数据，以逻辑方式将它们组织到分类。
- [如何创建和使用标记](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：删除未经授权的 Azure 资源

**指南** ：根据需要使用标记、管理组和单独的订阅来组织和跟踪 Site Recovery (恢复服务保管库) 和其他相关资源。 

此外，使用 Azure Policy 对可使用以下内置策略定义在客户订阅中创建的资源类型施加限制： 

- 不允许的资源类型
- 允许的资源类型

定期核对清单，确保及时地从订阅中删除未经授权的资源。

- [如何创建其他 Azure 订阅](../cost-management-billing/manage/create-subscription.md)

- [如何创建管理组](../governance/management-groups/create-management-group-portal.md)

- [如何创建和使用标记](../azure-resource-manager/management/tag-resources.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4：定义并维护已批准 Azure 资源的清单

**指南** ：根据客户的组织要求，为计算资源创建已批准的 Azure 资源和批准的软件的清单。

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：监视未批准的 Azure 资源

**指导** ：在 Azure Policy 中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制： 

- 不允许的资源类型 
- 允许的资源类型

使用 Azure 资源关系图可查询和发现订阅中的资源。

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [如何使用 Azure Resource Graph 创建查询](../governance/resource-graph/first-query-portal.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="69-use-only-approved-azure-services"></a>6.9：仅使用已批准的 Azure 服务

**指导** ：在 Azure Policy 中使用以下内置策略定义，对可以在客户订阅中创建的资源类型施加限制：

- 不允许的资源类型 
- 允许的资源类型

了解如何在 Azure 中创建和管理策略对于保持与公司标准和服务级别协议的符合性来说非常重要。

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [如何使用 Azure Policy 拒绝特定的资源类型](/azure/governance/policy/samples)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11：限制用户与 Azure 资源管理器进行交互的能力

**指导** ：通过对“Microsoft Azure 管理”应用配置“阻止访问”，使用 Azure 条件访问来限制用户与 Azure 资源管理器交互的功能。 这可防止在高度安全的环境中创建和更改资源。

- [如何配置条件访问以阻止访问 Azure 资源管理器](../role-based-access-control/conditional-access-azure-management.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="secure-configuration"></a>安全配置

*有关详细信息，请参阅 [Azure 安全基准：安全配置](../security/benchmarks/security-control-secure-configuration.md)。*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：为所有 Azure 资源建立安全配置

**指导** ：使用 Azure Policy 为恢复服务保管库定义和实施标准安全配置。 

使用 "Microsoft.recoveryservices" 命名空间中的 Azure 策略别名创建自定义策略，以便审核或强制执行 Site Recovery 服务的恢复服务保管库资源的配置。
- [如何查看可用的 Azure Policy 别名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：维护安全的 Azure 资源配置

**指南** ：使用 azure 策略 [拒绝] 和 [部署（如果不存在）影响跨 Azure 资源强制实施安全设置。
- [了解 Azure Policy 效果](../governance/policy/concepts/effects.md)

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全存储 Azure 资源的配置

**指南** ：若要将自定义 Azure 策略定义用于恢复服务保管库和相关资源，请选择 "Azure Repos" 来安全地存储和管理你的代码。

- [如何在 Azure DevOps 中存储代码](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Azure Repos 文档](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7：部署 Azure 资源的配置管理工具

**指导** ：在“Microsoft.RecoveryServices”命名空间中使用内置的 Azure Policy 定义和 Azure Policy 别名创建自定义策略，以审核、强制实施系统配置并为其发出警报。 

另外，开发一个用于管理策略例外的流程和管道。

- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9：为 Azure 资源实施自动配置监视

**指导** ：在“Microsoft.RecoveryServices”命名空间中使用内置的 Azure Policy 定义和 Azure Policy 别名创建自定义策略，以审核、强制实施系统配置并为其发出警报。 

使用 Azure 策略 [audit]、[拒绝] 和 [部署（如果不存在]）效果自动强制执行 Azure 资源的配置。
- [如何配置和管理 Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全管理 Azure 机密

**指南** ：客户应管理与 azure Key vault 集成 Site Recovery 机密，同时为启用了 Azure 磁盘加密的虚拟机启用灾难恢复。 

- [如何创建密钥保管库](../key-vault/secrets/quick-create-portal.md)

- [如何向密钥保管库进行身份验证](../key-vault/general/authentication.md)

- [如何分配密钥保管库访问策略](../key-vault/general/assign-access-policy-portal.md)

- [如何使用 Site Recovery 为启用了 Azure 磁盘加密的虚拟机启用 DR](azure-to-azure-how-to-enable-replication-ade-vms.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全自动管理标识

**指南** ： Site Recovery 仅支持客户可以在恢复服务保管库上启用系统托管标识的系统管理的标识。 这种方法适用于灾难恢复产品中使用的资源，用于定义访问边界。 

使用托管标识在 Azure AD 中为 Azure 服务提供自动托管标识。 

使用托管标识可以向支持 Azure AD 身份验证的任何服务（包括 Key Vault）进行身份验证，无需在代码中放入任何凭据。

- [如何与 Azure 托管标识集成](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity?tabs=core2x)

- [如何在恢复服务保管库上启用系统托管标识](azure-to-azure-how-to-enable-replication-private-endpoints.md#enable-the-managed-identity-for-the-vault)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除意外的凭据透露

**指南** ：实施凭据扫描程序来识别代码中的凭据。 凭据扫描程序还会建议将发现的凭据转移到更安全的位置，例如 Azure Key Vault。

- [如何设置凭据扫描程序](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="malware-defense"></a>恶意软件防护

*有关详细信息，请参阅 [Azure 安全基准：恶意软件防护](../security/benchmarks/security-control-malware-defense.md)。*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：预先扫描要上传到非计算 Azure 资源的文件

**指南** ：在支持 Azure 服务的基础主机上启用了 Microsoft 反恶意软件 (例如 Site Recovery) ，但它不会在你的内容上运行。 预扫描任何要上传到非计算 Azure 资源（例如应用服务、Data Lake Storage 和 Blob 存储）的文件。

使用安全中心的数据服务威胁检测来检测已上传到存储帐户的恶意软件。

- [了解适用于 Azure 云服务和虚拟机的 Microsoft Antimalware](../security/fundamentals/antimalware.md)

- [了解 Azure 安全中心的数据服务威胁检测](/azure/security-center/threat-protection)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="data-recovery"></a>数据恢复

*有关详细信息，请参阅 [Azure 安全基准：数据恢复](../security/benchmarks/security-control-data-recovery.md)。*

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：执行完整系统备份，并备份客户管理的所有密钥

**指南** ： Site Recovery 在内部使用 Azure 存储帐户来维护灾难恢复解决方案的状态（由客户在其工作负荷上配置）。

Site Recovery 服务元数据使用以下类型的所有存储资源：读取访问异地冗余存储 (GRS) 。  (GRS 以上类型的存储帐户（如 RAGRS、拖-ZRS) 会将数据复制到离源) 数据主位置 (数百英里以外的次要区域，以便在中断期间继续为客户提供灾难恢复。

这超出了客户范围，Site Recovery 团队在内部处理。 客户可以在 Azure 中备份 Key Vault 密钥。

- [如何在 Azure 中备份密钥保管库密钥](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：验证所有备份，包括客户管理的密钥

**指南** ：定期测试已备份客户托管密钥的还原。

- [如何在 Azure 中还原密钥保管库密钥](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：确保保护备份和客户管理的密钥

**指南** ：通过 Azure 的基础结构即服务 (基于 IaaS) 的虚拟机，使用存储服务加密 () SSE 对数据进行静态加密。 在 Key Vault 中启用软删除，以防止意外或恶意删除密钥。

- [如何在密钥保管库中启用软删除](https://docs.microsoft.com/azure/storage/blobs/soft-delete-blob-overview?tabs=azure-portal)

**Azure 安全中心监视** ：是

**责任** ：客户

## <a name="incident-response"></a>事件响应

*有关详细信息，请参阅 [Azure 安全基准：事件响应](../security/benchmarks/security-control-incident-response.md)。*

### <a name="101-create-an-incident-response-guide"></a>10.1：创建事件响应指导

**指南** ：为组织制定事件响应指南。 

请确保有一些书面的事件响应计划，这些计划定义了人员的所有角色，以及事件处理的阶段，以及从检测到后事件检查的管理阶段。

- [如何在 Azure 安全中心配置工作流自动化](../security-center/security-center-planning-and-operations-guide.md)

- [关于建立自己的安全事件响应流程的指南](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [Microsoft 安全响应中心事件分析](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [客户还可以利用 NIST 的“计算机安全事件处理指南”来制定他们自己的事件响应计划](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：创建事件评分和优先级设定过程

**指南** ：根据安全中心分配的警报严重性确定应该首先调查的警报的优先级。 严重性取决于安全中心在发出警报时所依据的检测结果和分析结果的置信度，以及导致发出警报的活动的恶意企图的置信度。

将订阅明确标记 (例如，生产、非生产) 并创建一个命名系统，以便清晰地识别和分类 Azure 资源。

- [Azure 安全中心中的安全警报](../security-center/security-center-alerts-overview.md) 

- [使用标记整理 Azure 资源](/azure/azure-resource-manager/resource-group-using-tags)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="103-test-security-response-procedures"></a>10.3：测试安全响应过程

**指导** ：定期执行演练来测试系统的事件响应功能。 根据需要确定弱点和间隙并修订计划

- [请参阅 NIST 的发布指南，了解 IT 计划和功能的测试、培训和试验计划](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全事件联系人详细信息，并针对安全事件配置警报通知

**指南** ：如果 Microsoft 安全响应中心 (MSRC) 发现非法或未经授权的某方访问了客户的数据，Microsoft 将使用安全事件联系人信息与你取得联系。 

创建一个流程来查看事件、发布事件，以确保解决问题。

- [如何设置 Azure 安全中心安全联系人](../security-center/security-center-provide-security-contact-details.md)

**Azure 安全中心监视** ：是

**责任** ：客户

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：将安全警报整合到事件响应系统中

**指南** ：使用连续导出功能导出安全中心警报和建议。 使用连续导出可以手动导出或者持续导出警报和建议。 

根据需要使用安全中心数据连接器将警报流式传输到 Azure Sentinel。
- [如何配置连续导出](../security-center/continuous-export.md)

- [如何将警报流式传输到 Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自动响应安全警报

**指南** ：若要在安全警报和建议上通过 "逻辑应用" 自动触发响应，请使用安全中心中的工作流自动化功能。
- [如何配置工作流自动化和逻辑应用](../security-center/workflow-automation.md)

**Azure 安全中心监视** ：不适用

**责任** ：客户

## <a name="penetration-tests-and-red-team-exercises"></a>渗透测试和红队练习

*有关详细信息，请参阅 [Azure 安全基准：渗透测试和 Red 团队练习](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md)。*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1：定期对 Azure 资源执行渗透测试，确保修正所有发现的关键安全问题

指南：遵循 Microsoft Rules of Engagement 以确保渗透测试不违反 Microsoft 政策： https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

- [可在此处详细了解如何针对 Microsoft 托管云基础结构、服务和应用程序执行红队测试和实时站点渗透测试，以及 Microsoft 的相关策略](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 安全中心监视** ：不适用

**责任** ：共享

## <a name="next-steps"></a>后续步骤

- 请参阅 [Azure 安全基准 V2 概述](/azure/security/benchmarks/overview)
- 详细了解 [Azure 安全基线](/azure/security/benchmarks/security-baselines-overview)
