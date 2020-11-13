---
title: 添加应用角色并从令牌获取它们 | Azure
titleSuffix: Microsoft identity platform
description: 了解如何将应用程序角色添加到 Azure Active Directory 中注册的应用程序，将用户和组分配到这些角色，并在令牌的 "roles" 声明中接收这些角色。
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 11/13/2020
ms.author: kkrishna
ms.reviewer: marsma, kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 96c52c46a75d6d5810dfddf91439c275d14e85f1
ms.sourcegitcommit: 9706bee6962f673f14c2dc9366fde59012549649
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94616123"
---
# <a name="how-to-add-app-roles-to-your-application-and-receive-them-in-the-token"></a>如何：向应用程序添加应用程序角色并在令牌中接收这些角色

基于角色的访问控制 (RBAC) 是一种常用的机制，用于在应用程序中强制进行授权。 使用 RBAC 时，管理员将权限授予角色而不是单个用户或组。 然后，管理员再将角色分配给不同的用户和组，以便控制用户对特定内容和功能的访问。

使用 RBAC 和应用程序角色和角色声明，开发人员可以更轻松地在应用程序中安全地实施授权。

另一种方法是使用 Azure AD 组和组声明，如 GitHub 上的 [active-aspnetcore-webapp-openidconnect-v2](https://aka.ms/groupssample) 代码示例中所示。 Azure AD 组和应用程序角色不相互排斥;它们可一起使用，以提供更精细的访问控制。

## <a name="declare-roles-for-an-application"></a>为应用程序声明角色

使用 [Azure 门户](https://portal.azure.com)定义应用程序角色。 当用户登录到应用程序时，Azure AD `roles` 会为每个角色发出一个声明，指出已向用户及其组成员身份单独授予了用户。

可以通过两种方法使用 Azure 门户来声明应用角色：

* [应用角色 UI](#app-roles-ui--preview) |预览
* [应用程序清单编辑器](#app-manifest-editor)

添加的角色数将计入 Azure Active Directory 强制实施的应用程序清单限制。 有关这些限制的详细信息，请参阅[Azure Active Directory 应用程序清单参考](reference-app-manifest.md)中的 "[清单限制](./reference-app-manifest.md#manifest-limits)" 部分。

### <a name="app-roles-ui--preview"></a>应用角色 UI |预览

> [!IMPORTANT]
> 应用角色门户 UI 功能 [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

使用 Azure 门户的用户界面创建应用角色：

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 在顶部菜单中选择 " **目录 + 订阅** " 筛选器，然后选择包含要向其添加应用程序角色的应用注册的 Azure Active Directory 租户。
1. 搜索并选择“Azure Active Directory”  。
1. 在 " **管理** " 下，选择 " **应用注册** "，然后选择要在其中定义应用角色的应用程序。
1. 选择 **应用角色 |预览** "，然后选择" **创建应用角色** "。

   :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-overview-pane.png" alt-text="Azure 门户中的应用注册应用程序角色窗格":::
1. 在 " **创建应用角色** " 窗格中，输入角色的设置。 图像后的表描述了每个设置及其参数。

    :::image type="content" source="media/howto-add-app-roles-in-azure-ad-apps/app-roles-create-context-pane.png" alt-text="应用注册的应用角色在 Azure 门户中创建上下文窗格":::

    | 字段 | 说明 | 示例 |
    |-------|-------------|---------|
    | **显示名称** | 显示在管理员同意和应用程序分配体验中的应用角色的显示名称。 此值可以包含空格。 | `Survey Writer` |
    | **允许的成员类型** | 指定是否可以将此应用角色分配给用户、应用程序或两者。<br/><br/>如果可用于 `applications` ，应用角色将在应用注册的 " **管理** " 部分中显示为应用程序权限，> **Api 权限 > > api 中添加权限 > 选择 api > 应用程序权限** 。 | `Users/Groups` |
    | **值** | 指定应用程序在令牌中应具有的角色声明的值。 该值应与应用程序代码中引用的字符串完全匹配。 该值不能包含空格。 | `Survey.Create` |
    | **说明** | 在管理应用分配和许可体验过程中显示的应用角色的更详细说明。 | `Writers can create surveys.` |
    | **是否要启用此应用角色？** | 指定是否启用应用角色。 若要删除应用角色，请取消选择此复选框，并在尝试删除操作之前应用更改。 | *Checked* |

1. 选择“应用”以保存所做的更改。

### <a name="app-manifest-editor"></a>应用程序清单编辑器

若要通过直接编辑清单来添加角色：

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 在顶部菜单中选择 " **目录 + 订阅** " 筛选器，然后选择包含要向其添加应用程序角色的应用注册的 Azure Active Directory 租户。
1. 搜索并选择“Azure Active Directory”  。
1. 在 " **管理** " 下，选择 " **应用注册** "，然后选择要在其中定义应用角色的应用程序。
1. 再次在 " **管理** " 下选择 " **清单** "。
1. 通过查找 `appRoles` 设置并添加应用程序角色来编辑应用程序清单。 您可以定义面向或的应用程序角色 `users` `applications` 。 下面的 JSON 代码段显示这两种情况的示例。
1. 保存清单。

清单中的每个应用角色定义的值都必须具有唯一的 GUID `id` 。

每个应用角色定义的 `value` 属性应该与应用程序的代码中使用的字符串完全匹配。 `value` 属性不能包含空格。 如果包含空格，则在保存清单时会收到错误。

#### <a name="example-user-app-role"></a>示例：用户应用角色

此示例定义了一个名为的应用程序角色 `Writer` ，可将其分配给 `User` ：

```json
"appId": "8763f1c4-0000-0000-0000-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-0000-0000-0000-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

#### <a name="example-application-app-role"></a>示例：应用程序应用程序角色

如果可用于 `applications` ，应用角色将在应用注册的 " **管理** " 部分中显示为应用程序权限，> **Api 权限 > > api 中添加权限 > 选择 api > 应用程序权限** 。

此示例显示了一个以为目标的应用程序角色 `Application` ：

```json
"appId": "8763f1c4-0000-0000-0000-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-0000-0000-0000-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

## <a name="assign-users-and-groups-to-roles"></a>将用户和组分配到角色

在应用程序中添加应用角色后，可以将用户和组分配给角色。 可以通过门户的 UI 为用户或组分配角色，也可以使用 [Microsoft Graph](/graph/api/user-post-approleassignments) 以编程方式进行分配。 当分配到各种应用角色的用户登录到应用程序时，其令牌会在 `roles` 声明中具有其分配的角色。

使用 Azure 门户将用户和组分配给角色：

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 在 **Azure Active Directory** 的左侧导航菜单中，选择 " **企业应用程序** "。
1. 选择“所有应用程序”，查看所有应用程序的列表。 如果你的应用程序未显示在列表中，请使用 " **所有应用程序** " 列表顶部的筛选器限制列表，或向下滚动列表以找到你的应用程序。
1. 选择一个应用程序，以便在其中为角色分配用户或安全组。
1. 在“管理”下，选择“用户和组”。
1. 选择 " **添加用户** " 以打开 " **添加分配** " 窗格。
1. 在“添加分配”窗格中，选择“用户和组”选择器。 将显示用户和安全组的列表。 可以搜索特定的用户或组，并选择列表中显示的多个用户和组。
1. 选择用户和组后，选择 " **选择** " 按钮以继续。
1. 选择 " **添加分配** " 窗格中的 " **选择角色** "。 将显示已为应用程序定义的所有角色。
1. 选择一个角色并选择 " **选择** " 按钮。
1. 选择 " **分配** " 按钮，完成将用户和组分配到应用的操作。

确认你添加的用户和组出现在 " **用户和组** " 列表中。

## <a name="assign-app-roles-to-applications"></a>向应用程序分配应用程序角色

在应用程序中添加应用程序角色后，可以通过使用 Azure 门户或使用 [Microsoft Graph](/graph/api/user-post-approleassignments)以编程方式将应用程序角色分配给客户端应用程序。

将应用程序角色分配到应用程序时，将创建 *应用程序权限* 。 应用程序权限通常由守护程序应用或后端服务使用，需要对其进行身份验证和授权的 API 调用，而无需用户交互。

使用 Azure 门户将应用程序角色分配给应用程序：

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 在 **Azure Active Directory** 中，在左侧导航菜单中选择 " **应用注册** "。
1. 选择“所有应用程序”，查看所有应用程序的列表。 如果你的应用程序未显示在列表中，请使用 " **所有应用程序** " 列表顶部的筛选器限制列表，或向下滚动列表以找到你的应用程序。
1. 选择要向其分配应用程序角色的应用程序。
1. 选择“API 权限” > “添加权限” 。
1. 选择 " **我的 api** " 选项卡，然后选择您为其定义了应用程序角色的应用程序。
1. 选择“应用程序权限”。
1. 选择要分配)  (角色。
1. 选择 " **添加权限** " 按钮完成角色 () 的添加。

新添加的角色应显示在应用注册的 **API 权限** 窗格中。

#### <a name="grant-admin-consent"></a>授予管理员许可

由于这些是 *应用程序权限* ，而不是委托权限，管理员必须授予许可才能使用分配给应用程序的应用角色。

1. 在应用注册的 " **API 权限** " 窗格中，选择 " **授予 \<tenant name\> 管理员许可** "。
1. 出现提示时，请选择 **"是"** 以授予请求的权限。

" **状态** " 列应反映已 **\<tenant name\> 授予** 同意。

## <a name="use-app-roles-in-your-web-api"></a>在 web API 中使用应用角色

定义应用角色并将其分配给用户、组或应用程序后，下一步是将代码添加到 web API，以便在调用 API 时检查这些角色。 也就是说，当客户端应用请求你确定的 API 操作需要授权时，API 的代码必须验证作用域是否位于客户端应用调用中提供的访问令牌中。

若要了解如何将授权添加到 web API，请参阅 [受保护的 WEB api：验证范围和应用角色](scenario-protected-web-api-verification-scope-app-roles.md)。

## <a name="app-roles-vs-groups"></a>应用角色与组

尽管可以使用应用角色或组进行授权，但是它们之间的主要差异可能会影响你决定使用哪些方案。

| 应用角色                                                                          | 组                                                      |
|------------------------------------------------------------------------------------|-------------------------------------------------------------|
| 它们是特定于应用程序的，并在应用程序注册中定义。 它们会随应用程序一起移动。 | 它们不特定于应用，而是 Azure AD 租户。 |
| 删除应用程序时，将删除应用程序角色。                      | 即使删除了应用，组也会保持不变。            |
| 在声明中提供 `roles` 。                                                     | 在声明中提供 `groups` 。                                 |

开发人员可以使用应用程序角色来控制用户是否可以登录到应用程序，或者应用程序是否可以获取 web API 的访问令牌。 若要将此安全控制扩展到组，开发人员和管理员还可以将安全组分配给应用程序角色。

当开发人员想要在自己的应用程序中描述和控制授权的参数时，开发人员首选应用程序角色。 例如，使用组进行授权的应用将在下一个租户中中断，因为组 ID 和名称可能不同。 使用应用程序角色的应用程序将保持安全。 事实上，出于相同的原因，将组分配到应用角色对于 SaaS 应用而言很常见。

## <a name="next-steps"></a>后续步骤

详细了解包含以下资源的应用角色。

* GitHub 上的代码示例
  * [使用组和组声明将授权添加到 ASP.NET Core web 应用](https://aka.ms/groupssample)
  * [角度单页应用程序 (SPA) 调用 .NET Core web API 并使用应用程序角色和安全组](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-dotnetcore-webapi-roles-groups/blob/master/README.md)
* 参考文档
  * [Azure AD 应用程序清单](./reference-app-manifest.md)
  * [Azure AD 访问令牌](access-tokens.md)
  * [Azure AD ID 令牌](id-tokens.md)
  * [向应用程序提供可选声明](active-directory-optional-claims.md)
* 视频： [在应用程序中通过 Microsoft 标识平台实现授权](https://www.youtube.com/watch?v=LRoc-na27l0) (1:01:15) 
