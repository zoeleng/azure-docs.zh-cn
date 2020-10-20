---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 36d12b29054f736b65af5ac411adbc26d870b982
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014133"
---
本快速入门介绍如何使用语音服务和 cURL 将语音转换为文本。

若要深入了解语音转文本的概念，请参阅[概述](../../../speech-to-text.md)一文。

## <a name="prerequisites"></a>先决条件

本文假定你有 Azure 帐户和语音服务订阅。 如果你没有帐户和订阅，[可以免费试用语音服务](../../../overview.md#try-the-speech-service-for-free)。

## <a name="convert-speech-to-text"></a>将语音转换为文本

请在命令提示符处运行以下命令。 需要将以下值插入到命令中。
- 语音服务订阅密钥。
- 你的语音服务区域。
- 输入的音频文件路径。 可以使用[语音转文本](../../../get-started-text-to-speech.md)来生成音频文件。

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="request":::

应收到类似于下面的响应。

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="response":::

有关详细信息，请参阅[语音转文本 REST API 参考](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text)。
