---
title: 电话身份验证方法-Azure Active Directory
description: 了解如何在 Azure Active Directory 中使用手机身份验证方法，以帮助改进和保护登录事件
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39555c5b396e8a8fd3449331cd2fd68b96ad2087
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94839992"
---
# <a name="authentication-methods-in-azure-active-directory---phone-options"></a>Azure Active Directory 电话选项中的身份验证方法

对于使用文本消息的直接身份验证，可以[为用户配置和启用基于 SMS 的身份验证（预览）](howto-authentication-sms-signin.md)。 基于 SMS 的登录方式对于一线工作人员来说非常适合。 使用基于 SMS 的登录，用户无需知道用户名和密码即可访问应用程序和服务。 用户的验证方式改为：输入其注册的手机号码，收到带有验证码的短信，并在登录界面中输入该验证码。

用户还可以使用移动电话或办公电话来验证自己的身份，作为 Azure AD 多重身份验证或自助服务密码重置 (SSPR) 期间使用的辅助身份验证。

若要正常工作，电话号码必须采用“+国家/地区代码 电话号码”格式，例如 +1 4251234567 。

> [!NOTE]
> 在国家/地区代码和电话号码之间需要有一个空格。
>
> 密码重置不支持电话分机。 即使采用“1 4251234567X12345”格式，在拨出电话前也会删除分机。

## <a name="mobile-phone-verification"></a>手机验证

对于 Azure AD 多重身份验证或 SSPR，用户可以选择接收包含验证码的短信，使其在登录界面中输入或接收电话呼叫。

如果用户不希望在目录中公开其移动电话号码，但想使用该号码来重置密码，则管理员不应在目录中填充该号码。 而是由用户通过组合的安全信息注册 ([https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)) 来填充“身份验证电话”属性。 管理员可在用户的配置文件中看到此信息，但此信息不会发布到其他位置。

:::image type="content" source="media/concept-authentication-methods/user-authentication-methods.png" alt-text="屏幕截图显示已填充电话号码的身份验证方法的 Azure 门户":::

Microsoft 不保证一致的短信或基于语音的 Azure AD 多重身份验证提示以相同数量传递。 为了用户的利益，我们在做出路线调整期间可能随时添加或删除简短代码，以提高短信传送能力。 Microsoft 不支持除美国和加拿大以外的国家/地区的简短代码。

### <a name="text-message-verification"></a>短信验证

通过在 SSPR 或 Azure AD 多重身份验证期间进行短信验证，将向包含验证码的手机号码发送一条短信。 若要完成登录过程，需要在登录界面中输入提供的验证码。

### <a name="phone-call-verification"></a>电话呼叫验证

通过在 SSPR 或 Azure AD 多重身份验证期间进行电话呼叫验证，对用户注册的电话号码进行自动语音呼叫。 若要完成登录过程，系统会提示用户在其键盘上按 #。

## <a name="office-phone-verification"></a>办公电话验证

通过在 SSPR 或 Azure AD 多重身份验证期间进行电话呼叫验证，对用户注册的电话号码进行自动语音呼叫。 若要完成登录过程，系统会提示用户在其键盘上按 #。

## <a name="troubleshooting-phone-options"></a>排查电话选项问题

如果在使用 Azure AD 的手机身份验证时出现问题，请查看以下故障排除步骤：

* "你已经达到了对验证调用的限制" 或 "在登录时遇到了对文本验证代码的限制" 错误消息
   * 使用 Microsoft 验证器应用或验证码完成身份验证，或稍后重试。
* 单个设备上阻止了呼叫方 ID。
   * 查看设备上配置的任何被阻拦的号码。
* 电话号码错误或国家/地区代码不正确，或混淆个人电话号码和工作电话号码。
   * 对用户对象和配置的身份验证方法进行故障排除。 确保注册了正确的电话号码。
* 输入了错误的 PIN。
   * 确认用户已使用为其帐户注册的正确 PIN (仅) MFA 服务器用户。
* 呼叫被转到语音信箱。
   * 确保用户的电话处于开启状态，并且所需服务在其区域中可用，或使用替代方法。
* 用户被阻止
   * 让 Azure AD 管理员在 Azure 门户中取消阻止该用户。
* 设备上未订阅 SMS。
   * 让用户更改方法或在设备上激活 SMS。
* 电信提供程序有问题，如检测不到电话输入、DTMF 音调缺失的问题、在多台设备上阻止呼叫方 ID 或在多台设备阻止 SMS。
   * Microsoft 使用多个电信提供程序来路由电话呼叫和短信以进行身份验证。 如果发生上述任何问题，请让用户尝试在 5 分钟内至少使用此方法 5 次，并在联系 Microsoft 支持人员时提供该用户的信息。

## <a name="next-steps"></a>后续步骤

若要开始，请参阅 [自助服务密码重置教程 (SSPR) ][tutorial-sspr] 和 [Azure AD 多重身份验证][tutorial-azure-mfa]。

要详细了解 SSPR 概念，请参阅[ Azure AD 自助式密码重置的工作原理][concept-sspr]。

若要了解有关 MFA 概念的详细信息，请参阅 [Azure AD 多重身份验证的工作原理][concept-mfa]。

详细了解如何使用 [Microsoft Graph REST API beta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta&preserve-view=true)配置身份验证方法。

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
