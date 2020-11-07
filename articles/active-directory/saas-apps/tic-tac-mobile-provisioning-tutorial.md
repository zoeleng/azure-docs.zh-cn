---
title: 教程：配置 Tic-Tac Mobile 以 Azure Active Directory 的自动用户预配 |Microsoft Docs
description: 了解如何自动预配和取消设置从 Azure AD 到 Tic-Tac Mobile 的用户帐户。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d0f24e81-fecf-4e71-bd8a-ab911366fdf5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/08/2020
ms.author: Zhchia
ms.openlocfilehash: 99fe2f8356b53b5d8e459dbf62534ddb4f0019b2
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357260"
---
# <a name="tutorial-configure-tic-tac-mobile-for-automatic-user-provisioning"></a>教程：为自动用户预配配置 Tic-Tac Mobile

本教程介绍 Tic-Tac 移动和 Azure Active Directory (Azure AD) 配置自动用户预配时需要执行的步骤。 配置后，Azure AD 使用 Azure AD 预配服务自动将用户和组预配和取消设置到 [票 Mobile](https://www.tictacmobile.com/) 。 有关此服务的作用、工作原理和常见问题的信息，请参阅 [使用 Azure AD 自动执行用户预配和取消预配到软件即服务 (SaaS) 应用程序](../manage-apps/user-provisioning.md)。


## <a name="capabilities-supported"></a>支持的功能

> [!div class="checklist"]
> * 在 Tic-Tac Mobile 中创建用户。
> * 如果用户不再需要访问，请将其从 Tic-Tac 移动中删除。
> * 使用户属性在 Azure AD 和 Tic-Tac Mobile 之间保持同步。

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)。
* Azure AD 有 [权](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 配置预配的用户帐户。 例如，应用程序管理员、云应用程序管理员、应用程序所有者或全局管理员。
* 具有超级管理员角色的 [票的移动](https://www.tictacmobile.com/) 帐户。


## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署

1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
1. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
1. 确定要 [在 Azure AD 和 Tic-Tac Mobile 之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。

## <a name="step-2-configure-tic-tac-mobile-to-support-provisioning-with-azure-ad"></a>步骤 2。 配置 Tic-Tac Mobile 以支持预配 Azure AD

联系 support@tictacmobile.com 以获取 **租户 URL** 和 **机密令牌** 。 若要接收令牌，必须在 Tic-Tac Mobile 中具有超级管理员角色。 令牌将在 Azure 门户中 Tic-Tac 移动应用程序的 " **设置** " 选项卡上的 " **机密令牌** " 框中输入。

## <a name="step-3-add-tic-tac-mobile-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 Tic-Tac 移动

从 Azure AD 应用程序库中添加 Tic-Tac Mobile，开始管理 Tic-Tac Mobile 的预配。 如果以前为单一登录设置 Tic-Tac Mobile，则可以使用相同的应用程序。 最初测试集成时，请创建一个单独的应用程序。 若要详细了解如何从库中添加应用程序，请参阅 [使用范围筛选器进行基于属性的应用程序设置](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)。

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中

利用 Azure AD 预配服务，可以确定将根据分配给应用程序或基于用户或组属性设置的用户的范围。 如果选择将根据分配预配到你的应用的用户的范围，请按照 [在 Azure Active Directory 中管理应用的用户分配](../manage-apps/assign-user-or-group-access-portal.md) 中的步骤将用户和组分配给应用程序。 如果选择仅根据用户或组的属性设置的作用域，请使用范围筛选器，如使用 [范围筛选器进行基于属性的应用程序设置](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中所述。

* 将用户和组分配到 Tic-Tac Mobile 时，必须选择 " **默认" 访问权限** 以外的角色。 具有默认访问角色的用户将从预配中排除，并在预配日志中被标记为不有效。 如果应用程序上唯一可用的角色是默认访问角色，则可以 [更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) 来添加更多角色。
* 先小部分测试。 向所有人推出之前，请使用一小部分用户和组进行测试。 如果设置的作用域设置为 "分配的用户和组"，则可以通过将一个或两个用户或组分配到应用来保持控制。 当作用域设置为 "所有用户和组" 时，可以指定 [基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。

## <a name="step-5-configure-automatic-user-provisioning-to-tic-tac-mobile"></a>步骤 5。 配置 Tic-Tac Mobile 的自动用户预配

本部分将指导你完成以下步骤：配置 Azure AD 预配服务，以便基于 Azure AD 中的用户或组分配在 TestApp 中创建、更新和禁用用户或组。

### <a name="configure-automatic-user-provisioning-for-tic-tac-mobile-in-azure-ad"></a>为 Azure AD 中 Tic-Tac Mobile 配置自动用户预配

1. 登录 [Azure 门户](https://portal.azure.com)。 选择“企业应用程序” > “所有应用程序”   。

    ![显示 "企业应用程序" 窗格的屏幕截图。](common/enterprise-applications.png)

1. 在应用程序列表中，选择 " **票-Tac Mobile** "。

    ![屏幕截图，显示应用程序列表中的 Tic-Tac 移动链接。](common/all-applications.png)

1. 选择“预配”  选项卡。

    ![显示 "预配" 选项卡的屏幕截图。](common/provisioning.png)

1. 将“预配模式”设置为“自动”。

    ![显示 "设置" 选项卡 "自动" 选项的屏幕截图。](common/provisioning-automatic.png)

1. 在 " **管理员凭据** " 部分中，输入 Tic-Tac 移动 **租户 URL** 和 **机密令牌** 。 选择 " **测试连接** " 以确保 Azure AD 可以连接到 Tic-Tac Mobile。 如果连接失败，请确保 Tic-Tac 移动帐户具有管理员权限，然后重试。

    ![显示 "机密令牌" 框的屏幕截图。](common/provisioning-testconnection-tenanturltoken.png)

1. 在 " **通知电子邮件** " 框中，输入应接收预配错误通知的人员或组的电子邮件地址。 选中“发生故障时发送电子邮件通知”复选框。

    ![显示 "通知电子邮件" 框的屏幕截图。](common/provisioning-notification-email.png)

1. 选择“保存”。

1. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到 Tic-Tac Mobile** "。

1. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 Tic-Tac Mobile 的用户属性。 选为 " **匹配** " 属性的属性用于匹配 Tic-Tac Mobile 中的用户帐户以执行更新操作。 如果更改了 [匹配目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，则必须确保 Tic-Tac 移动 API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|
   |---|---|
   |userName|字符串|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |externalId|字符串|
   |title|字符串|
   |电子邮件 [类型 eq "work"]。值|字符串|
   |preferredLanguage|字符串|
   |externalId|字符串|
   |userType|字符串|
   |区域设置|字符串|
   |timezone|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|字符串|

1. 若要配置范围筛选器，请参阅 [范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中的说明。

1. 若要为 Tic-Tac Mobile 启用 Azure AD 预配服务，请在 " **设置** " 部分中将 " **预配状态** " 更改为 **On** 。

    ![显示设置状态切换的屏幕截图。](common/provisioning-toggle-on.png)

1. 通过在 " **设置** " 部分的 " **范围** " 中选择所需的值，定义要为 Tic-Tac Mobile 设置的用户或组。

    ![显示预配范围的屏幕截图。](common/provisioning-scope.png)

1. 准备好预配时，选择“保存”。

    ![显示保存预配配置的屏幕截图。](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署

配置预配后，请使用以下资源来监视你的部署。

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败。
1. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user)以查看预配周期的状态以及完成进度。
1. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 若要了解有关隔离状态的详细信息，请参阅 [处于隔离状态的应用程序设置](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)
