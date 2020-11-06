---
title: Azure Active Directory REST API 身份验证
description: 使用 Azure Active Directory 通过 REST API 对 Azure 应用配置进行身份验证
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: fb3d00fb79c55e29d578f5e068e4ae025414a935
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423945"
---
# <a name="azure-active-directory-authentication"></a>Azure Active Directory 身份验证

可以使用 **持有** 者身份验证方案对 HTTP 请求进行身份验证，并通过 Azure Active Directory (Azure AD) 获取令牌。 必须通过 TLS 传输这些请求。

## <a name="prerequisites"></a>先决条件

必须将用于请求 Azure AD 令牌的主体分配给适用的[应用配置角色](./rest-api-authorization-azure-ad.md)之一

为每个请求提供身份验证所需的所有 HTTP 标头。 要求的最小值为：

|  请求标头 | 说明  |
| --------------- | ------------ |
| **授权** | **持有** 者方案所需的身份验证信息。 格式和详细信息如下所述。 |

**示例：**

```http
Host: {myconfig}.azconfig.io
Authorization: Bearer {{AadToken}}
```

## <a name="azure-active-directory-token-acquisition"></a>Azure Active Directory 令牌采集

在获取 Azure AD 令牌之前，必须先确定要进行身份验证的用户、他们请求令牌所使用的用户以及应使用哪些 Azure AD 终结点 (颁发) 机构。

### <a name="audience"></a>读者

必须使用适当的受众请求 Azure AD 令牌。 对于 Azure 应用配置，请求令牌时应指定以下受众之一。 还可以将受众称为 "资源"，请求令牌的 "资源"。

- {configurationStoreName}. azconfig
- *. azconfig.io

> [!IMPORTANT]
> 当请求的受众为 {configurationStoreName}. azconfig 时，它必须与) 用于发送请求 (区分大小写的 "Host" 请求标头完全匹配。

### <a name="azure-ad-authority"></a>Azure AD 颁发机构

Azure AD 机构是用于获取 Azure AD 令牌的终结点。 其形式为 `https://login.microsoftonline.com/{tenantId}` 。 `{tenantId}`该段指的是尝试进行身份验证的用户/应用程序所属的 Azure Active Directory 租户 ID。

### <a name="authentication-libraries"></a>身份验证库

Azure 提供一组称为 Azure Active Directory 身份验证库 (ADAL) ，以简化获取 Azure AD 令牌的过程。 这些库是针对多种语言构建的。 可在 [此处](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)找到相关文档。

## <a name="errors"></a>**错误**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer
```

**原因：** 未提供授权请求标头和持有者方案。
**解决方案：** 提供有效的 ```Authorization``` HTTP 请求标头

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="Authorization token failed validation"
```

**原因：** Azure AD 标记无效。
**解决方案：** 从 Azure AD 机构获取 Azure AD 令牌，并确保使用适当的受众。

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="The access token is from the wrong issuer. It must match the AD tenant associated with the subscription to which the configuration store belongs. If you just transferred your subscription and see this error message, please try back later."
```

**原因：** Azure AD 标记无效。
**解决方案：** 从 Azure AD 机构获取 Azure AD 令牌，并确保 Azure AD 租户与配置存储所属的订阅关联。 如果主体属于多个 Azure AD 租户，则可能出现此错误。
