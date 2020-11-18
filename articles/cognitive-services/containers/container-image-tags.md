---
title: 认知服务容器映像标记
titleSuffix: Azure Cognitive Services
description: 所有认知服务容器映像标记的全面列表。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: ba51776942ad28fc8d4b0db7dd2d0e162e5322b5
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94743331"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure 认知服务容器图像标记和发行说明

Azure 认知服务提供了许多容器映像。 容器注册表和相应的存储库因容器映像而异。 每个容器映像名称都提供多个标记。 容器映像标记是对容器映像进行版本控制的机制。 本文旨在作为综合性的参考资料使用，其中列出了所有认知服务容器映像及其可用标记。

> [!TIP]
> 在使用 [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) 时，请密切注意容器注册表、存储库、容器映像名称和相应标记的大小写，因为它们是区分大小写的。

## <a name="anomaly-detector"></a>异常检测器

可以在容器注册表联合中找到 [异常探测器][ad-containers] 容器映像 `mcr.microsoft.com` 。 它驻留在 `azure-cognitive-services/decision` 存储库中，名为 `anomaly-detector`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>读取 OCR (光学字符识别) 

可以在容器注册表联合中找到 [计算机视觉][cv-containers] 读取 OCR 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services` 存储库中，名为 `read`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/vision/read`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `v2.0.013250001-amd64-preview` ：

* 进一步降低容器的内存使用率。
* 对于多 pod 安装程序，外部缓存是必需的。 例如，设置 Redis 以用于缓存。
* 修复了 Redis 缓存设置为0时丢失的结果 `ResultExpirationPeriod` 。
* 删除26MB 的请求正文大小限制。 容器现在可以接受大于 26MB 的文件。
* 向控制台日志记录添加时间戳和生成版本。

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `2.0.013250001-amd64-preview` |  |

# <a name="previous-versions"></a>[旧版](#tab/previous)

的发行说明 `1.1.013050001-amd64-preview`

* 添加了 `ReadEngineConfig:ResultExpirationPeriod` 用于指定系统何时应清除识别结果的容器初始化配置。 
    * 此设置为小时，默认值为48小时。
    * 该设置可减少用于存储结果的内存使用量，尤其是在使用容器内存中存储时。
    * 示例 1。 ReadEngineConfig:ResultExpirationPeriod=1，系统将在进程结束后 1 小时清除识别结果。
    * 如果将此配置设置为0，则系统会在检索结果后清除识别结果。

* 修复了在向系统传递无效的图像格式时出现500内部服务器错误。 现在，它将会返回 400 错误：

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>表单识别器

