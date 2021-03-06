---
title: 教程：使用 Azure Active Directory 为 Dynamic Signal 配置自动用户预配 | Microsoft Docs
description: 了解如何配置 Azure Active Directory 来使其自动将用户帐户预配到 Dynamic Signal 和取消其预配。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: jeedes
ms.openlocfilehash: 263a67fd8fba2c336d1ed4d91475386a8ae175dd
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359011"
---
# <a name="tutorial-configure-dynamic-signal-for-automatic-user-provisioning"></a>教程：为 Dynamic Signal 配置自动用户预配

本教程的目的是演示需要执行哪些步骤在 Dynamic Signal 和 Azure Active Directory (Azure AD) 中配置 Azure AD 来使其自动将用户和/或组预配到 Dynamic Signal 以及对其取消预配。

> [!NOTE]
> 本教程介绍在 Azure AD 用户预配服务之上构建的连接器。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../app-provisioning/user-provisioning.md)。
>
> 此连接器目前以公共预览版提供。 若要详细了解 Microsoft Azure 预览版功能的一般使用条款，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* Azure AD 租户
* [Dynamic Signal 租户](https://dynamicsignal.com/)
* Dynamic Signal 中具有管理员权限的用户帐户。

## <a name="add-dynamic-signal-from-the-gallery"></a>从库中添加 Dynamic Signal

在使用 Azure AD 为 Dynamic Signal 配置自动用户预配之前，需要将 Dynamic Signal 从 Azure AD 应用程序库添加到托管 SaaS 应用程序的列表。

**若要从 Azure AD 应用程序库中添加 Dynamic Signal，请执行以下步骤：**

1. 在 [Azure 门户](https://portal.azure.com)的左侧导航面板中，选择“Azure Active Directory” 。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用程序”，并选择“所有应用程序”。 

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 要添加新应用程序，请选择窗格顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中输入 Dynamic Signal，在结果面板中选择 Dynamic Signal，然后单击“添加”按钮来添加该应用程序  。

    ![结果列表中的 Dynamic Signal](common/search-new-app.png)

## <a name="assigning-users-to-dynamic-signal"></a>将用户分配到 Dynamic Signal

Azure Active Directory 使用称为分配的概念来确定哪些用户应收到对所选应用的访问权限。 在自动用户预配的上下文中，只同步已分配到 Azure AD 中的应用程序的用户和/或组。

在配置和启用自动用户预配之前，应确定 Azure AD 中的哪些用户和/或组需要访问 Dynamic Signal。 确定后，可按照此处的说明将这些用户和/或组分配到 Dynamic Signal：

* [向企业应用分配用户或组](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dynamic-signal"></a>有关将用户分配到 Dynamic Signal 的重要提示

* 建议将单个 Azure AD 用户分配到 Dynamic Signal 来测试自动用户预配配置。 其他用户和/或组可以稍后分配。

* 如果将用户分配到 Dynamic Signal，必须在分配对话框中选择应用程序特定的所有有效角色（若可用）。 具有“默认访问权限”  角色的用户排除在预配之外。

## <a name="configuring-automatic-user-provisioning-to-dynamic-signal"></a>配置到 Dynamic Signal 的自动用户预配 

本部分介绍了如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 Dynamic Signal 中创建、更新和禁用用户和/或组。

> [!TIP]
> 还可选择按照 [Dynamic Signal 单一登录教程](dynamicsignal-tutorial.md)中提供的说明为 Dynamic Signal 启用基于 SAML 的单一登录。 可以独立于自动用户预配配置单一登录，尽管这两个功能互相补充。

### <a name="to-configure-automatic-user-provisioning-for-dynamic-signal-in-azure-ad"></a>若要在 Azure AD 中为 Dynamic Signal 配置自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Dynamic Signal”。

    ![应用程序列表中的 Dynamic Signal 链接](common/all-applications.png)

3. 选择“预配”  选项卡。

    ![“管理”选项的屏幕截图，其中突出显示了“预配”选项。](common/provisioning.png)

4. 将“预配模式”  设置为“自动”  。

    ![“预配模式”下拉列表的屏幕截图，其中突出显示了“自动”选项。](common/provisioning-automatic.png)

5. 在“管理员凭据”部分下，输入 Dynamic Signal 帐户的租户 URL 和机密令牌，如步骤 6 中所述  。

6. 在 Dynamic Signal 管理控制台中，导航到“管理员”>“高级”>“API”。

    :::image type="content" source="./media/dynamic-signal-provisioning-tutorial/secret-token-1.png" alt-text="Dynamic Signal 管理控制台的屏幕截图。“高级”选项在管理员菜单中突出显示。还显示了“高级”菜单，其中突出显示了 API。" border="false":::

    将 SCIM API URL 复制到“租户 URL” 。 单击“生成新令牌”以生成持有者令牌，并将值复制到“机密令牌”  。

    :::image type="content" source="./media/dynamic-signal-provisioning-tutorial/secret-token-2.png" alt-text="“令牌”页面的屏幕截图，其中突出显示了 SCI M API URL、“生成新令牌”和“持有者令牌”，“持有者令牌”框中有一个占位符。" border="false":::

7. 填充步骤 5 中所示的字段后，请单击“测试连接”，确保 Azure AD 可连接到 Dynamic Signal。 如果连接失败，请确保 Juno Dynamic Signal 帐户具有管理员权限，然后重试。

    ![租户 URL + 令牌](common/provisioning-testconnection-tenanturltoken.png)

8. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中复选框“发生故障时发送电子邮件通知”   。

    ![通知电子邮件](common/provisioning-notification-email.png)

9. 单击“ **保存**”。

10. 在“映射”部分下，选择“将 Azure Active Directory 用户同步到 Dynamic Signal” 。

    ![Dynamic Signal 用户映射](media/dynamic-signal-provisioning-tutorial/user-mappings.png)

11. 在“特性映射”部分中，查看从 Azure AD 同步到 Dynamic Signal 的用户特性。 选为“匹配”属性的特性用于匹配 Dynamic Signal 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改  。

    ![Dynamic Signal 用户属性](media/dynamic-signal-provisioning-tutorial/user-mapping-attributes.png)

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 Dynamic Signal 启用 Azure AD 预配服务，请在“设置”部分中将“预配状态”更改为“开”  。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在“设置”部分的“范围”中选择所需的值，定义要预配到 Dynamic Signal 的用户和/或组 。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和/或组启动初始同步   。 初始同步执行的时间比后续同步长，只要 Azure AD 预配服务正在运行，大约每隔 40 分钟就会进行一次同步。 可使用“同步详细信息”部分监视进度并跟踪指向预配活动报告的链接，该报告描述了 Azure AD 预配服务对 Dynamic Signal 执行的所有操作。

若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](../app-provisioning/check-status-user-account-provisioning.md)。

## <a name="connector-limitations"></a>连接器限制

* Dynamic Signal 不支持从 Azure AD 中永久删除用户。 若要在 Dynamic Signal 中永久删除用户，必须通过 Dynamic Signal 管理控制台 UI 执行此操作。 
* Dynamic Signal 当前不支持组。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../app-provisioning/check-status-user-account-provisioning.md)

