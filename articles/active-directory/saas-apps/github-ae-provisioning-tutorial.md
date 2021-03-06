---
title: 教程：配置 GitHub AE 以便通过 Azure Active Directory 进行自动用户预配 |Microsoft Docs
description: 了解如何从 Azure AD 自动预配和取消预配用户帐户。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d9818c05-e279-45b4-8aad-0fa156abd74e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2020
ms.author: Zhchia
ms.openlocfilehash: c1a20f35884e03d5fae53e5465e96c275a39131e
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94593342"
---
# <a name="tutorial-configure-github-ae-for-automatic-user-provisioning"></a>教程：针对自动用户预配配置 GitHub 自动曝光

本教程介绍了需要在 GitHub 自动曝光和 Azure Active Directory (Azure AD) 中执行的步骤，以配置自动用户预配。 配置后，Azure AD 使用 Azure AD 预配服务自动预配用户和/或组，并将其预配到 GitHub 自动曝光。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 GitHub 自动曝光中创建用户
> * 如果用户不需要访问权限，请删除 GitHub 自动曝光中的用户
> * 使用户属性在 Azure AD 和 GitHub 自动曝光之间保持同步
> * 单一登录到 Github 自动 [曝光](https://docs.microsoft.com/azure/active-directory/saas-apps/github-ae-tutorial) (建议) 

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 中[有权](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)配置预配的用户帐户（例如应用管理员、云应用管理员、应用所有者或全局管理员）。 
* GitHub 自动曝光，已完全 [初始化](https://docs.github.com/github-ae@latest/admin/configuration/initializing-github-ae) 并配置为通过 Azure AD 租户通过 [SAML SSO](https://docs.github.com/github-ae@latest/admin/authentication/configuring-authentication-and-provisioning-for-your-enterprise-using-azure-ad) 登录。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
3. 确定要 [在 Azure AD 和 GitHub 自动曝光之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。 

## <a name="step-2-configure-github-ae-to-support-provisioning-with-azure-ad"></a>步骤 2. 配置 GitHub 自动曝光以支持 Azure AD 的预配

了解如何 [在此处](https://docs.github.com/github-ae@latest/admin/authentication/configuring-user-provisioning-for-your-enterprise)启用 GitHub AE 的预配。

## <a name="step-3-add-github-ae-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 GitHub 自动曝光

从 Azure AD 应用程序库中添加 GitHub 自动曝光，开始管理预配到 GitHub 自动曝光。 如果你以前为 SSO 设置 GitHub AE，则可以使用相同的应用程序。 但建议你在最初测试集成时创建一个单独的应用。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)详细了解如何从库中添加应用程序。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

Azure AD 预配服务允许你确定将根据分配给应用程序的人员，或基于用户和/或组的属性进行预配的用户的范围。 如果选择将根据分配预配到你的应用的用户的范围，则可以使用以下 [步骤](../manage-apps/assign-user-or-group-access-portal.md) 将用户和/或组分配到应用程序。 如果选择仅基于用户和/或组的属性设置的用户的作用域，则可以使用 [此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户分配到 GitHub 自动曝光时，必须选择 "默认" **访问权限** 以外的其他角色。 具有“默认访问”角色的用户将从预配中排除，并在预配日志中被标记为未有效授权。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)以添加其他角色。 

* 先小部分测试。 使用少量的用户和/或组进行测试，然后再向所有人推出。 如果设置的作用域设置为 "分配的用户和/或组"，则可以通过将一个或两个用户和/或组分配到应用来对此进行控制。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-github-ae"></a>步骤 5。 配置 GitHub 自动曝光的自动用户预配 

本部分介绍了如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-github-ae-in-azure-ad"></a>若要在 Azure AD 中配置 GitHub AE 的自动用户预配，请执行以下操作：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择 " **GitHub 自动曝光**"。

    ![应用程序列表中的 GitHub 自动曝光链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![“预配”选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![自动设置选项卡](common/provisioning-automatic.png)

5. 在 " **管理员凭据** " 部分下，输入你在步骤2之前检索到的 GITHUB 自动曝光 **租户 URL** 和 **机密令牌** 。 单击 " **测试连接** " 以确保 Azure AD 可以连接到 GITHUB 自动曝光。 如果连接失败，请确保 GitHub 自动曝光帐户具有管理员权限，然后重试。

    ![令牌](common/provisioning-testconnection-tenanturltoken.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 " **映射** " 部分下，选择 "将 **Azure Active Directory 用户同步** 到 **GitHub 自动曝光**"。

9. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 GitHub AE 的用户属性。 选为 " **匹配** " 属性的属性用于匹配 GitHub AE 中的用户帐户以执行更新操作。 如果选择更改 [匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保 GITHUB 自动曝光 API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|
   |---|---|
   |userName|字符串|
   |externalId|字符串|
   |emails[type eq "work"].value|字符串|
   |活动|Boolean|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |name.formatted|字符串|
   |displayName|字符串|

10. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

11. 若要为 GitHub 自动曝光启用 Azure AD 预配服务，请在 "**设置**" 部分中将设置 **状态** 更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

12. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 GitHub 自动曝光的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

13. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作将启动 "**设置**" 部分的 "**范围**" 中定义的所有用户和/或组的初始同步循环。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)了解有关隔离状态的详细信息。  

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)
