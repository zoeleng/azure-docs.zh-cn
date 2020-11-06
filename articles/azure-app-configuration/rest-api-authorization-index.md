---
title: Azure 应用配置 REST API-授权
description: 使用 Azure 应用配置的授权参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 54f9cfab1f49837a3765c978de6deeb9e5e7d644
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423938"
---
# <a name="authorization"></a>授权

授权是指用于确定调用方在发出请求时所具有的权限的过程。 有多个授权模型。 用于请求的授权模型取决于所使用的 [身份验证](./rest-api-authentication-index.md) 方法。 授权模型如下所示。

## <a name="hmac"></a>HMAC

与 HMAC 身份验证关联的 [授权模型](./rest-api-authorization-hmac.md) 模型将权限拆分为只读或读写模式。 有关详细信息，请参阅 [HMAC 授权](./rest-api-authorization-hmac.md) 页。

## <a name="azure-active-directory"></a>Azure Active Directory

与 Azure Active Directory (Azure AD) authentication 关联的 [授权模型](./rest-api-authorization-azure-ad.md) 使用 Azure RBAC 来控制权限。 有关详细信息，请参阅 [Azure AD 授权](./rest-api-authorization-azure-ad.md) 页。
