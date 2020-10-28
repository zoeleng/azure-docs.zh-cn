---
title: 将客户管理的密钥或自带密钥 (BYOK) 与媒体服务配合使用
description: 本教程介绍如何将客户管理的密钥与媒体服务存储帐户配合使用
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 10/18/2020
ms.openlocfilehash: c26da9d888bbcdf72c052fa4bd7fcdf443a2d8f7
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92325835"
---
# <a name="tutorial-use-customer-managed-keys-or-bring-your-own-key-byok-with-media-services"></a>教程：将客户管理的密钥或自带密钥 (BYOK) 与媒体服务配合使用

借助 2020-05-01 API，可将客户管理的 RSA 密钥与具有系统托管标识的媒体服务帐户配合使用。  本教程包括用于将 REST 请求发送到 Azure 服务的 Postman 集合和环境。  使用的服务：

- Azure Active Directory Postman 应用程序注册
- Microsoft Graph API
- Azure 存储
- Azure Key Vault
- 媒体服务

本教程将介绍如何使用 Postman 完成以下操作：

> [!div class="checklist"]
> * 获取用于 Azure 服务的令牌。
> * 创建资源组和存储帐户。
> * 创建包含系统托管标识的媒体服务帐户
> * 创建一个 Key Vault，以存储用于存储帐户的 RSA（客户管理的）密钥。
> * 更新媒体服务帐户，以将 RSA 密钥与存储帐户配合使用。
> * 在 Postman 中使用变量。

如果没有 Azure 订阅，[请创建一个免费的试用帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

- 注册具有适当权限的服务主体。
- 安装 [Postman](https://www.postman.com)。
- 通过 [Azure 示例：media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok) 为本教程下载 Postman 集合

### <a name="register-a-service-principal-with-the-needed-permissions"></a>注册具有所需权限的服务主体

[创建服务主体](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)。  请参阅[选项二](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options)以获取服务主体机密。  将机密记在某个位置，因为在离开门户中的机密页后，将无法访问该机密。

服务主体需要以下权限才能执行本教程中的任务。

![服务主体所需的权限](./media/tutorial-byok/service-principal-permissions-1.png)

### <a name="install-postman"></a>安装 Postman

如果尚未安装 Postman 以与 Azure 配合使用，可通过 [postman.com](https://www.postman.com/) 来获取它。

### <a name="download-and-import-the-collection"></a>下载并导入集合

通过 [Azure 示例：media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok) 为本教程下载 Postman 集合

## <a name="installation-of-collection-and-environment"></a>安装集合和环境

1. 运行 Postman。
1. 选择“导入”。
1. 选择“上传文件”。
1. 导航到集合和环境文件的保存位置。
1. 选择集合和环境文件。
1. 选择“打开”  。  （你将看到一条警告，指出不会将文件导入为 API，而是导入为集合。  这很好。  这正是你所希望的情况。）
1. 此集合现在将以 BYOK 的形式显示在“集合”中。
1. 环境变量现在将显示在“环境”中。

### <a name="understand-the-rest-api-requests-in-the-collection"></a>了解集合中的 REST API 请求

集合提供以下 REST API 请求。 必须按所提供的顺序发送请求，因为大多数请求都具有测试脚本，而这些脚本可为序列中的下一个（或后续）请求动态创建全局变量。 无需手动创建全局变量。

在 Postman 中，你将看到 `{{ }}` 括号中包含这些变量。  例如，`{{bearerToken}}`。

1. 获取 AAD 令牌 - 测试设置全局变量 bearerToken
2. 获取 Graph 令牌 - 测试设置全局变量 graphToken
3. 获取服务主体详细信息 - 测试设置全局变量 servicePrincipalObjectId
4. 创建存储帐户 - 测试设置全局变量 storageAccountId
5. 创建包含系统托管标识的媒体服务帐户 - 测试设置全局变量 principalId
6. 创建 Key Vault，授予访问服务主体的权限 - 测试设置全局变量 keyVaultId
7. 获取 Key Vault 令牌 - 测试设置全局变量 keyVaultToken
8. 在密钥保管库中创建 RSA 密钥 - 测试设置全局变量 keyId
9. 更新媒体服务帐户以将密钥与存储帐户配合使用 - 此请求无测试脚本。

## <a name="define-environment-variables"></a>定义环境变量

1. 通过选择环境下拉列表切换到下载的环境。
1. 在 Postman 中建立环境变量。 它们还用作 `{{ }}` 括号中包含的变量。  例如，`{{tenantId}}`。

    * tenantId = 你的租户 ID
    * servicePrincipalId = 使用你最喜欢的方法（门户、CLI 等）建立的服务主体的 ID。
    * servicePrincipalSecret = 为服务主体创建的机密
    * subscription = 你的订阅 ID
    * storageName = 要为你的存储提供的名称
    * accountName = 要使用的媒体服务帐户名称
    * keyVaultName = 要使用的 Key Vault 名称
    * resourceLocation = centralus（或要放置资源的任何位置。  仅使用 centralus 对此集合进行了测试。）
    * resourceGroup = 资源组名称

    以下变量是用于 Azure 资源的标准变量，因此无需更改它们。

    * armResource = `https://management.core.windows.net`
    * graphResource = `https://graph.windows.net/`
    * keyVaultResource = `https://vault.azure.net`
    * armEndpoint = `management.azure.com`
    * graphEndpoint = `graph.windows.net`
    * aadEndpoint = `login.microsoftonline.com`
    * keyVaultDomainSuffix = `vault.azure.net`

## <a name="send-the-requests"></a>发送请求

定义环境变量之后，可以按照上述顺序一次运行一个请求，或使用 Postman 的运行程序来运行集合。

## <a name="change-the-key"></a>更改密钥

媒体服务将自动检测密钥的更改时间。  若要对此进行测试，请为同一密钥创建另一个密钥版本。 媒体服务应该会在 15 分钟内检测到此密钥。

## <a name="clean-up-resources"></a>清理资源

如果不打算继续使用所创建的资源，并且不想继续付费，请删除这些资源。

## <a name="next-steps"></a>后续步骤

转到下一篇文章，了解如何…
> [!div class="nextstepaction"]
> [基于 URL 对远程文件进行编码并使用 REST 流式传输视频](stream-files-tutorial-with-rest.md)
