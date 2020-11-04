---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 11/03/2020
ms.author: trbye
ms.openlocfilehash: 040ffea69f76255dcb1bfc6787cad45a95baa904
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93305908"
---
首先，使用静态函数加载关键字模型文件 `FromFile()` ，该函数将返回 `KeywordRecognitionModel` 。 使用 `.table` 从 Speech Studio 下载的文件的路径。 此外，你可以 `AudioConfig` 使用默认麦克风创建，然后使用音频配置实例化新的 `KeywordRecognizer` 。

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var keywordModel = KeywordRecognitionModel.FromFile("your/path/to/Activate_device.table");
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var keywordRecognizer = new KeywordRecognizer(audioConfig);
```

接下来，通过一次调用来运行关键字识别， `RecognizeOnceAsync()` 方法是传递模型对象。 这会启动一个关键字识别会话，此会话将一直持续到识别关键字为止。 因此，通常在多线程应用程序中使用此设计模式，或在可能会无限期等待唤醒字词的使用情况下使用此设计模式。

```csharp
KeywordRecognitionResult result = await keywordRecognizer.RecognizeOnceAsync(keywordModel);
```

> [!NOTE]
> 此处显示的示例使用本地关键字识别，因为它不需要 `SpeechConfig` 用于身份验证上下文的对象，也不会与后端联系。 但是，可以 [使用连续后端连接](https://docs.microsoft.com/azure/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk#view-the-source-code-that-enables-keyword)来运行关键字识别和验证。