---
title: 从 web 终结点更新命令
titleSuffix: Azure Cognitive Services
description: 从 web 终结点更新命令
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 4432843ac93002bc92068db191706352234d76e6
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94571177"
---
# <a name="update-a-command-from-a-web-endpoint"></a>从 web 终结点更新命令

如果客户端应用程序需要在不使用语音输入的情况下更新正在进行的命令的状态，则可以使用对 web 端点的调用来更新命令。

在本文中，你将了解如何从 web 终结点更新正在进行的命令。

## <a name="prerequisites"></a>先决条件
> [!div class = "checklist"]
> * 之前[创建的自定义命令应用](quickstart-custom-commands-application.md)

## <a name="create-an-azure-function"></a>创建 Azure 函数 

在此示例中，我们将需要一个支持以下输入 (的 HTTP-Triggered [Azure 函数](https://docs.microsoft.com/azure/azure-functions/) 或此输入) 的子集。

```JSON
{
  "conversationId": "SomeConversationId",
  "currentCommand": {
    "name": "SomeCommandName",
    "parameters": {
      "SomeParameterName": "SomeParameterValue",
      "SomeOtherParameterName": "SomeOtherParameterValue"
    }
  },
  "currentGlobalParameters": {
      "SomeGlobalParameterName": "SomeGlobalParameterValue",
      "SomeOtherGlobalParameterName": "SomeOtherGlobalParameterValue"
  }
}
```

允许查看此输入的键属性。

| Attribute | 说明 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **conversationId** | "conversationId" 是会话的唯一标识符，请注意，此 id 可以从客户端应用程序生成。 |
| **currentCommand** | "currentCommand" 是会话中当前活动的命令。 |
| **name** | "name" 是命令的名称，"parameters" 是包含参数的当前值的映射。 |
| **currentGlobalParameters** | "currentGlobalParameters" 也是类似于 "parameters" 但用于全局参数的映射。 |

Azure 函数的输出需要支持以下格式。

```JSON
{
  "updatedCommand": {
    "name": "SomeCommandName",
    "updatedParameters": {
      "SomeParameterName": "SomeParameterValue"
    },
    "cancel": false
  },
  "updatedGlobalParameters": {
    "SomeGlobalParameterName": "SomeGlobalParameterValue"
  }
}
```

你可能会认识到这种格式，因为它与 [更新客户端中的命令](./how-to-custom-commands-update-command-from-client.md)时使用的格式相同。 

现在，创建基于 NodeJS 的 Azure 函数并复制并粘贴此代码

```nodejs
module.exports = async function (context, req) {
    context.log(req.body);
    context.res = {
        body: {
            updatedCommand: {
                name: "IncrementCounter",
                updatedParameters: {
                    Counter: req.body.currentCommand.parameters.Counter + 1
                }
            }
        }
    };
}
```

当我们从自定义命令中调用此 Azure 函数时，我们将发送会话的当前值，我们将返回要更新的参数，或者如果我们要取消当前命令，则返回。

## <a name="update-the-existing-custom-commands-app"></a>更新现有的自定义命令应用

现在，让我们将 Azure 函数与现有的自定义命令应用程序挂钩。

1. 添加一个名为 IncrementCounter 的新命令。
1. 只添加一个值为 "增量" 的示例句子。
1. 添加一个名为 Counter 的新参数 (与上面的 Azure 函数中指定的名称) ，其默认值为0。
1. 添加一个名为 IncrementEndpoint 的新 Web 终结点，其中包含 Azure 函数的 URL 和启用了远程更新的。
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-web-endpoint-with-remote-updates.png" alt-text="设置具有远程更新的 web 终结点":::
1. 创建名为 "IncrementRule" 的新交互规则，并添加一个调用 web 终结点操作。
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-rule-web-endpoint.png" alt-text="递增规则":::
1. 在 "操作配置" 中，选择 "IncrementEndpoint"，将 "成功时配置" 发送具有 "计数器" 和 "失败" 值的语音响应，并提供错误消息。
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/set-increment-counter-call-endpoint.png" alt-text="设置增量计数器调用终结点":::
1. 设置规则的执行后状态以等待用户输入

## <a name="test-it"></a>测试

1. 保存并训练应用
1. 单击 "测试"
1. 发送几次 "递次" (这是 IncrementCounter 命令的示例句子) 
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-commands/increment-counter-example.png" alt-text="递增值计数器示例":::

请注意，Azure 函数每次转换时，Counter 参数的值如何递增。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [为自定义命令应用程序启用 CI/CD 过程](./how-to-custom-commands-deploy-cicd.md)