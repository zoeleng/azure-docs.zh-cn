---
author: baanders
description: 有关在 Azure 数字孪生示例中为 DefaultAzureCredential 设置本地身份验证的 include 文件 - 无简介
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 8bf0590b867d37a22706f679bfbeee2140935637
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494702"
---
使用 `DefaultAzureCredential`，此示例将在本地环境中搜索凭据，如本地 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 或 Visual Studio/Visual Studio Code 中的 Azure 登录。 这意味着，你应该通过这些机制之一在本地登录 Azure，为示例设置凭据。

如果你使用 Visual Studio 或 Visual Studio Code 运行代码示例，请确保使用你要用于访问 Azure 数字孪生实例的相同 Azure 凭据登录到该编辑器。

否则，可[安装本地 Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)，在计算机上启动命令提示符，并运行 `az login` 命令以登录 Azure 帐户。 此后，当你运行代码示例时，它应自动使你登录。