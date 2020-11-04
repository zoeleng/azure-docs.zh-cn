---
title: 审核和管理策略符合性
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 策略将内置策略用于 Azure 机器学习，确保你的工作区符合你的要求。
author: jhirono
ms.author: jhirono
ms.date: 09/15/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: 47a97bb8fbe52d3dde84afd77997b179a5c9248d
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325504"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>使用 Azure Policy 审核和管理 Azure 机器学习

[Azure Policy](../governance/policy/index.yml) 是一种管理工具，你可用它来确保 Azure 资源符合你的策略。 通过 Azure 机器学习，你可分配以下策略：

* **客户管理的密钥** ：审核或强制执行工作区是否必须使用客户管理的密钥。
* **专用链接** ：审核工作区是否使用专用终结点与虚拟网络进行通信。

可以在不同的范围（如订阅或资源组级别）内设置策略。 有关详细信息，请参阅 [Azure Policy 文档](../governance/policy/overview.md)。

## <a name="built-in-policies"></a>内置策略

Azure 机器学习提供了一组策略，可用于 Azure 机器学习的常见方案。 你可以将这些策略定义分配给现有订阅，也可以将它们作为基础来创建你自己的自定义定义。 有关 Azure 机器学习的内置策略的完整列表，请参阅 [Azure 机器学习的内置策略](../governance/policy/samples/built-in-policies.md#machine-learning)。

若要查看与 Azure 机器学习相关的内置策略定义，请使用以下步骤：

1. 在 [Azure 门户](https://portal.azure.com)中转到“Azure Policy”。
1. 选择“定义”。
1. 对于“类型”，请选择“内置”；对于“类别”，请选择“机器学习” 。

可在此选择策略定义以进行查看。 查看定义时，可使用“分配”链接将策略分配到某个特定范围，并配置策略的参数。 有关详细信息，请参阅[分配策略 - 门户](../governance/policy/assign-policy-portal.md)。

还可以使用 [Azure PowerShell](../governance/policy/assign-policy-powershell.md)、[Azure CLI](../governance/policy/assign-policy-azurecli.md) 和[模板](../governance/policy/assign-policy-template.md)来分配策略。

## <a name="workspaces-encryption-with-customer-managed-key"></a>使用客户管理的密钥对工作区进行加密

控制是否应使用客户管理的密钥 (CMK) 对工作区进行加密，或使用 Microsoft 托管的密钥来加密指标和元数据。 有关如何使用 CMK 的详细信息，请参阅企业安全性文章的 [Azure Cosmos DB](concept-enterprise-security.md#azure-cosmos-db) 部分。

若要配置此策略，请将 effect 参数设置为“审核”或“拒绝” 。 如果设置为“审核”，则无需 CMK 即可创建工作区，并且会在活动日志中创建警告事件。

如果策略设置为“拒绝”，则无法创建工作区，除非该策略指定了 CMK。 尝试在不使用 CMK 的情况下创建工作区会导致类似于 `Resource 'clustername' was disallowed by policy` 的错误，并且会在活动日志中创建一个错误。 策略标识符也作为此错误的一部分返回。

## <a name="workspaces-should-use-private-link"></a>工作区应使用专用链接

控制工作区是否应使用 Azure 专用链接来与 Azure 虚拟网络通信。 有关使用专用链接的详细信息，请参阅 [配置工作区的专用链接](how-to-configure-private-link.md)。

若要配置此策略，请将 effect 参数设置为 __audit__ 。 如果不使用 "专用" 链接创建工作区，则会在活动日志中创建一个警告事件。

## <a name="next-steps"></a>后续步骤

* [Azure Policy 文档](../governance/policy/overview.md)
* [Azure 机器学习的内置策略](policy-reference.md)
* [通过 Azure 安全中心使用安全策略](../security-center/tutorial-security-policy.md)