---
author: baanders
description: 有关在 Azure 数字孪生示例中为 DefaultAzureCredential 设置本地身份验证的 include 文件 - 带简介
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 35643370d6fd313d42f954a578b73befc025c040
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329540"
---
### <a name="set-up-local-azure-credentials"></a>设置本地 Azure 凭据

当你在本地计算机上运行示例时，此示例使用 [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet)（属于 `Azure.Identity` 库的一部分）对用户进行 Azure 数字孪生实例验证。 若要详细了解客户端应用可向 Azure 数字孪生进行验证的不同方法，请参阅[操作说明：编写应用身份验证码](../articles/digital-twins/how-to-authenticate-client.md)。

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]