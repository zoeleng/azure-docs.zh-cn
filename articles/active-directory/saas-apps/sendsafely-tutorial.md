---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 SendSafely 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 SendSafely 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/24/2020
ms.author: jeedes
ms.openlocfilehash: c43756e7526a260f238ed6e9d0c1f253041b05e7
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674411"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sendsafely"></a>教程：Azure Active Directory 单一登录 (SSO) 与 SendSafely 集成

本教程介绍如何将 SendSafely 与 Azure Active Directory (Azure AD) 集成。 将 SendSafely 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 SendSafely。
* 让用户使用其 Azure AD 帐户自动登录到 SendSafely。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 SendSafely 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* SendSafely 支持 SP 和 IDP 发起的 SSO
* SendSafely 支持实时用户预配

* 配置 SendSafely 后，就可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-sendsafely-from-the-gallery"></a>从库中添加 SendSafely

要配置 SendSafely 与 Azure AD 的集成，需要从库中将 SendSafely 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入“SendSafely” 。
1. 从结果面板中选择“SendSafely”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-single-sign-on-for-sendsafely"></a>配置和测试 SendSafely 的 Azure AD 单一登录

使用名为 B.Simon 的测试用户配置和测试 SendSafely 的 Azure AD SSO。 若要正常使用 SSO，需要在 Azure AD 用户与 SendSafely 中的相关用户之间建立链接关系。

若要配置并测试 SendSafely 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 SendSafely SSO](#configure-sendsafely-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 SendSafely 测试用户](#create-sendsafely-test-user)** - 在 SendSafely 中创建 B.Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“SendSafely”应用程序集成页上，找到“管理”部分，选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分中输入以下字段的值 ：

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<SENDSAFELY_URL>/auth/saml2/`

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://<SENDSAFELY_URL>/auth/saml2/`

1. 若要在 **SP** 发起的模式下配置应用程序，请单击“设置其他 URL”并执行以下步骤：

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<SENDSAFELY_URL>/auth/`。

    b. 在“中继状态”文本框中，使用以下模式键入 URL：`https://<SENDSAFELY_URL>/auth/saml2/`

    > [!NOTE]
    > 这些不是实际值。 请使用“标识符”、“回复 URL”、“登录 URL”和“中继状态”更新这些值。 请联系 [SendSafely 客户端支持团队](mailto:support@sendsafely.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中，找到“证书(Base64)”，选择“下载”以下载该证书并将其保存到计算机上   。

    ![证书下载链接](common/certificatebase64.png)

1. 在“设置 SendSafely”部分，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

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

在本部分中，将通过授予 B.Simon 访问 SendSafely 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“SendSafely”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。  

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。  
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。 
1. 在“添加分配”对话框中，单击“分配”按钮。 

## <a name="configure-sendsafely-sso"></a>配置 SendSafely SSO

若要在 SendSafely 端配置单一登录，请按照[此处](https://sendsafely.zendesk.com/hc/articles/360004152492-Setup-Single-Sign-On-SSO-with-SAML)提供的步骤进行操作。

### <a name="create-sendsafely-test-user"></a>创建 SendSafely 测试用户

在本部分，将在 SendSafely 中创建名为 B.Simon 的用户。 SendSafely 支持默认启用的实时预配。 此部分不存在任何操作项。 尝试访问 SendSafely 时，如果 SendSafely 中尚不存在用户，则系统会创建一个新用户。

## <a name="test-sso"></a>测试 SSO 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 SendSafely 磁贴时，应当会自动登录到为其设置了 SSO 的 SendSafely。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/my-apps-portal-end-user-access.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](./tutorial-list.md)

- [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

- [什么是 Azure Active Directory 中的条件访问？](../conditional-access/overview.md)

- [通过 Azure AD 试用 SendSafely](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security 中的会话控制是什么？](/cloud-app-security/proxy-intro-aad)

- [如何通过高级可见性和控制保护 SendSafely](/cloud-app-security/proxy-intro-aad)