---
title: Azure 应用配置 REST API-Azure Active Directory 授权
description: 使用 Azure Active Directory 对使用 REST API 的 Azure 应用配置进行授权
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: bebab7c06062726f7b5c7868f984cadda3b4c98e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423941"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Azure Active Directory 授权-REST API 参考

如果使用 Azure Active Directory (Azure AD) 身份验证，则会通过 Azure 基于角色的访问控制 (RBAC) 来处理授权。 Azure RBAC 要求将用户分配到角色，以便授予对资源的访问权限。 每个角色都包含一组分配给角色的用户能够执行的操作。

## <a name="roles"></a>角色

以下角色是 Azure 订阅中默认情况下可用的内置角色：

- **Azure 应用配置数据所有者** ：此角色提供对所有操作的完全访问权限。
- **Azure 应用配置数据读取器** ：此角色启用读取操作。

## <a name="actions"></a>操作

角色包含分配给该角色的用户可执行的操作的列表。 Azure 应用配置支持以下操作：

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`：此操作允许对应用配置键值资源（例如/kv 和/labels.）进行读取访问
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`：此操作允许对应用配置键值资源进行写入访问。
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`：此操作允许删除应用配置键值资源。 请注意，删除资源将返回已删除的键-值。

## <a name="errors"></a>错误

```http
HTTP/1.1 403 Forbidden
```

**原因：** 发出请求的主体没有执行请求的操作所需的权限。
**解决方案：** 将执行请求的操作所需的角色分配给发出请求的主体。

## <a name="managing-role-assignments"></a>管理角色分配

使用在所有 Azure 服务中标准的 [AZURE RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview) 过程来管理角色分配。 可以通过 Azure CLI、PowerShell、Azure 门户等来实现此目的。 [此处](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)提供有关如何进行角色分配的官方文档。
