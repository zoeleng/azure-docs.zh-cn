---
title: 教程：Azure Active Directory 与 Jive 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Jive 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: 3d15e5e13b2b0defe45fd45450aee7c262052b4f
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92459407"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jive"></a>教程：Azure Active Directory 与 Jive 的单一登录 (SSO) 集成

本教程介绍如何将 Jive 与 Azure Active Directory (Azure AD) 集成。 将 Jive 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Jive。
* 让用户使用其 Azure AD 帐户自动登录到 Jive。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了单一登录 (SSO) 的 Jive 订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Jive 支持 SP 发起的 SSO
* Jive 支持 [**自动** 用户预配](jive-provisioning-tutorial.md)
* 配置 Jive 后，就可以强制实施会话控制，从而实时保护组织的敏感数据免于外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-jive-from-the-gallery"></a>从库中添加 Jive

要配置 Jive 与 Azure AD 的集成，需要从库中将 Jive 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分中，在搜索框中键入“Jive” 。
1. 从结果面板中选择“Jive”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-single-sign-on-for-jive"></a>配置并测试 Jive 的 Azure AD 单一登录

使用名为 **B.Simon** 的测试用户配置并测试 Jive 的 Azure AD SSO。 若要正常使用 SSO，需要在 Azure AD 用户与 Jive 中的相关用户之间建立链接关系。

若要配置并测试 Jive 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    * **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    * **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Jive SSO](#configure-jive-sso)** - 在应用程序端配置单一登录设置。
    * **[创建 Jive 测试用户](#create-jive-test-user)** - 在 Jive 中创建 B.Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)中，在 **Jive** 应用程序集成页上，找到“管理”部分并选择“单一登录” 。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”部分，输入以下字段的值：

   a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<instance name>.jivecustom.com`

   b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：
   ```http
   https://<instance name>.jiveon.com
   ```

    > [!NOTE]
    > 这些不是实际值。 使用实际登录 URL 和标识符更新这些值。 请联系 [Jive 客户端支持团队](https://www.jivesoftware.com/services-support/)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

5. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分，单击“下载”以根据要求下载从给定选项提供的“联合元数据 XML”并将其保存在计算机上   。

    ![证书下载链接](common/metadataxml.png)

6. 在“设置 Jive”部分，根据要求复制相应 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

    a. 登录 URL

    b. Azure AD 标识符

    c. 注销 URL

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。 
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过授予 B.Simon 访问 Jive 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Jive”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。  

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。  
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。 
1. 在“添加分配”对话框中，单击“分配”按钮。 

## <a name="configure-jive-sso"></a>配置 Jive SSO

1. 若要在“Jive”端配置单一登录，请以管理员身份登录到 Jive 租户。

1. 在顶部菜单中，单击“SAML”。

    ![屏幕截图显示选择了“已启用”的“SAML”选项卡。](./media/jive-tutorial/tutorial_jive_002.png)

    a. 在“常规”选项卡下选择“已启用”。

    b. 单击“保存所有 SAML 设置”按钮。

1. 导航到“IDP 元数据”选项卡。

    ![屏幕截图显示选择了“IDP 元数据”的“SAML”选项卡。](./media/jive-tutorial/tutorial_jive_003.png)

    a. 复制下载的元数据 XML 文件的内容，并将其粘贴到“标识提供者(IDP)元数据”文本框中。

    b. 单击“保存所有 SAML 设置”按钮。

1. 选择“用户属性映射”选项卡。

    ![屏幕截图显示选择了“用户属性映射”的“SAML”选项卡。](./media/jive-tutorial/tutorial_jive_004.png)

    a. 在“电子邮件”文本框中，复制并粘贴“mail”值的属性名称。

    b. 在“名字”文本框中，复制并粘贴“givenname”值的属性名称。

    c. 在“姓氏”文本框中，复制并粘贴“surname”值的属性名称。

### <a name="create-jive-test-user"></a>创建 Jive 测试用户

本部分要在 Jive 中创建名为“Britta Simon”的用户。 Jive 支持在默认情况下启用的自动用户预配。 有关如何配置自动用户预配的更多详细信息，请参见[此处](jive-provisioning-tutorial.md)。

如果需要手动创建用户，请与 [Jive 客户端支持团队](https://www.jivesoftware.com/services-support/)协作，将用户添加到 Jive 平台中。

## <a name="test-sso"></a>测试 SSO 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Jive 磁贴时，应当会自动登录到为其设置了 SSO 的 Jive。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](./tutorial-list.md)

- [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

- [什么是 Azure Active Directory 中的条件访问？](../conditional-access/overview.md)

- [尝试通过 Azure AD 使用 Jive](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security 中的会话控制是什么？](/cloud-app-security/proxy-intro-aad)

- [配置用户预配](jive-provisioning-tutorial.md)

- [如何通过高级可见性和控制保护 Jive](/cloud-app-security/proxy-intro-aad)