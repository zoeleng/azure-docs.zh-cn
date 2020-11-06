---
title: Azure 应用配置 REST API-身份验证
description: 使用 Azure 应用配置的身份验证的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 21a43a005b78c8916d06e97ca9d2ba21d5a585a3
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423976"
---
# <a name="authentication"></a>身份验证

所有 HTTP 请求都必须经过身份验证。 支持以下身份验证方案。

## <a name="hmac"></a>HMAC

[HMAC 身份验证](./rest-api-authentication-hmac.md) 使用随机生成的机密来签署请求有效负载。 有关如何授权使用此身份验证方法的请求的详细信息，请参阅 [HMAC 授权](./rest-api-authorization-hmac.md) 部分。

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory (Azure AD) 身份验证](/azure/active-directory/authentication/overview-authentication) 利用从 Azure Active Directory 获取的持有者令牌对请求进行身份验证。 有关如何授权使用此身份验证方法的请求的详细信息，请参阅 [Azure AD 授权](./rest-api-authorization-azure-ad.md) 部分。
