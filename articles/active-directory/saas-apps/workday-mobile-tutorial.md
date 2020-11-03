---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Workday 移动应用程序集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Workday 移动应用程序之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: jeedes
ms.openlocfilehash: 256da169761da486d8ac064a2f58a59be43bb5df
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754149"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday-mobile-application"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Workday 移动应用程序集成

本教程介绍如何将 Azure Active Directory (Azure AD)、条件访问和 Intune 与 Workday 移动应用集成。 将 Workday 的移动应用与 Microsoft 集成后，可以执行以下操作：

* 登录之前，请确保设备符合策略。
* 在 Workday 的应用中添加控件，以确保用户安全地访问公司数据。 
* 在 Azure AD 中控制谁有权访问 Workday。
* 让用户使用其 Azure AD 帐户自动登录到 Workday。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 将 Workday 与 Azure AD 集成
* 教程：[Azure Active Directory 单一登录 (SSO) 与 Workday 集成](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial)

## <a name="scenario-description"></a>方案描述

在本教程中，你将为 Workday 的移动应用程序配置并测试 Microsoft 的条件访问策略和 Intune。

* 现在可以为 Workday 联合应用程序配置 Azure AD 以启用 SSO。 有关如何配置的详细信息，请访问[此链接](workday-tutorial.md)。

> [!NOTE] 
> Workday 不支持 Intune 的应用保护策略。 必须使用移动设备管理才能利用条件访问。


## <a name="ensure-users-have-access-to-the-workday-mobile-app"></a>确保用户能够访问 Workday 移动应用：

配置 Workday 以允许访问其移动应用产品/服务。 需要为移动设备配置以下策略：

可以按照以下说明进行配置：

1. 访问功能区域报表的域安全策略。
2. 选择安全策略。
    * 移动设备使用情况 - Android
    * 移动设备使用情况 - iPad
    * 移动设备使用情况 - iPhone
3. 单击“编辑权限”。
4. 选中“查看或修改”复选框，授权安全组访问应保护的报告或任务对象项。
5. 选中“获取”或“放置”复选框，授予安全组访问集成和报表或任务安全对象操作的权限。

通过运行“激活挂起的安全策略更改”任务来激活挂起的安全策略更改。

## <a name="open-workday-login-page-in-mobile-browser"></a>在移动浏览器中打开 Workday 登录页：

若要将条件访问权限应用于 Workday 的移动应用，必须在外部浏览器中打开该应用。 可通过选中“编辑租户设置 - 安全性”中的“为本机应用启用移动浏览器 SSO”框来完成此操作 。 这将需要在 iOS 设备上和于 Android 工作配置文件中安装 Intune 批准的浏览器

![移动浏览器登录](./media/workday-tutorial/mobile-browser.png)

## <a name="setup-conditional-access-policy"></a>设置条件访问策略：

此策略只会影响在 iOS 或 Android 设备上的登录。 如果想将其扩展到所有平台，选择“任何设备”即可。 此策略要求设备符合策略，将通过 Microsoft Intune 验证其符合性。 由于 Android 具有工作配置文件，这将阻止任何用户登录 Workday（Web 或应用），除非他们通过其工作配置文件登录且已通过 Intune 公司门户安装了该应用。 对于 iOS，需要执行一个额外的步骤，确保同样的情况适用。 下面是“条件访问”设置的一些屏幕截图。

**Workday 支持以下访问控制：**
* 需要多重身份验证
* 要求将设备标记为合规

**Workday 应用不支持以下功能：**
* 需要批准的客户端应用
* 需要应用保护策略（预览版）

若要将 Workday 设置为“受管理设备”，请执行以下步骤 ：

![设置条件访问策略](./media/workday-tutorial/managed-devices-only.png)

1. 单击“主页”>“Microsoft Intune”>“条件访问策略”>“仅限受管理设备” 

1. 在“仅限受管理设备”页上，将“名称”字段值设置为 `Managed Devices Only`，然后单击“云应用或操作”  。

1. 在“云应用或操作”中执行以下步骤。

    a. 将“选择此策略适用的对象”切换为“云应用” 。

    b. 在“包含”中，单击“选择应用”。

    c. 从选择列表中选择“Workday”。

    d. 单击“完成”  。

1. 打开“启用策略”。

1. 单击“ **保存** ”。

若要“授权”访问，请执行以下步骤：

![Workday 设置条件访问策略](./media/workday-tutorial/managed-devices-only-2.png)

1. 单击“主页”>“Microsoft Intune”>“条件访问策略”>“仅限受管理设备” 

1. 在“仅限受管理设备”页上，将“名称”字段值设置为 `Managed Devices Only`，然后单击“访问控制”>“授权”  。