可以在容器注册表 "联合" 中找到 [窗体识别器][fr-containers] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/custom-form` 存储库中，名为 `labeltool`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>语言理解 (LUIS)

在 `mcr.microsoft.com` 容器注册表联合项中可以找到 [LUIS][lu-containers] 容器映像。 该映像驻留在 `azure-cognitive-services/language` 存储库中，名为 `luis`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/language/luis`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[先前版本](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>自定义语音转文本

可以在容器注册表 "联合" 中找到 [自定义语音到文本][sp-cstt] 的容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `custom-speech-to-text`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `2.6.0-amd64` ：

**功能**
* 支持 phraselist v2 
* 短语列表在以下区域设置中受支持：
    * en au
    * en-ca
    * en-gb
    * en
    * en-nz
    * zh-cn
    * zh-cn
* 支持自定义基本模型下载。 
    * `BaseModelLocale=<locale>`与命令一起 `docker run` 使用
* 完全迁移到 .NET 3。1

**修复项**
* 修复了在 Diarization 模式下置信分数始终为1的问题
* 已迁移到 TextAnalytics 3.0 api

请注意，由于包含短语列表，此容器映像的大小已增加。

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `2.6.0-amd64`                 |       |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `2.5.0-amd64` ：

**功能**
* 支持自定义模型上的自定义发音
* 支持 Azure 和 Azure 美国政府云

**修复项**
* 在 Diarization 模式下修复作为非超级用户运行的用户问题

| 映像标记                    | 说明               |
|-------------------------------|:--------------------|
| `2.5.0-amd64`                 |   第一 GA 版本    |

---

## <a name="custom-text-to-speech"></a>自定义文本到语音转换

可在容器注册表 "联合" 中找到 [自定义的文本到语音][sp-ctts] 容器图像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `custom-text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `1.8.0-amd64` ：

**功能**
* 完全迁移到 .NET 3。1

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.8.0-amd64`                 |       |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `1.7.0-amd64` ：

**功能**
* 部分迁移到 .NET 3。1

| 映像标记                    | 说明               |
|-------------------------------|:--------------------|
| `1.7.0-amd64`                 |   第一 GA 版本    |

---

## <a name="speech-to-text"></a>语音转文本

" [语音到文本][sp-stt] " 容器映像可在容器注册表 "联合" 中找到 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `speech-to-text`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`。 可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list)的完整列表。

由于从语音到文本的 v 2.5.0， *美国政府弗吉尼亚州* 区域支持映像。 使用此区域时，请使用 *美国政府弗吉尼亚* 帐单终结点和 API 密钥。

# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `2.6.0-amd64-<locale>` ：

**功能**
* 升级到最新模型并完全迁移到 .NET 3。1
* 支持 phraselist v2
* 短语列表在以下区域设置中受支持：
    * en au
    * en-ca
    * en-gb
    * en
    * en-nz
    * zh-cn
    * zh-cn
* 支持新的区域设置 `cs-CZ` 
    * 当前不支持大写和标点。

**修复项**
* 修复了 Diarization 模式下置信分数始终为1的问题
* 已迁移使用 TextAnalytics 3.0 API

请注意，由于包含短语列表，此容器映像的大小已增加。 

| 映像标记                    | 说明                                                                                                | 
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | 具有区域设置的容器映像 `en-US` 。                                                             |
| `2.6.0-amd64-<locale>`        | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`2.6.0-amd64-en-us`。 |

此容器具有以下可用的区域设置。

| V 2.6.0 的区域设置           | 备注                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | 具有区域设置的容器映像 `ar-AE` 。 |
| `ar-eg`                     | 具有区域设置的容器映像 `ar-EG` 。 |
| `ar-kw`                     | 具有区域设置的容器映像 `ar-KW` 。 |
| `ar-qa`                     | 具有区域设置的容器映像 `ar-QA` 。 |
| `ar-sa`                     | 具有区域设置的容器映像 `ar-SA` 。 |
| `ca-es`                     | 具有区域设置的容器映像 `ca-ES` 。 |
| `cs-cz`                     | 具有区域设置的容器映像 `cs-CZ` 。 |
| `da-dk`                     | 具有区域设置的容器映像 `da-DK` 。 |
| `de-de`                     | 具有区域设置的容器映像 `de-DE` 。 |
| `en-au`                     | 具有区域设置的容器映像 `en-AU` 。 |
| `en-ca`                     | 具有区域设置的容器映像 `en-CA` 。 |
| `en-gb`                     | 具有区域设置的容器映像 `en-GB` 。 |
| `en-in`                     | 具有区域设置的容器映像 `en-IN` 。 |
| `en-nz`                     | 具有区域设置的容器映像 `en-NZ` 。 |
| `en-us`                     | 具有区域设置的容器映像 `en-US` 。 |
| `es-es`                     | 具有区域设置的容器映像 `es-ES` 。 |
| `es-mx`                     | 具有区域设置的容器映像 `es-MX` 。 |
| `fi-fi`                     | 具有区域设置的容器映像 `fi-FI` 。 |
| `fr-ca`                     | 具有区域设置的容器映像 `fr-CA` 。 |
| `fr-fr`                     | 具有区域设置的容器映像 `fr-FR` 。 |
| `gu-in`                     | 具有区域设置的容器映像 `gu-IN` 。 |
| `hi-in`                     | 具有区域设置的容器映像 `hi-IN` 。 |
| `it-it`                     | 具有区域设置的容器映像 `it-IT` 。 |
| `ja-jp`                     | 具有区域设置的容器映像 `ja-JP` 。 |
| `ko-kr`                     | 具有区域设置的容器映像 `ko-KR` 。 |
| `mr-in`                     | 具有区域设置的容器映像 `mr-IN` 。 |
| `nb-no`                     | 具有区域设置的容器映像 `nb-NO` 。 |
| `nl-nl`                     | 具有区域设置的容器映像 `nl-NL` 。 |
| `pl-pl`                     | 具有区域设置的容器映像 `pl-PL` 。 |
| `pt-br`                     | 具有区域设置的容器映像 `pt-BR` 。 |
| `pt-pt`                     | 具有区域设置的容器映像 `pt-PT` 。 |
| `ru-ru`                     | 具有区域设置的容器映像 `ru-RU` 。 |
| `sv-se`                     | 具有区域设置的容器映像 `sv-SE` 。 |
| `ta-in`                     | 具有区域设置的容器映像 `ta-IN` 。 |
| `te-in`                     | 具有区域设置的容器映像 `te-IN` 。 |
| `th-th`                     | 具有区域设置的容器映像 `th-TH` 。 |
| `tr-tr`                     | 具有区域设置的容器映像 `tr-TR` 。 |
| `zh-cn`                     | 具有区域设置的容器映像 `zh-CN` 。 |
| `zh-hk`                     | 具有区域设置的容器映像 `zh-HK` 。 |
| `zh-tw`                     | 具有区域设置的容器映像 `zh-TW` 。 |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `2.5.0-amd64-<locale>` ：

**功能**
* 支持 Azure 美国政府云

**修复项**
* 修复了在 Diarization 模式下作为非根用户运行的问题

| 映像标记                  | 说明                                    |
|-----------------------------|:-----------------------------------------|
| `2.5.0-amd64-<locale>`      | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`2.5.0-amd64-en-us`。  |

此容器具有以下可用的区域设置。

| V 2.5.0 的区域设置           | 备注                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | 具有区域设置的容器映像 `ar-AE` 。 |
| `ar-eg`                     | 具有区域设置的容器映像 `ar-EG` 。 |
| `ar-kw`                     | 具有区域设置的容器映像 `ar-KW` 。 |
| `ar-qa`                     | 具有区域设置的容器映像 `ar-QA` 。 |
| `ar-sa`                     | 具有区域设置的容器映像 `ar-SA` 。 |
| `ca-es`                     | 具有区域设置的容器映像 `ca-ES` 。 |
| `da-dk`                     | 具有区域设置的容器映像 `da-DK` 。 |
| `de-de`                     | 具有区域设置的容器映像 `de-DE` 。 |
| `en-au`                     | 具有区域设置的容器映像 `en-AU` 。 |
| `en-ca`                     | 具有区域设置的容器映像 `en-CA` 。 |
| `en-gb`                     | 具有区域设置的容器映像 `en-GB` 。 |
| `en-in`                     | 具有区域设置的容器映像 `en-IN` 。 |
| `en-nz`                     | 具有区域设置的容器映像 `en-NZ` 。 |
| `en-us`                     | 具有区域设置的容器映像 `en-US` 。 |
| `es-es`                     | 具有区域设置的容器映像 `es-ES` 。 |
| `es-mx`                     | 具有区域设置的容器映像 `es-MX` 。 |
| `fi-fi`                     | 具有区域设置的容器映像 `fi-FI` 。 |
| `fr-ca`                     | 具有区域设置的容器映像 `fr-CA` 。 |
| `fr-fr`                     | 具有区域设置的容器映像 `fr-FR` 。 |
| `gu-in`                     | 具有区域设置的容器映像 `gu-IN` 。 |
| `hi-in`                     | 具有区域设置的容器映像 `hi-IN` 。 |
| `it-it`                     | 具有区域设置的容器映像 `it-IT` 。 |
| `ja-jp`                     | 具有区域设置的容器映像 `ja-JP` 。 |
| `ko-kr`                     | 具有区域设置的容器映像 `ko-KR` 。 |
| `mr-in`                     | 具有区域设置的容器映像 `mr-IN` 。 |
| `nb-no`                     | 具有区域设置的容器映像 `nb-NO` 。 |
| `nl-nl`                     | 具有区域设置的容器映像 `nl-NL` 。 |
| `pl-pl`                     | 具有区域设置的容器映像 `pl-PL` 。 |
| `pt-br`                     | 具有区域设置的容器映像 `pt-BR` 。 |
| `pt-pt`                     | 具有区域设置的容器映像 `pt-PT` 。 |
| `ru-ru`                     | 具有区域设置的容器映像 `ru-RU` 。 |
| `sv-se`                     | 具有区域设置的容器映像 `sv-SE` 。 |
| `ta-in`                     | 具有区域设置的容器映像 `ta-IN` 。 |
| `te-in`                     | 具有区域设置的容器映像 `te-IN` 。 |
| `th-th`                     | 具有区域设置的容器映像 `th-TH` 。 |
| `tr-tr`                     | 具有区域设置的容器映像 `tr-TR` 。 |
| `zh-cn`                     | 具有区域设置的容器映像 `zh-CN` 。 |
| `zh-hk`                     | 具有区域设置的容器映像 `zh-HK` 。 |
| `zh-tw`                     | 具有区域设置的容器映像 `zh-TW` 。 |

---

## <a name="text-to-speech"></a>文本转语音

可在容器注册表 "联合" 中找到 [文本到语音的][sp-tts] 容器图像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `1.8.0-amd64-<locale-and-voice>` ：

**功能**
* 完全迁移到 .NET 3。1

| 映像标记                                  | 说明                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。                                            | 
| `1.8.0-amd64-<locale-and-voice>`            | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.8.0-amd64-en-us-ariarus`。  |


| V 1.8.0 的区域设置                          | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | 具有 `ar-EG` 区域设置和 `ar-EG-Hoda` 语音的容器映像。            |
| `ar-sa-naayf`                               | 具有 `ar-SA` 区域设置和 `ar-SA-Naayf` 语音的容器映像。           |
| `bg-bg-ivan`                                | 具有 `bg-BG` 区域设置和 `bg-BG-Ivan` 语音的容器映像。            |
| `ca-es-herenarus`                           | 具有 `ca-ES` 区域设置和 `ca-ES-HerenaRUS` 语音的容器映像。       |
| `cs-cz-jakub`                               | 具有 `cs-CZ` 区域设置和 `cs-CZ-Jakub` 语音的容器映像。           |
| `da-dk-hellerus`                            | 具有 `da-DK` 区域设置和 `da-DK-HelleRUS` 语音的容器映像。        |
| `de-at-michael`                             | 具有 `de-AT` 区域设置和 `de-AT-Michael` 语音的容器映像。         |
| `de-ch-karsten`                             | 具有 `de-CH` 区域设置和 `de-CH-Karsten` 语音的容器映像。         |
| `de-de-hedda`                               | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-heddarus`                            | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-stefan-apollo`                       | 具有 `de-DE` 区域设置和 `de-DE-Stefan-Apollo` 语音的容器映像。   |
| `el-gr-stefanos`                            | 具有 `el-GR` 区域设置和 `el-GR-Stefanos` 语音的容器映像。        |
| `en-au-catherine`                           | 具有 `en-AU` 区域设置和 `en-AU-Catherine` 语音的容器映像。       |
| `en-au-hayleyrus`                           | 具有 `en-AU` 区域设置和 `en-AU-HayleyRUS` 语音的容器映像。       |
| `en-ca-heatherrus`                          | 具有 `en-CA` 区域设置和 `en-CA-HeatherRUS` 语音的容器映像。      |
| `en-ca-linda`                               | 具有 `en-CA` 区域设置和 `en-CA-Linda` 语音的容器映像。           |
| `en-gb-george-apollo`                       | 具有 `en-GB` 区域设置和 `en-GB-George-Apollo` 语音的容器映像。   |
| `en-gb-hazelrus`                            | 具有 `en-GB` 区域设置和 `en-GB-HazelRUS` 语音的容器映像。        |
| `en-gb-susan-apollo`                        | 具有 `en-GB` 区域设置和 `en-GB-Susan-Apollo` 语音的容器映像。    |
| `en-ie-sean`                                | 具有 `en-IE` 区域设置和 `en-IE-Sean` 语音的容器映像。            |
| `en-in-heera-apollo`                        | 具有 `en-IN` 区域设置和 `en-IN-Heera-Apollo` 语音的容器映像。    |
| `en-in-priyarus`                            | 具有 `en-IN` 区域设置和 `en-IN-PriyaRUS` 语音的容器映像。        |
| `en-in-ravi-apollo`                         | 具有 `en-IN` 区域设置和 `en-IN-Ravi-Apollo` 语音的容器映像。     |
| `en-us-benjaminrus`                         | 具有 `en-US` 区域设置和 `en-US-BenjaminRUS` 语音的容器映像。     |
| `en-us-guy24krus`                           | 具有 `en-US` 区域设置和 `en-US-Guy24kRUS` 语音的容器映像。       |
| `en-us-aria24krus`                          | 具有 `en-US` 区域设置和 `en-US-Aria24kRUS` 语音的容器映像。      |
| `en-us-ariarus`                             | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。         |
| `en-us-zirarus`                             | 具有 `en-US` 区域设置和 `en-US-ZiraRUS` 语音的容器映像。         |
| `es-es-helenarus`                           | 具有 `es-ES` 区域设置和 `es-ES-HelenaRUS` 语音的容器映像。       |
| `es-es-laura-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Laura-Apollo` 语音的容器映像。    |
| `es-es-pablo-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Pablo-Apollo` 语音的容器映像。    |
| `es-mx-hildarus`                            | 具有 `es-MX` 区域设置和 `es-MX-HildaRUS` 语音的容器映像。        |
| `es-mx-raul-apollo`                         | 具有 `es-MX` 区域设置和 `es-MX-Raul-Apollo` 语音的容器映像。     |
| `fi-fi-heidirus`                            | 具有 `fi-FI` 区域设置和 `fi-FI-HeidiRUS` 语音的容器映像。        |
| `fr-ca-caroline`                            | 具有 `fr-CA` 区域设置和 `fr-CA-Caroline` 语音的容器映像。        |
| `fr-ca-harmonierus`                         | 具有 `fr-CA` 区域设置和 `fr-CA-HarmonieRUS` 语音的容器映像。     |
| `fr-ch-guillaume`                           | 具有 `fr-CH` 区域设置和 `fr-CH-Guillaume` 语音的容器映像。       |
| `fr-fr-hortenserus`                         | 具有 `fr-FR` 区域设置和 `fr-FR-HortenseRUS` 语音的容器映像。     |
| `fr-fr-julie-apollo`                        | 具有 `fr-FR` 区域设置和 `fr-FR-Julie-Apollo` 语音的容器映像。    |
| `fr-fr-paul-apollo`                         | 具有 `fr-FR` 区域设置和 `fr-FR-Paul-Apollo` 语音的容器映像。     |
| `he-il-asaf`                                | 具有 `he-IL` 区域设置和 `he-IL-Asaf` 语音的容器映像。            |
| `hi-in-hemant`                              | 具有 `hi-IN` 区域设置和 `hi-IN-Hemant` 语音的容器映像。          |
| `hi-in-kalpana-apollo`                      | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana-Apollo` 语音的容器映像。  |
| `hi-in-kalpana`                             | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana` 语音的容器映像。         |
| `hr-hr-matej`                               | 具有 `hr-HR` 区域设置和 `hr-HR-Matej` 语音的容器映像。           |
| `hu-hu-szabolcs`                            | 具有 `hu-HU` 区域设置和 `hu-HU-Szabolcs` 语音的容器映像。        |
| `id-id-andika`                              | 具有 `id-ID` 区域设置和 `id-ID-Andika` 语音的容器映像。          |
| `it-it-cosimo-apollo`                       | 具有 `it-IT` 区域设置和 `it-IT-Cosimo-Apollo` 语音的容器映像。   |
| `it-it-luciarus`                            | 具有 `it-IT` 区域设置和 `it-IT-LuciaRUS` 语音的容器映像。        |
| `ja-jp-ayumi-apollo`                        | 具有 `ja-JP` 区域设置和 `ja-JP-Ayumi-Apollo` 语音的容器映像。    |
| `ja-jp-harukarus`                           | 具有 `ja-JP` 区域设置和 `ja-JP-HarukaRUS` 语音的容器映像。       |
| `ja-jp-ichiro-apollo`                       | 具有 `ja-JP` 区域设置和 `ja-JP-Ichiro-Apollo` 语音的容器映像。   |
| `ko-kr-heamirus`                            | 具有 `ko-KR` 区域设置和 `ko-KR-HeamiRUS` 语音的容器映像。        |
| `ms-my-rizwan`                              | 具有 `ms-MY` 区域设置和 `ms-MY-Rizwan` 语音的容器映像。          |
| `nb-no-huldarus`                            | 具有 `nb-NO` 区域设置和 `nb-NO-HuldaRUS` 语音的容器映像。        |
| `nl-nl-hannarus`                            | 具有 `nl-NL` 区域设置和 `nl-NL-HannaRUS` 语音的容器映像。        |
| `pl-pl-paulinarus`                          | 具有 `pl-PL` 区域设置和 `pl-PL-PaulinaRUS` 语音的容器映像。      |
| `pt-br-daniel-apollo`                       | 具有 `pt-BR` 区域设置和 `pt-BR-Daniel-Apollo` 语音的容器映像。   |
| `pt-br-heloisarus`                          | 具有 `pt-BR` 区域设置和 `pt-BR-HeloisaRUS` 语音的容器映像。      |
| `pt-pt-heliarus`                            | 具有 `pt-PT` 区域设置和 `pt-PT-HeliaRUS` 语音的容器映像。        |
| `ro-ro-andrei`                              | 具有 `ro-RO` 区域设置和 `ro-RO-Andrei` 语音的容器映像。          |
| `ru-ru-ekaterinarus`                        | 具有 `ru-RU` 区域设置和 `ru-RU-EkaterinaRUS` 语音的容器映像。    |
| `ru-ru-irina-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Irina-Apollo` 语音的容器映像。    |
| `ru-ru-pavel-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Pavel-Apollo` 语音的容器映像。    |
| `sk-sk-filip`                               | 具有 `sk-SK` 区域设置和 `sk-SK-Filip` 语音的容器映像。           |
| `sl-si-lado`                                | 具有 `sl-SI` 区域设置和 `sl-SI-Lado` 语音的容器映像。            |
| `sv-se-hedvigrus`                           | 具有 `sv-SE` 区域设置和 `sv-SE-HedvigRUS` 语音的容器映像。       |
| `ta-in-valluvar`                            | 具有 `ta-IN` 区域设置和 `ta-IN-Valluvar` 语音的容器映像。        |
| `te-in-chitra`                              | 具有 `te-IN` 区域设置和 `te-IN-Chitra` 语音的容器映像。          |
| `th-th-pattara`                             | 具有 `th-TH` 区域设置和 `th-TH-Pattara` 语音的容器映像。         |
| `tr-tr-sedarus`                             | 具有 `tr-TR` 区域设置和 `tr-TR-SedaRUS` 语音的容器映像。         |
| `vi-vn-an`                                  | 具有 `vi-VN` 区域设置和 `vi-VN-An` 语音的容器映像。              |
| `zh-cn-huihuirus`                           | 具有 `zh-CN` 区域设置和 `zh-CN-HuihuiRUS` 语音的容器映像。       |
| `zh-cn-kangkang-apollo`                     | 具有 `zh-CN` 区域设置和 `zh-CN-Kangkang-Apollo` 语音的容器映像。 |
| `zh-cn-yaoyao-apollo`                       | 具有 `zh-CN` 区域设置和 `zh-CN-Yaoyao-Apollo` 语音的容器映像。   |
| `zh-hk-danny-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Danny-Apollo` 语音的容器映像。    |
| `zh-hk-tracy-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Tracy-Apollo` 语音的容器映像。    |
| `zh-hk-tracyrus`                            | 具有 `zh-HK` 区域设置和 `zh-HK-TracyRUS` 语音的容器映像。        |
| `zh-tw-hanhanrus`                           | 具有 `zh-TW` 区域设置和 `zh-TW-HanHanRUS` 语音的容器映像。       |
| `zh-tw-yating-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Yating-Apollo` 语音的容器映像。   |
| `zh-tw-zhiwei-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Zhiwei-Apollo` 语音的容器映像。   |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `1.7.0-amd64-<locale-and-voice>` ：

**功能**
* 将组件升级到 .NET 3。1

| 映像标记                                  | 说明                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.7.0-amd64-<locale-and-voice>`            | 第一 GA 版本。 `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.7.0-amd64-en-us-ariarus`。  |


| V 1.7.0 的区域设置                          | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | 具有 `ar-EG` 区域设置和 `ar-EG-Hoda` 语音的容器映像。            |
| `ar-sa-naayf`                               | 具有 `ar-SA` 区域设置和 `ar-SA-Naayf` 语音的容器映像。           |
| `bg-bg-ivan`                                | 具有 `bg-BG` 区域设置和 `bg-BG-Ivan` 语音的容器映像。            |
| `ca-es-herenarus`                           | 具有 `ca-ES` 区域设置和 `ca-ES-HerenaRUS` 语音的容器映像。       |
| `cs-cz-jakub`                               | 具有 `cs-CZ` 区域设置和 `cs-CZ-Jakub` 语音的容器映像。           |
| `da-dk-hellerus`                            | 具有 `da-DK` 区域设置和 `da-DK-HelleRUS` 语音的容器映像。        |
| `de-at-michael`                             | 具有 `de-AT` 区域设置和 `de-AT-Michael` 语音的容器映像。         |
| `de-ch-karsten`                             | 具有 `de-CH` 区域设置和 `de-CH-Karsten` 语音的容器映像。         |
| `de-de-hedda`                               | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-heddarus`                            | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-stefan-apollo`                       | 具有 `de-DE` 区域设置和 `de-DE-Stefan-Apollo` 语音的容器映像。   |
| `el-gr-stefanos`                            | 具有 `el-GR` 区域设置和 `el-GR-Stefanos` 语音的容器映像。        |
| `en-au-catherine`                           | 具有 `en-AU` 区域设置和 `en-AU-Catherine` 语音的容器映像。       |
| `en-au-hayleyrus`                           | 具有 `en-AU` 区域设置和 `en-AU-HayleyRUS` 语音的容器映像。       |
| `en-ca-heatherrus`                          | 具有 `en-CA` 区域设置和 `en-CA-HeatherRUS` 语音的容器映像。      |
| `en-ca-linda`                               | 具有 `en-CA` 区域设置和 `en-CA-Linda` 语音的容器映像。           |
| `en-gb-george-apollo`                       | 具有 `en-GB` 区域设置和 `en-GB-George-Apollo` 语音的容器映像。   |
| `en-gb-hazelrus`                            | 具有 `en-GB` 区域设置和 `en-GB-HazelRUS` 语音的容器映像。        |
| `en-gb-susan-apollo`                        | 具有 `en-GB` 区域设置和 `en-GB-Susan-Apollo` 语音的容器映像。    |
| `en-ie-sean`                                | 具有 `en-IE` 区域设置和 `en-IE-Sean` 语音的容器映像。            |
| `en-in-heera-apollo`                        | 具有 `en-IN` 区域设置和 `en-IN-Heera-Apollo` 语音的容器映像。    |
| `en-in-priyarus`                            | 具有 `en-IN` 区域设置和 `en-IN-PriyaRUS` 语音的容器映像。        |
| `en-in-ravi-apollo`                         | 具有 `en-IN` 区域设置和 `en-IN-Ravi-Apollo` 语音的容器映像。     |
| `en-us-benjaminrus`                         | 具有 `en-US` 区域设置和 `en-US-BenjaminRUS` 语音的容器映像。     |
| `en-us-guy24krus`                           | 具有 `en-US` 区域设置和 `en-US-Guy24kRUS` 语音的容器映像。       |
| `en-us-aria24krus`                          | 具有 `en-US` 区域设置和 `en-US-Aria24kRUS` 语音的容器映像。      |
| `en-us-ariarus`                             | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。         |
| `en-us-zirarus`                             | 具有 `en-US` 区域设置和 `en-US-ZiraRUS` 语音的容器映像。         |
| `es-es-helenarus`                           | 具有 `es-ES` 区域设置和 `es-ES-HelenaRUS` 语音的容器映像。       |
| `es-es-laura-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Laura-Apollo` 语音的容器映像。    |
| `es-es-pablo-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Pablo-Apollo` 语音的容器映像。    |
| `es-mx-hildarus`                            | 具有 `es-MX` 区域设置和 `es-MX-HildaRUS` 语音的容器映像。        |
| `es-mx-raul-apollo`                         | 具有 `es-MX` 区域设置和 `es-MX-Raul-Apollo` 语音的容器映像。     |
| `fi-fi-heidirus`                            | 具有 `fi-FI` 区域设置和 `fi-FI-HeidiRUS` 语音的容器映像。        |
| `fr-ca-caroline`                            | 具有 `fr-CA` 区域设置和 `fr-CA-Caroline` 语音的容器映像。        |
| `fr-ca-harmonierus`                         | 具有 `fr-CA` 区域设置和 `fr-CA-HarmonieRUS` 语音的容器映像。     |
| `fr-ch-guillaume`                           | 具有 `fr-CH` 区域设置和 `fr-CH-Guillaume` 语音的容器映像。       |
| `fr-fr-hortenserus`                         | 具有 `fr-FR` 区域设置和 `fr-FR-HortenseRUS` 语音的容器映像。     |
| `fr-fr-julie-apollo`                        | 具有 `fr-FR` 区域设置和 `fr-FR-Julie-Apollo` 语音的容器映像。    |
| `fr-fr-paul-apollo`                         | 具有 `fr-FR` 区域设置和 `fr-FR-Paul-Apollo` 语音的容器映像。     |
| `he-il-asaf`                                | 具有 `he-IL` 区域设置和 `he-IL-Asaf` 语音的容器映像。            |
| `hi-in-hemant`                              | 具有 `hi-IN` 区域设置和 `hi-IN-Hemant` 语音的容器映像。          |
| `hi-in-kalpana-apollo`                      | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana-Apollo` 语音的容器映像。  |
| `hi-in-kalpana`                             | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana` 语音的容器映像。         |
| `hr-hr-matej`                               | 具有 `hr-HR` 区域设置和 `hr-HR-Matej` 语音的容器映像。           |
| `hu-hu-szabolcs`                            | 具有 `hu-HU` 区域设置和 `hu-HU-Szabolcs` 语音的容器映像。        |
| `id-id-andika`                              | 具有 `id-ID` 区域设置和 `id-ID-Andika` 语音的容器映像。          |
| `it-it-cosimo-apollo`                       | 具有 `it-IT` 区域设置和 `it-IT-Cosimo-Apollo` 语音的容器映像。   |
| `it-it-luciarus`                            | 具有 `it-IT` 区域设置和 `it-IT-LuciaRUS` 语音的容器映像。        |
| `ja-jp-ayumi-apollo`                        | 具有 `ja-JP` 区域设置和 `ja-JP-Ayumi-Apollo` 语音的容器映像。    |
| `ja-jp-harukarus`                           | 具有 `ja-JP` 区域设置和 `ja-JP-HarukaRUS` 语音的容器映像。       |
| `ja-jp-ichiro-apollo`                       | 具有 `ja-JP` 区域设置和 `ja-JP-Ichiro-Apollo` 语音的容器映像。   |
| `ko-kr-heamirus`                            | 具有 `ko-KR` 区域设置和 `ko-KR-HeamiRUS` 语音的容器映像。        |
| `ms-my-rizwan`                              | 具有 `ms-MY` 区域设置和 `ms-MY-Rizwan` 语音的容器映像。          |
| `nb-no-huldarus`                            | 具有 `nb-NO` 区域设置和 `nb-NO-HuldaRUS` 语音的容器映像。        |
| `nl-nl-hannarus`                            | 具有 `nl-NL` 区域设置和 `nl-NL-HannaRUS` 语音的容器映像。        |
| `pl-pl-paulinarus`                          | 具有 `pl-PL` 区域设置和 `pl-PL-PaulinaRUS` 语音的容器映像。      |
| `pt-br-daniel-apollo`                       | 具有 `pt-BR` 区域设置和 `pt-BR-Daniel-Apollo` 语音的容器映像。   |
| `pt-br-heloisarus`                          | 具有 `pt-BR` 区域设置和 `pt-BR-HeloisaRUS` 语音的容器映像。      |
| `pt-pt-heliarus`                            | 具有 `pt-PT` 区域设置和 `pt-PT-HeliaRUS` 语音的容器映像。        |
| `ro-ro-andrei`                              | 具有 `ro-RO` 区域设置和 `ro-RO-Andrei` 语音的容器映像。          |
| `ru-ru-ekaterinarus`                        | 具有 `ru-RU` 区域设置和 `ru-RU-EkaterinaRUS` 语音的容器映像。    |
| `ru-ru-irina-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Irina-Apollo` 语音的容器映像。    |
| `ru-ru-pavel-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Pavel-Apollo` 语音的容器映像。    |
| `sk-sk-filip`                               | 具有 `sk-SK` 区域设置和 `sk-SK-Filip` 语音的容器映像。           |
| `sl-si-lado`                                | 具有 `sl-SI` 区域设置和 `sl-SI-Lado` 语音的容器映像。            |
| `sv-se-hedvigrus`                           | 具有 `sv-SE` 区域设置和 `sv-SE-HedvigRUS` 语音的容器映像。       |
| `ta-in-valluvar`                            | 具有 `ta-IN` 区域设置和 `ta-IN-Valluvar` 语音的容器映像。        |
| `te-in-chitra`                              | 具有 `te-IN` 区域设置和 `te-IN-Chitra` 语音的容器映像。          |
| `th-th-pattara`                             | 具有 `th-TH` 区域设置和 `th-TH-Pattara` 语音的容器映像。         |
| `tr-tr-sedarus`                             | 具有 `tr-TR` 区域设置和 `tr-TR-SedaRUS` 语音的容器映像。         |
| `vi-vn-an`                                  | 具有 `vi-VN` 区域设置和 `vi-VN-An` 语音的容器映像。              |
| `zh-cn-huihuirus`                           | 具有 `zh-CN` 区域设置和 `zh-CN-HuihuiRUS` 语音的容器映像。       |
| `zh-cn-kangkang-apollo`                     | 具有 `zh-CN` 区域设置和 `zh-CN-Kangkang-Apollo` 语音的容器映像。 |
| `zh-cn-yaoyao-apollo`                       | 具有 `zh-CN` 区域设置和 `zh-CN-Yaoyao-Apollo` 语音的容器映像。   |
| `zh-hk-danny-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Danny-Apollo` 语音的容器映像。    |
| `zh-hk-tracy-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Tracy-Apollo` 语音的容器映像。    |
| `zh-hk-tracyrus`                            | 具有 `zh-HK` 区域设置和 `zh-HK-TracyRUS` 语音的容器映像。        |
| `zh-tw-hanhanrus`                           | 具有 `zh-TW` 区域设置和 `zh-TW-HanHanRUS` 语音的容器映像。       |
| `zh-tw-yating-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Yating-Apollo` 语音的容器映像。   |
| `zh-tw-zhiwei-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Zhiwei-Apollo` 语音的容器映像。   |

---

## <a name="neural-text-to-speech"></a>神经文本转语音

可在容器注册表 "联合" 中找到 [神经文本到语音][sp-ntts] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `neural-text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `v1.3.0` ：
* 神经文本到语音转换容器现已正式发布。 

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `1.3.0-amd64-<locale-and-voice>`    | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.3.0-amd64-en-us-arianeural`。 |


| v 1.3.0 区域设置和语音           | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | 具有 `de-DE` 区域设置和 `de-DE-KatjaNeural` 语音的容器映像。     |
| `en-au-natashaneural`               | 具有 `en-AU` 区域设置和 `en-AU-NatashaNeural` 语音的容器映像。   |
| `en-ca-claraneural`                 | 具有 `en-CA` 区域设置和 `en-CA-ClaraNeural` 语音的容器映像。     |
| `en-gb-libbyneural`                 | 具有 `en-GB` 区域设置和 `en-GB-LibbyNeural` 语音的容器映像。     |
| `en-gb-mianeural`                   | 具有 `en-GB` 区域设置和 `en-GB-MiaNeural` 语音的容器映像。       |
| `en-us-arianeural`                  | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `en-us-guyneural`                   | 具有 `en-US` 区域设置和 `en-US-GuyNeural` 语音的容器映像。       |
| `es-es-elviraneural`                | 具有 `es-ES` 区域设置和 `es-ES-ElviraNeural` 语音的容器映像。    |
| `es-mx-dalianeural`                 | 具有 `es-MX` 区域设置和 `es-MX-DaliaNeural` 语音的容器映像。     |
| `fr-ca-sylvieneural`                | 具有 `fr-CA` 区域设置和 `fr-CA-SylvieNeural` 语音的容器映像。    |
| `fr-fr-deniseneural`                | 具有 `fr-FR` 区域设置和 `fr-FR-DeniseNeural` 语音的容器映像。    |
| `it-it-elsaneural`                  | 具有 `it-IT` 区域设置和 `it-IT-ElsaNeural` 语音的容器映像。      |
| `ja-jp-nanamineural`                | 具有 `ja-JP` 区域设置和 `ja-JP-NanamiNeural` 语音的容器映像。    |
| `ko-kr-sunhineural`                 | 具有 `ko-KR` 区域设置和 `ko-KR-SunHiNeural` 语音的容器映像。     |
| `pt-br-franciscaneural`             | 具有 `pt-BR` 区域设置和 `pt-BR-FranciscaNeural` 语音的容器映像。 |
| `zh-cn-xiaoxiaoneural`              | 具有 `zh-CN` 区域设置和 `zh-CN-XiaoxiaoNeural` 语音的容器映像。  |

# <a name="previous-version"></a>[先前版本](#tab/previous)

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.2.0-amd64-en-us-arianeural-preview`。 |


| 1.2.0 预览区域设置和语音           | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `de-de-katjaneural-preview`                 | 具有 `de-DE` 区域设置和 `de-DE-KatjaNeural` 语音的容器映像。     |
| `en-au-natashaneural-preview`               | 具有 `en-AU` 区域设置和 `en-AU-NatashaNeural` 语音的容器映像。   |
| `en-ca-claraneural-preview`                 | 具有 `en-CA` 区域设置和 `en-CA-ClaraNeural` 语音的容器映像。     |
| `en-gb-libbyneural-preview`                 | 具有 `en-GB` 区域设置和 `en-GB-LibbyNeural` 语音的容器映像。     |
| `en-gb-mianeural-preview`                   | 具有 `en-GB` 区域设置和 `en-GB-MiaNeural` 语音的容器映像。       |
| `en-us-arianeural-preview`                  | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `en-us-guyneural-preview`                   | 具有 `en-US` 区域设置和 `en-US-GuyNeural` 语音的容器映像。       |
| `es-es-elviraneural-preview`                | 具有 `es-ES` 区域设置和 `es-ES-ElviraNeural` 语音的容器映像。    |
| `es-mx-dalianeural-preview`                 | 具有 `es-MX` 区域设置和 `es-MX-DaliaNeural` 语音的容器映像。     |
| `fr-ca-sylvieneural-preview`                | 具有 `fr-CA` 区域设置和 `fr-CA-SylvieNeural` 语音的容器映像。    |
| `fr-fr-deniseneural-preview`                | 具有 `fr-FR` 区域设置和 `fr-FR-DeniseNeural` 语音的容器映像。    |
| `it-it-elsaneural-preview`                  | 具有 `it-IT` 区域设置和 `it-IT-ElsaNeural` 语音的容器映像。      |
| `ja-jp-nanamineural-preview`                | 具有 `ja-JP` 区域设置和 `ja-JP-NanamiNeural` 语音的容器映像。    |
| `ko-kr-sunhineural-preview`                 | 具有 `ko-KR` 区域设置和 `ko-KR-SunHiNeural` 语音的容器映像。     |
| `pt-br-franciscaneural-preview`             | 具有 `pt-BR` 区域设置和 `pt-BR-FranciscaNeural` 语音的容器映像。 |
| `zh-cn-xiaoxiaoneural-preview`              | 具有 `zh-CN` 区域设置和 `zh-CN-XiaoxiaoNeural` 语音的容器映像。  |

---

## <a name="speech-language-detection"></a>语音语言检测

可以在容器注册表联合中找到 [语音语言检测][sp-lid] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `language-detection`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list)的完整列表。

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>关键短语提取

