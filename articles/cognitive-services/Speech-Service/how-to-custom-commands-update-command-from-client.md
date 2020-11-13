---
title: 从客户端应用程序更新命令
titleSuffix: Azure Cognitive Services
description: 从客户端应用程序更新命令
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: encorona
ms.openlocfilehash: 290f9ee9c23071ac56b1ff0c65ddc03decbc7344
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94571159"
---
# <a name="update-a-command-from-the-client"></a>从客户端更新命令

本文介绍如何从客户端应用程序更新正在进行的命令。

## <a name="prerequisites"></a>先决条件
> [!div class = "checklist"]
> * 之前[创建的自定义命令应用](quickstart-custom-commands-application.md)

## <a name="update-the-state-of-a-command"></a>更新命令的状态

如果客户端应用程序需要在不使用语音输入的情况下更新正在进行的命令的状态，则可以发送事件以更新命令。

为了说明这种情况，若要 (TurnOnOff 更新正在进行的命令的状态) 我们可以发送以下事件活动。 

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "updatedCommand": {
      "name": "TurnOnOff",
      "updatedParameters": {
        "OnOff": "on"
      },
      "cancel": false
    },
    "updatedGlobalParameters": {},
    "processTurn": true
  }
}
```

允许查看此活动的关键属性。

| Attribute | 说明 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| type | 该活动的类型为 "事件"，事件的名称必须为 "RemoteUpdate"。 |
| **value** | 属性 "value" 包含更新当前命令所需的属性。 |
| **updatedCommand** | 特性 "updatedCommand" 包含命令的名称，"updatedParameters" 是带有参数名称及其更新值的映射。 |
| **cancel** | 如果需要取消正在进行的命令，请将属性 "cancel" 设置为 true。 |
| **updatedGlobalParameters** | 特性 "updatedGlobalParameters" 也是一个映射，与 "updatedParameters" 相同，但用于全局参数。 |
| **processTurn** | 如果需要在发送活动后处理 turn，请将 "processTurn" 属性设置为 true。 |

可在自定义命令门户中测试此方案。

1. 打开之前创建的自定义命令应用程序。 
1. 单击 "训练"，然后单击 "测试"。
1. 发送 "turn"。
1. 打开侧边面板，然后单击 "活动编辑器"。
1. 键入并发送上一部分中指定的只有事件。
    > [!div class="mx-imgBorder"]
    > ![发送远程命令](media/custom-commands/send-remote-command-activity.png)

请注意，使用客户端（而不是语音或文本）中的活动将参数 "麦克风" 的值设置为 "on"。

## <a name="update-the-catalog-of-the-parameter-for-a-command"></a>更新命令的参数的目录

在配置参数的有效选项列表时，会为应用程序全局定义参数的值。 

在我们的示例中，SubjectDevice 参数将具有受支持的值的固定列表，而与会话无关。

如果需要将新条目添加到每个会话的参数目录中，则可以发送以下活动。

```json
{
  "type": "event",
  "name": "RemoteUpdate",
  "value": {
    "catalogUpdate": {
      "commandParameterCatalogs": {
        "TurnOnOff": [
          {
            "name": "SubjectDevice",
            "values": {
              "stereo": [
                "cd player"
              ]
            }
          }
        ]
      }
    },
    "processTurn": false
  }
}
```
使用此活动时，我们将 "立体声" 的条目添加到命令 "TurnOnOff" 中的参数 "SubjectDevice" 的目录。

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-commands/update-catalog-with-remote-activity.png" alt-text="更新目录":::

请注意几个问题。
1. 只需在启动连接) 后 (最好地发送此活动。
1. 发送此活动后，应等待事件 ParameterCatalogsUpdated 发送回客户端。

## <a name="add-additional-context-from-the-client-application"></a>添加客户端应用程序中的其他上下文

可以从客户端应用程序为每个会话设置其他上下文，以后可在自定义命令应用程序中使用该上下文。 

例如，请考虑要将连接到自定义命令应用程序的设备的 ID 和名称发送到的方案。

若要测试此方案，请在当前的应用程序中创建新的命令。
1. 创建名为 GetDeviceInfo 的新命令。
1. 添加包含 "获取设备信息" 的示例句子。
1. 在完成规则 "完成" 中，添加 "发送语音响应" 操作。
    > ![通过上下文发送语音响应](media/custom-commands/send-speech-response-context.png)
1. 保存并训练您的应用程序。
1. 测试应用程序。
    > ![发送客户端上下文活动](media/custom-commands/send-client-context-activity.png)

请注意几个事项。
1. 只需在启动连接) 后 (最好地发送此活动。
1. 您可以使用 ClientContext 的复杂对象。
1. 你可以在语音响应中使用 ClientContext，以便发送活动和调用 web 终结点。


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [从 web 终结点更新命令](./how-to-custom-commands-update-command-from-web-endpoint.md)
