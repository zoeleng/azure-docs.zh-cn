---
title: 为基于事件的视频录制配置信号入口-Azure
description: 本文介绍如何在 media graph 中配置信号入口。
ms.topic: how-to
ms.date: 11/3/2020
ms.openlocfilehash: 4204a43915f9c79cae7dfe82b37e741fee89fb4a
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380122"
---
# <a name="configure-a-signal-gate-for-event-based-video-recording"></a>为基于事件的视频录制配置信号入口

在媒体图中，当事件触发入口时， [信号入口处理器节点](media-graph-concept.md#signal-gate-processor) 允许您将媒体从一个节点转发到另一个节点。 触发时，入口会打开，并允许媒体流过指定的持续时间。 如果没有触发入口的事件，入口会关闭，media 停止流动。 信号入口处理器适用于基于事件的视频录制。
在本文中，你将了解有关如何配置信号入口处理器的详细信息。

## <a name="suggested-pre-reading"></a>建议的读前准备
-   [媒体图](media-graph-concept.md)
-   [基于事件的视频录制](event-based-video-recording-concept.md)


## <a name="problem"></a>问题
用户可能需要在事件触发入口之前或之后开始记录特定的时间。 用户在其系统中知道可接受的延迟，因此用户需要指定信号入口处理器的滞后时间。 用户需要指定最短和最长时间，无论接收多少个新事件，其录制的持续时间都是如此。
 
### <a name="use-case-scenario"></a>用例场景
假设你想要在每次打开大楼的前门时录制视频。 您希望在记录中包含打开门之前的 **X** 秒。 如果没有再次打开门，则需要记录至少为 **Y** 秒。 如果要反复打开门，则需要记录最多 **Z** 秒。 你知道，你的门传感器的延迟为 **K** 秒，并且想要降低事件被忽略的几率 ( "延迟到达" ) ，因此你希望至少使用 **K** 秒来接收事件。


## <a name="solution"></a>解决方案

**_修改信号入口处理器参数_* _

信号入口处理器由4个参数配置：
- _ *激活评估窗口**
- **激活信号偏移**
- **最小激活窗口**
- **最大激活窗口** 。 

触发信号入口处理器时，它将保持打开状态，以达到最小激活时间。 激活事件从最早事件的时间戳开始，加上激活信号偏移量。 如果再次触发了信号入口处理器（当它处于打开状态时），则计时器将重置，并且入口将保持打开状态至少最短的激活时间。 信号入口处理器绝不会保持打开时间超过最长激活时间。 事件 **1) 事件 1** 发生在另一个事件 **(事件 2)** 之前，如果系统延迟，并且 **事件 1** 在 **事件 2** 之后到达信号入口处理器，则该事件 (发生。 如果事件 **1** 未到达 **事件 2** 到达和 **激活评估窗口** 之间，则会忽略 **事件 1** ，而不会通过信号入口处理器进行传递。 为每个事件设置相关 Id。 这些 Id 是从初始事件设置的，并在每个事件中按顺序进行。

> [!IMPORTANT]
> 媒体时间基于媒体中发生事件时的媒体时间戳。 到达信号入口的事件序列可能不反映到达媒体时间的事件的顺序。


### <a name="parameters-based-on-when-events-arrive-in-physical-time-to-the-signal-gate"></a>参数：基于事件何时到达信号入口的物理时间 () 

* **minimumActivationTime (记录的最短持续时间)** = 在触发接收新事件后，信号入口处理器将保持打开状态的最小秒数，除非 **maximumActivationTime**
* **maximumActivationTime (记录的最长持续时间)** = 在触发接收新事件后，信号入口处理器将保持打开状态的最大秒数，而不管接收了哪些事件
* **activationSignalOffset** = 激活信号入口处理器与录像开始时间之间的秒数，此值通常为负值，以便在触发事件之前开始录制
* **activationEvaluationWindow** = 从初始触发事件开始的秒数，在此时间内，初始事件之前发生的事件（以媒体时间表示）必须在被忽略之前到达信号入口处理器，并被视为 "延迟到达"

> [!NOTE]
> "延迟到达" 是指在 "激活评估" 窗口已过去，但此事件到达媒体时间内的初始事件之前到达的任何事件。

### <a name="limits-of-parameters"></a>参数的限制

* **activationEvaluationWindow：0秒到10秒**

* **activationSignalOffset：-1 分钟到1分钟**

* **minimumActivationTime：1秒到1小时**

* **maximumActivationTime：1秒到1小时**


根据用例，将按如下所示设置参数：

* **activationEvaluationWindow = K 秒**
* **activationSignalOffset =-X 秒**
* **minimumActivationWindow = Y 秒**
* **maximumActivationWindow = Z 秒**


下面是有关以下参数值的媒体图拓扑中的信号入口处理器节点部分的示例：
* **activationEvaluationWindow = 1 秒**
* **activationSignalOffset =-5 秒**
* **minimumActivationTime = 20 秒**
* **maximumActivationTime = 40 秒**

> [!IMPORTANT]
> 每个参数值应有[ISO 8601 持续时间格式](https://en.wikipedia.org/wiki/ISO_8601#Durations
)。 
**例如： PT1S = 1 秒**


```
"processors":              
[
          {
            "@type": "#Microsoft.Media.MediaGraphSignalGateProcessor",
            "name": "signalGateProcessor",
            "inputs": [
              {
                "nodeName": "iotMessageSource"
              },
              {
                "nodeName": "rtspSource"
              }
            ],
            "activationEvaluationWindow": "PT1S",
            "activationSignalOffset": "-PT5S",
            "minimumActivationTime": "PT20S",
            "maximumActivationTime": "PT40S"
          }
]
```


接下来，让我们考虑一下此信号入口处理器配置在不同的录制方案中的行为方式。


**1个源的事件 ( *正常激活* )**

接收一个事件的信号入口处理器会导致一条记录，该记录在事件到达入口之前 (5) 秒。 记录的其余部分为 "最小激活时间" (20) 秒长，因为在最小激活时间之前没有其他事件到达重新触发入口。

示例关系图：
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/normal-activation.svg" alt-text="正常激活":::

* 记录的持续时间 =-偏移量 + minimumActivationTime = [E1 + offset，E1 + minimumActivationTime]


**1个源 ( *Retriggered 激活* 中的2个事件)**

收到两个事件的信号入口处理器会导致一条记录，该记录在第一个事件到达入口之前 (五) 秒。 第二个事件在第一个事件（早于 "最小激活时间"）之前的5秒内，第一个事件完成后 (20) 秒，因此 retriggered 的入口保持打开状态。 记录的其余部分为 "最小激活时间" (20) 秒长，因为在第二个事件的最小激活时间之前没有其他事件到达，重新触发入口。

示例关系图：
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/retriggering-activation.svg" alt-text="Retriggered 激活":::

* 记录的持续时间 =-第二个事件) + minimumActivationTime 的第二个事件到达时的偏移量 + (到达次数


**来自1个源的 N 个事件 ( *最大激活* )**

接收 N 个事件的信号入口处理器会导致一条记录，该记录在第一个事件到达入口之前 (5) 秒。 当每个事件在完成 "最小激活时间" 之前 (20) 秒（从上一个事件开始）后，在第一个事件（其中，入口将关闭，不再接受任何新事件）之前，该门将持续 retriggered 并保持打开状态，直到 "最大激活时间" () 40。

示例关系图：
> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/configure-signal-gate-how-to/maximum-activation.svg" alt-text="最大激活":::
 
* 记录的持续时间 =-偏移量 + maximumActivationTime

> [!IMPORTANT]
> 关系图假设每个事件在物理和媒体时间都到达同一个实例。  (没有延迟抵达) 

## <a name="next-steps"></a>后续步骤

### <a name="try-it-out"></a>试用

[基于事件的视频录制教程](event-based-video-recording-tutorial.md)

使用基于事件的视频录制教程， [ 在上编辑topology.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json)，修改 signalgateProcessor 节点的参数，然后按教程的其余部分进行操作。 查看视频录制，分析参数的效果。



