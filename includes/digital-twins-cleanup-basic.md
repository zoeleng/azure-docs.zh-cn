---
author: baanders
description: 包括用于清理基本 Azure 数字孪生实例的文件
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 29a07ffa917153c0cb062d34e2807d43d039a373
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494693"
---
如果不再需要本教程中创建的资源，请按照以下步骤将其删除。

利用 [Azure Cloud Shell](https://shell.azure.com)，你可以使用 [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) 命令删除资源组中的所有 Azure 资源。 这将删除资源组和 Azure 数字孪生实例。

> [!IMPORTANT]
> 删除资源组的操作不可逆。 资源组以及包含在其中的所有资源将被永久删除。 请确保不会意外删除错误的资源组或资源。 

打开 Azure Cloud Shell 并运行以下命令，以删除资源组及其包含的所有内容。

```azurecli-interactive
az group delete --name <your-resource-group>
```