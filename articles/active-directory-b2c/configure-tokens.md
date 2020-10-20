---
title: 配置令牌 - Azure Active Directory B2C | Microsoft Docs
description: 了解如何在 Azure Active Directory B2C 中配置令牌生存期和兼容性设置。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10f86d194c5c70f2de0ab6502893e228800b1bdf
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92215461"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中配置令牌

在本文汇总，你将了解如何在 Azure Active Directory B2C (Azure AD B2C) 中配置[令牌的生存期和兼容性](tokens-overview.md)。

## <a name="prerequisites"></a>先决条件

[创建用户流](tutorial-create-user-flows.md)，以便用户能够注册并登录应用程序。

## <a name="configure-jwt-token-lifetime"></a>配置 JWT 令牌生存期

可以在任何用户流上配置令牌生存期。

1. 登录 [Azure 门户](https://portal.azure.com)。
2. 确保你正在使用包含 Azure AD B2C 租户的目录。 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含 Azure AD B2C 租户的目录。
3. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“Azure AD B2C” 。
4. 选择“用户流(策略)”。
5. 打开先前创建的用户流。
6. 选择“属性”。
7. 在“令牌生存期”下，调整以下属性以满足应用程序的需要：

    ![Azure 门户中的令牌生存期属性设置](./media/configure-tokens/token-lifetime.png)

8. 单击“ **保存**”。

> [!NOTE]
> 使用授权代码流和 PKCE 的单页面应用程序的刷新令牌生存期始终为24小时。 [详细了解浏览器中刷新令牌的安全隐患](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser)。

## <a name="configure-jwt-token-compatibility"></a>配置 JWT 令牌兼容性

1. 选择“用户流(策略)”。
2. 打开之前创建的用户流。
3. 选择“属性”。
4. 在“令牌兼容性设置”下，调整以下属性以满足应用程序的需要：

    ![Azure 门户中的令牌兼容性属性设置](./media/configure-tokens/token-compatibility.png)

5. 单击“保存” 。

## <a name="next-steps"></a>后续步骤

详细了解如何[请求访问令牌](access-tokens.md)。



