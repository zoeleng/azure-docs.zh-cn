---
title: Azure 快速入门 - 使用 Azure 门户在 Key Vault 中设置和检索机密 | Microsoft Docs
description: 快速入门介绍如何使用 Azure 门户在 Azure Key Vault 中设置和检索机密
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 080e2daf5065c0762fb039a84e62580e5c915ddb
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92735173"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-the-azure-portal"></a>快速入门：使用 Azure 门户在 Azure Key Vault 中设置和检索机密

Azure Key Vault 是一项云服务，它为机密提供了安全的存储。 可以安全地存储密钥、密码、证书和其他机密。 可以通过 Azure 门户创建和管理 Azure Key Vault。 在本快速入门中，你将创建一个 Key Vault 并使用它来存储机密。 有关 Key Vault 的详细信息，请参阅[概述](../general/overview.md)。

有关机密的详细信息，请参阅[关于机密](about-secrets.md)。

## <a name="prerequisites"></a>先决条件

- Azure 订阅 - [免费创建订阅](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.com 登录到 Azure 门户。

## <a name="create-a-vault"></a>创建保管库

1. 在 Azure 门户菜单或“主页”中，选择“创建资源” 。
2. 在“搜索”框中输入“Key Vault”。
3. 从结果列表中选择“Key Vault”。
4. 在“Key Vault”部分，选择“创建”。
5. 在“创建密钥保管库”部分，提供以下信息： 
    - **Name** ：必须提供唯一的名称。 对于本快速入门，我们使用 **Contoso-vault2** 。 
    - **订阅** ：选择订阅。
    - 在“资源组”下选择“新建”，然后输入资源组名称。
    - 在“位置”下拉菜单中选择一个位置。
    - 让其他选项保留默认值。
6. 提供上述信息后，选择“创建”  。

请记下下面列出的两个属性：

* **保管库名称** ：在本示例中，此项为 **Contoso-Vault2** 。 将在其他步骤中使用此名称。
* **保管库 URI** ：在本示例中，此项为 https://contoso-vault2.vault.azure.net/ 。 通过其 REST API 使用保管库的应用程序必须使用此 URI。

还可使用 Azure CLI 和 PowerShell 创建 Key Vault：
- [使用 PowerShell 创建 Key Vault](../general/quick-create-powershell.md)
- [使用 Azure CLI 创建 Key Vault](../general/quick-create-cli.md)

目前，只有你的 Azure 帐户有权对这个新保管库执行操作。

![Key Vault 创建完成后的输出](../media/quick-create-portal/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>向 Key Vault 添加机密

只需再执行几个步骤即可向保管库添加机密。 在此示例中，我们将添加可供应用程序使用的密码。 此密码名为 **ExamplePassword** ，我们在其中存储的值为 **hVFkk965BuUv** 。

1. 在 Key Vault 属性页中，选择“机密”。
2. 单击“生成/导入”。
3. 在“创建机密”屏幕上，选择以下值：
    - **上传选项** ：手动。
    - **Name** ：ExamplePassword。
    - **值** ：hVFkk965BuUv
    - 让其他值保留默认设置。 单击“创建”。

收到机密已成功创建的消息后，即可单击列表中的该机密， 

## <a name="retrieve-a-secret-from-key-vault"></a>从 Key Vault 检索机密

如果单击当前版本，则可看到在上一步指定的值。

![机密属性](../media/quick-create-portal/current-version-hidden.png)

单击右侧窗格中的“显示机密值”按钮后，可看到隐藏的值。 

![显示的机密值](../media/quick-create-portal/current-version-shown.png)

## <a name="clean-up-resources"></a>清理资源

其他 Key Vault 快速入门和教程是在本快速入门的基础上制作的。 如果打算继续使用后续的快速入门和教程，则可能需要保留这些资源。
如果不再需要资源组，可以将其删除，这将删除 Key Vault 和相关的资源。 要通过门户删除资源组，请执行以下操作：

1. 在门户顶部的“搜索”框中输入资源组的名称。 在搜索结果中看到在本快速入门中使用的资源组后，将其选中。
2. 选择“删除资源组”  。
3. 在“键入资源组名称:”框中，键入资源组的名称，然后选择“删除” 。


## <a name="next-steps"></a>后续步骤

在本快速入门中，创建了 Key Vault 并在其中存储了一个机密。 若要详细了解 Key Vault 以及如何将其与应用程序集成，请继续阅读以下文章。

- 阅读 [Azure Key Vault 概述](../general/overview.md)
- 阅读[保护对 Key Vault 的访问](../general/secure-your-key-vault.md)
- 参阅[将 Key Vault 与应用服务 Web 应用结合使用](../general/tutorial-net-create-vault-azure-web-app.md)
- 参阅[将 Key Vault 与部署到 VM 的应用程序结合使用](../general/tutorial-net-virtual-machine.md)
- 参阅 [Azure Key Vault 开发人员指南](../general/developers-guide.md)
- 查看 [Azure Key Vault 最佳做法](../general/best-practices.md)
