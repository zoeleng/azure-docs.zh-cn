---
title: 将 Google 添加为 B2B 的标识提供者 - Azure AD
description: 与 Google 联合，使来宾用户能够使用其自己的 Gmail 帐户登录到你的 Azure AD 应用。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff8912794169cf61f394a097248a8476b2e0c0f3
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92926229"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>将 Google 添加为 B2B 来宾用户的标识提供者

通过设置与 Google 的联合身份验证，你可以允许受邀用户使用其自己的 Gmail 帐户登录到你的共享应用和资源，而无需创建 Microsoft 帐户。 

> [!NOTE]
> Google 联合专为 Gmail 用户设计。 若要与 G Suite 域联合，请使用 [直接联合](direct-federation.md)。

## <a name="what-is-the-experience-for-the-google-user"></a>Google 用户体验是什么？
向 Google Gmail 用户发送邀请时，来宾用户应通过使用包含租户上下文的链接来访问共享应用或资源。 他们的体验根据是否已登录到 Google 而异：
  - 系统将提示未登录到 Google 的来宾用户执行此操作。
  - 系统将提示已登录到 Google 的来宾用户选择他们想要使用的帐户。 他们必须选择你在邀请他们时所用的帐户。

如果来宾用户看到 "标头过长" 错误，则可能会清除其 cookie，或者打开私有或 incognito 窗口并再次尝试登录。

![显示 Google 登录页的屏幕截图。](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>限制

Teams 完全支持所有设备上的 Google 来宾用户。 Google 用户可以从 `https://teams.microsoft.com` 等常见终结点登录到 Teams。

其他应用程序的常见终结点可能不支持 Google 用户。 Google guest 用户必须使用包含租户信息的链接登录。 下面是一些示例：
  * `https://myapps.microsoft.com/?tenantid=<your tenant ID>`
  * `https://portal.azure.com/<your tenant ID>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   如果 Google guest 用户尝试使用类似于或的 `https://myapps.microsoft.com` 链接 `https://portal.azure.com` ，则会出现错误。

你还可以为 Google guest 用户提供指向应用程序或资源的直接链接，前提是该链接包含你的租户信息。 例如 `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`。 

## <a name="step-1-configure-a-google-developer-project"></a>步骤 1：配置 Google 开发人员项目
首先，在 Google 开发人员控制台中创建一个新项目，以获取客户端 ID 和客户端密钥，稍后你可以将其添加到 Azure Active Directory (Azure AD) 。 
1. 转到 Google API (https://console.developers.google.com )，并使用 Google 帐户登录。 我们建议使用共享团队 Google 帐户。
2. 如果系统提示你执行此操作，请接受服务条款。
3. 创建新项目：在仪表板上，选择 " **创建项目** "，为项目指定名称 (例如， **Azure AD B2B** ) ，然后选择 " **创建** "： 
   
   ![显示新项目页的屏幕截图。](media/google-federation/google-new-project.png)

4. 在 " **api & Services** " 页上，选择新项目下的 " **查看** "。

5. 选择 "Api" 卡上的 " **跳到 api 概述** "。 选择 " **OAuth 许可屏幕** "。

6. 依次选择“外部”、“创建”。 

7. 在 **OAuth 许可屏幕** 上，输入 **应用程序名称** ：

   ![显示 Google OAuth 许可屏幕的屏幕截图。](media/google-federation/google-oauth-consent-screen.png)

8. 滚动到 " **授权域** " 部分，然后输入 **microsoftonline.com** ：

   ![显示授权域部分的屏幕截图。](media/google-federation/google-oauth-authorized-domains.PNG)

9. 选择“保存”  。

10. 选择“凭据”。  在 " **创建凭据** " 菜单上，选择 " **OAUTH 客户端 ID** "：

    ![显示 Google Api 创建凭据菜单的屏幕截图。](media/google-federation/google-api-credentials.png)

11. 在“应用程序类型”  下，选择“Web 应用程序”  。 为应用程序指定适当的名称，如 **AZURE AD B2B** 。 在 " **授权重定向 uri** " 下，输入以下 uri：
    - `https://login.microsoftonline.com`
    - `https://login.microsoftonline.com/te/<tenant ID>/oauth2/authresp` <br> (，其中 `<tenant ID>` 是租户 ID) 
   
    > [!NOTE]
    > 若要查找租户 ID，请参阅 [Azure 门户](https://portal.azure.com)。 在 " **Azure Active Directory** " 下，选择 " **属性** " 并复制 " **租户 ID** "。

    ![显示 "授权重定向 Uri" 部分的屏幕截图。](media/google-federation/google-create-oauth-client-id.png)

12. 选择“创建”。 复制客户端 ID 和客户端密码。 在 Azure 门户中添加标识提供程序时，将使用它们。

    ![显示 OAuth 客户端 ID 和客户端密钥的屏幕截图。](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>步骤 2：在 Azure AD 中配置 Google 联合 
现在，你将设置 Google 客户端 ID 和客户端密码。 你可以使用 Azure 门户或 PowerShell 来执行此操作。 请确保通过邀请自己来测试 Google 联合身份验证配置。 使用 Gmail 地址，尝试使用受邀的 Google 帐户兑换邀请。 

**在 Azure 门户中配置 Google federation** 
1. 转到 [Azure 门户](https://portal.azure.com)。 在左侧窗格中，选择“Azure Active Directory”。 
2. 选择“外部标识”。
3. 选择 " **所有标识提供者** "，然后选择 " **Google** " 按钮。
4. 输入前面获取的客户端 ID 和客户端密码。 选择 " **保存** "： 

   ![显示 "添加 Google 标识提供程序" 页的屏幕截图。](media/google-federation/google-identity-provider.png)

**使用 PowerShell 配置 Google 联合**
1. 安装最新版本的 Azure AD PowerShell for Graph 模块 ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview))。
2. 运行以下命令：`Connect-AzureAD`
3. 根据登录提示使用托管的全局管理员帐户登录。  
4. 运行以下命令： 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId <client ID> -ClientSecret <client secret>`
 
   > [!NOTE]
   > 使用在 "步骤1：配置 Google 开发人员项目" 中创建的应用的客户端 ID 和客户端密码。 有关详细信息，请参阅 [AzureADMSIdentityProvider](/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview)。 
 
## <a name="how-do-i-remove-google-federation"></a>如何删除 Google 联合？
可以删除 Google 联合设置。 如果这样做，已经兑换邀请的 Google guest 用户将无法登录。 不过，你可以通过从目录中删除并重新邀请资源来向他们授予对资源的访问权限。 
 
**在 Azure AD 门户中删除 Google federation**
1. 转到 [Azure 门户](https://portal.azure.com)。 在左侧窗格中，选择“Azure Active Directory”。 
2. 选择“外部标识”。
3. 选择“所有标识提供者”。
4. 在 **Google** 行上，选择省略号按钮 " ( **...** ") ，然后选择 " **删除** "。 
   
   ![显示社交标识提供者的 "删除" 按钮的屏幕截图。](media/google-federation/google-social-identity-providers.png)

1. 选择 **"是"** 以确认删除。 

**使用 PowerShell 删除 Google federation** 
1. 安装最新版本的 Azure AD PowerShell for Graph 模块 ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview))。
2. 运行 `Connect-AzureAD`。  
4. 根据登录提示，使用托管的全局管理员帐户登录。  
5. 输入以下命令：

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > 有关详细信息，请参阅 [Remove-AzureADMSIdentityProvider](/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview)。
