---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 029cb1e59876eb01c609ef8b7ba0412ed35a2a1b
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940731"
---
本快速入门介绍如何使用语音服务和 cURL 将文本转换为语音。

若要深入了解文本转语音的概念，请参阅[概述](../../../text-to-speech.md)一文。

## <a name="prerequisites"></a>先决条件

本文假定你有 Azure 帐户和语音服务订阅。 如果你没有帐户和订阅，[可以免费试用语音服务](../../../overview.md#try-the-speech-service-for-free)。

## <a name="convert-text-to-speech"></a>将文本转换为语音

请在命令提示符处运行以下命令。 需要将以下值插入到命令中。
- 语音服务订阅密钥。
- 你的语音服务区域。

可能还需要更改以下值。
- `X-Microsoft-OutputFormat` 标头值，该值控制音频输出格式。 可以在[文本转语音 REST API 参考](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech#audio-outputs)中找到受支持的音频输出格式列表。
- 输出的语音。 若要获取适用于语音终结点的语音列表，请参阅下一节。
- 输出文件。 在此示例中，我们将服务器的响应定向到名为 `output.wav` 的文件中。

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/text-to-speech.sh":::

## <a name="list-available-voices-for-your-speech-endpoint"></a>列出适用于语音终结点的语音

若要列出适用于语音终结点的语音，请运行以下命令。

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="request":::

应收到类似于下面的响应。

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="response":::