容器映像可在容器注册表 "联合" 中找到 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `keyphrase`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)


| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>文本语言检测

在 `mcr.microsoft.com` 容器注册表联合项中可以找到[语言检测][ta-la]容器映像。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `language`。 完全限定的容器映像名称为， `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`


此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list)的完整列表。

# <a name="latest-versions"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[旧版](#tab/previous)


| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>情绪分析

在 `mcr.microsoft.com` 容器注册表联合项中可以找到[情绪分析][ta-se]容器映像。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `sentiment`。 完全限定的容器映像名称为， `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list)的完整列表。

| 映像标记 | 说明                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | 情绪分析 v3（英语）               |
| `3.0-es`   | 情绪分析 v3（西班牙语）               |
| `3.0-fr`   | 情绪分析 v3（法语）                |
| `3.0-it`   | 情绪分析 v3（意大利语）               |
| `3.0-de`   | 情绪分析 v3（德语）                |
| `3.0-zh`   | 情绪分析 v3（简体中文）  |
| `3.0-zht`  | 情绪分析 v3（繁体中文） |
| `3.0-ja`   | 情绪分析 v3（日语）              |
| `3.0-pt`   | 情绪分析 v3（葡萄牙语）            |
| `3.0-nl`   | 情绪分析 v3（荷兰语）                 |
| `2.1`    | 情绪分析 v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
