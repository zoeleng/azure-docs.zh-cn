---
author: baanders
description: 有关在 Azure 数字孪生示例中为 DefaultAzureCredential 设置本地身份验证的 include 文件 - 无简介
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 3cb053f9673532ac19e2098678ec341f2f676486
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329503"
---
使用 `DefaultAzureCredential`，此示例将在本地环境中搜索凭据，如本地 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 或 Visual Studio/Visual Studio Code 中的 Azure 登录。 因此，应该通过这些机制在本地登录 Azure，以便设置示例的凭据。

如果使用 Visual Studio 或 Visual Studio Code 运行代码示例，请确保使用要用于访问 Azure 数字孪生实例的相同 Azure 凭据登录到该编辑器。

否则，可[安装本地 Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)，在计算机上启动命令提示符，并运行 `az login` 命令以登录 Azure 帐户。 登录后，运行代码示例时，它应自动使你登录。