1. 在“授权”页面上，执行以下步骤。

    a. 为要强制执行的控件选择“授予访问权限”。

    b. 选中“需要将设备标记为合规”框。

    c. 选择“需要所选控件之一”。

    d. 单击“选择”。

1. 打开“启用策略”。

1. 单击“保存” 

## <a name="set-up-device-compliance-policy"></a>设置设备合规性策略：

若要确保 iOS 设备只能通过 MDM 托管的 Workday 应用登录，必须通过在受限应用列表中添加 com.workday.workdayapp 来阻止 App Store 应用。 这可确保只有通过公司门户安装 Workday 应用的设备才能访问 Workday。 对于浏览器，仅当设备由 Intune 管理，且用户使用托管浏览器时，才能访问 Workday。

![Workday 设置设备合规性策略](./media/workday-tutorial/ios-policy.png)

## <a name="set-up-microsoft-intune-app-configuration-policies"></a>设置 Microsoft Intune 应用程序配置策略：

| 方案 | 键值对 |
|----------------------------------------------------------------------------------------   |-----------|
| 自动为以下应用填充“租户”和“Web 地址”字段：<br>● Android 上的 Workday（启用 Android for Work 配置文件时）。<br>● iPad 和 iPhone 上的 Workday。     | 使用以下值来配置租户： <br>● 配置密钥 = UserGroupCode<br>●   值类型 = 字符串 <br>●   配置值 = 租户名称。 示例：gms<br>使用以下值来配置 Web 地址：<br>●    配置密钥 = AppServiceHost<br>● 值类型 = 字符串<br>●    配置值 = 租户的基 URL。 示例： https://www.myworkday.com                              |   |
| 在 iPad 和 iPhone 上为 Workday 禁用以下操作：<br>●    剪切、复制和粘贴<br>●   打印                       | 在这些键上将值（布尔值）设置为 False，以禁用功能：<br>● AllowCutCopyPaste<br>●  AllowPrint  |
| 在 Android 上禁用 Workday 的屏幕截图功能。 |将 AllowScreenshots 键上的值（布尔值）设置为 False，以禁用功能。|
| 为用户禁用建议的更新。|将 AllowSuggestedUpdates 键上的值（布尔值）设置为 False，以禁用功能。|
|自定义应用商店 URL，以将移动用户定向到所选的应用商店。|使用以下值更改应用商店的 URL：<br>● 配置密钥 = AppUpdateURL<br>●   值类型 = 字符串<br> ●   配置值 = 应用商店 URL|
|       |


## <a name="ios-configuration-policies"></a>iOS 配置策略：

1. 转到 https://portal.azure.com/ 并登录
2. 搜索“Intune”或单击列表中的小组件。
3. 转到“客户端应用”->“应用”->“应用程序配置策略”->“+ 添加”->“受管理设备”
4. 输入名称。
5. 在“平台”下，选择“iOS/iPadOS” 
6. 在“关联的应用”下，选择你添加的适用于 iOS 的 Workday 应用
7. 单击“配置设置”，然后在“配置设置格式”下选择“输入 XML 数据”  
8. 下面是示例 XML 文件。 添加要应用的配置。 将 STRING_VALUE 替换为要使用的字符串。 将 `<true />` 或 `<false />` 替换为 `<true />` 或 `<false />`。 如果未添加配置，则其功能将与设置为 True 时相同。

    ```
    <dict>
    <key>UserGroupCode</key>
    <string>STRING_VALUE</string>
    <key>AppServiceHost</key>
    <string>STRING_VALUE</string>
    <key>AllowCutCopyPaste</key>
    <true /> or <false />
    <key>AllowPrint</key>
    <true /> or <false />
    <key>AllowSuggestedUpdates</key>
    <true /> or <false />
    <key>AppUpdateURL</key>
    <string>STRING_VALUE</string>
    </dict>

    ```
9. 单击“添加”
10. 刷新页面，然后单击新创建的策略。
11. 单击“分配”，然后选择该应用适用于的用户。
12. 单击“保存”。

## <a name="android-configuration-policies"></a>Android 配置策略：

1. 转到 `https://portal.azure.com/` 并登录。
2. 搜索“Intune”或单击列表中的小组件。
3. 转到“客户端应用”->“应用”->“应用程序配置策略”->“+ 添加”->“受管理设备”
5. 输入名称。 
6. 在“平台”下，选择“Android” 
7. 在“关联的应用”下，选你择添加的适用于 Android 的 Workday 应用
8. 单击“配置设置”，然后在“配置设置格式”下选择“输入 JSON 数据”  

