---
title: 教程：配置 Figma 自动用户预配 Azure Active Directory |Microsoft Docs
description: 了解如何配置 Azure Active Directory 以自动将用户帐户预配到 Figma 以及取消其预配。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: 789dafc61c89515f4b2ef64933262252d1232f16
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357022"
---
# <a name="tutorial-configure-figma-for-automatic-user-provisioning"></a>教程：为 Figma 配置自动用户预配

本教程的目的是演示要在 Figma 和 Azure Active Directory (Azure AD) 中执行的步骤，以将 Azure AD 自动预配和取消预配到 Figma。

> [!NOTE]
> 本教程介绍在 Azure AD 用户预配服务之上构建的连接器。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../app-provisioning/user-provisioning.md)。
>
> 此连接器目前以公共预览版提供。 若要详细了解 Microsoft Azure 预览版功能的一般使用条款，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* Azure AD 租户。
* [Figma 租户](https://www.figma.com/pricing/)。
* Figma 中具有管理员权限的用户帐户。

## <a name="assign-users-to-figma"></a>将用户分配到 Figma。
Azure Active Directory 使用称为 "分配" 的概念来确定哪些用户应收到对所选应用的访问权限。 在自动用户预配的上下文中，只同步已分配到 Azure AD 中的应用程序的用户和/或组。

在配置和启用自动用户预配之前，应确定 Azure AD 中哪些用户和/或组需要访问 Figma。 确定后，可按照此处的说明将这些用户和/或组分配到 Figma：
 
* [向企业应用分配用户或组](../manage-apps/assign-user-or-group-access-portal.md)
## <a name="important-tips-for-assigning-users-to-figma"></a>将用户分配到 Figma 的重要提示

 * 建议将单个 Azure AD 用户分配到 Figma 以测试自动用户预配配置。 其他用户和/或组可以稍后分配。

* 将用户分配到 Figma 时，必须在分配对话框中选择任何特定于应用程序的有效角色 (如有) 。 具有“默认访问权限”  角色的用户排除在预配之外。

## <a name="set-up-figma-for-provisioning"></a>设置 Figma 以进行预配

在将 Figma 配置为 Azure AD 的自动用户预配之前，需要从 Figma 检索一些设置信息。

1. 登录到 [Figma 管理控制台](https://www.Figma.com/)。 单击租户旁边的齿轮图标。

    :::image type="content" source="media/Figma-provisioning-tutorial/image0.png" alt-text="Figma 管理控制台的屏幕截图。名为 A D Scim Test 的租户可见。租户旁边会突出显示齿轮图标。" border="false":::

2. 导航到 " **常规" > 更新 "设置" 中的 "日志"** 。

    :::image type="content" source="media/Figma-provisioning-tutorial/figma03.png" alt-text="Figma 管理控制台的 &quot;常规&quot; 选项卡的屏幕截图。在 &quot;登录并设置&quot; 下，将突出显示 &quot;更新登录设置&quot;。" border="false":::

3. 复制 **租户 ID** 。 此值将用于构造要输入到 Azure 门户中 Figma 应用程序的 "预配" 选项卡的 " **租户 URL** " 字段中的 SCIM 终结点 url。

    :::image type="content" source="media/Figma-provisioning-tutorial/figma-tenantid.png" alt-text="Figma 管理控制台中 S A M L S S O 部分的屏幕截图。一个租户 ID 标签和一个相邻的链接，其中突出显示了 &quot;复制&quot;。" border="false":::

4. 向下滚动并单击 " **生成 API 令牌** "。

    :::image type="content" source="media/Figma-provisioning-tutorial/token.png" alt-text="Figma 管理控制台中的 &quot;C I M 预配&quot; 部分的屏幕截图。将突出显示标记为 &quot;生成 P I 令牌&quot; 的链接。" border="false":::

5. 复制 "  **API 令牌** " 值。 此值将在 Azure 门户的 Figma 应用程序的 "预配" 选项卡的 " **机密令牌** " 字段中输入。 

    :::image type="content" source="media/Figma-provisioning-tutorial/figma04.png" alt-text="Figma 管理控制台中页的屏幕截图。在设置 P I 令牌下，将突出显示令牌的占位符。" border="false":::

## <a name="add-figma-from-the-gallery"></a>从库中添加 Figma

若要为 Figma 配置自动用户预 Azure AD 配，需要将 Azure AD 应用程序库中的 Figma 添加到托管的 SaaS 应用程序列表。

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，选择 " **Azure Active Directory** "。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用程序”，并选择“所有应用程序”。 

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请选择窗格顶部的 " **新建应用程序** " 按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中，输入 " **Figma** "，在结果面板中选择 " **Figma** "，然后单击 " **添加** " 按钮添加该应用程序。

    ![结果列表中的 Figma](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-figma"></a>配置 Figma 的自动用户预配 

本部分将指导你完成以下步骤：配置 Azure AD 预配服务，以便基于 Azure AD 中的用户和/或组分配在 Figma 中创建、更新和禁用用户和/或组。

> [!TIP]
> 你还可以选择按照 [Figma 单一登录教程](figma-tutorial.md)中提供的说明为 Figma 启用基于 SAML 的单一登录。 可以独立于自动用户预配配置单一登录，尽管这两个功能互相补充。

### <a name="to-configure-automatic-user-provisioning-for-figma--in-azure-ad"></a>若要在 Azure AD 中配置 Figma 的自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Figma”。

    ![应用程序列表中的 Figma 链接](common/all-applications.png)

3. 选择“预配”  选项卡。

    ![带有称为 "预配" 选项的 "管理" 选项的屏幕截图。](common/provisioning.png)

4. 将“预配模式”  设置为“自动”  。

    ![具有 "自动" 选项的 "预配模式" 下拉列表屏幕截图。](common/provisioning-automatic.png)

5. 在 " **管理员凭据** " 部分，在 " `https://www.figma.com/scim/v2/<TenantID>` **租户 URL** " 中输入 TenantID，其中 **TenantID** 是你之前从 Figma 中检索到的值。 输入 **机密令牌** 中的 **API 令牌** 值。 单击 " **测试连接** " 以确保 Azure AD 可以连接到 Figma。 如果连接失败，请确保 Figma 帐户具有管理员权限，然后重试。

    ![租户 URL + 令牌](common/provisioning-testconnection-tenanturltoken.png)

8. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中复选框“发生故障时发送电子邮件通知”   。

    ![通知电子邮件](common/provisioning-notification-email.png)

9. 单击“ **保存** ”。

10. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到 Figma** "。

    ![Figma 用户映射](media/Figma-provisioning-tutorial/figma05.png)

11. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 Figma 的用户属性。 选为 " **匹配** " 属性的特性用于匹配 Figma 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改  。

    ![Figma 用户属性](media/Figma-provisioning-tutorial/figma06.png)

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 Figma 启用 Azure AD 预配服务，请在 " **设置** " 部分中将 " **预配状态** " 更改为 **"打开** "。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 " **设置** " 部分的 " **范围** " 中选择所需的值，定义要预配到 Figma 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和/或组启动初始同步   。 初始同步执行的时间比后续同步长，只要 Azure AD 预配服务正在运行，大约每隔 40 分钟就会进行一次同步。 你可以使用 " **同步详细信息** " 部分监视进度并跟踪指向预配活动报告的链接，该报告描述了 Azure AD 预配服务对 Figma 执行的所有操作。

若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](../app-provisioning/check-status-user-account-provisioning.md)。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../app-provisioning/check-status-user-account-provisioning.md)