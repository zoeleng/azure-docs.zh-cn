---
author: baanders
description: Azure 数字孪生的 include 文件 - 列举 Cloud Shell 身份验证的已知问题
ms.service: digital-twins
ms.topic: include
ms.date: 9/1/2020
ms.author: baanders
ms.openlocfilehash: 574ca9d42565e897e49d6800e61bb2c33717891b
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92321968"
---
>[!NOTE]
>目前，Cloud Shell 中存在一个已知问题，从 https://shell.azure.com 运行时，该问题会影响以下命令组：`az dt route` 、`az dt model` 和 `az dt twin`。
>
>要解决问题，可以执行以下任一操作：
> * 先在 Cloud Shell 中运行 `az login`，然后再运行命令。
> * 在 Azure 门户中打开 Cloud Shell 窗格，并从其中完成 Cloud Shell 工作。
>  :::image type="content" source="../articles/digital-twins/media/includes/portal-cloud-shell.png" alt-text="突出显示“Cloud Shell”图标的 Azure 门户视图，Cloud Shell 显示在门户窗口底部":::
> * 使用[本地 CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 而不是 Cloud Shell。
>
>有关此问题的更多详细信息，请参阅 [*故障排除：Azure 数字孪生中的已知问题*](../articles/digital-twins/troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell)。