---
title: 在 Azure Marketplace 上配置虚拟机产品/服务列表详细信息
description: 了解如何在 Azure Marketplace 上配置虚拟机产品/服务列表的详细信息。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: 2af96c0c2e850d8e1be67c23e71aa677773c1e03
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283694"
---
# <a name="how-to-configure-virtual-machine-offer-listing-details"></a>如何配置虚拟机产品/服务列表详细信息

在“产品/服务列表”页上定义产品/服务详细信息，例如产品/服务名称、说明、链接和联系人。

> [!NOTE]
> 你的产品/服务列表内容（如说明、文档、屏幕截图和使用条款）不一定要使用英语，只要产品/服务说明以短语开头，"此应用程序仅在中可用 \<non-English language> "。 还可以提供一个 URL，链接到使用与产品/服务列表内容中所用语言不同的语言提供内容的站点。

## <a name="marketplace-details"></a>市场详细信息

### <a name="name"></a>名称

在此处输入的名称作为产品/服务列表的标题向客户显示。 此字段自动填充了在创建产品/服务时在“产品/服务别名”框中输入的名称。 可以稍后更改此名称。 名称：

- 可以添加商标。 可以包括商标和版权符号。
- 不能超过 50 个字符。
- 不得包含表情符号。

### <a name="search-results-summary"></a>搜索结果摘要

提供要在 Azure 市场搜索结果中显示的产品/服务的简短说明。 最多可包含 100 个字符。

### <a name="long-summary"></a>长摘要

提供要在 Azure 市场搜索结果中显示的产品/服务的较长。 最多可包含 256 个字符。

### <a name="description"></a>说明

[!INCLUDE [Long description-1](includes/long-description-1.md)]

[!INCLUDE [Long description-2](includes/long-description-2.md)]

使用 HTML 标记设置说明的格式，以使其更具吸引力。 有关允许的标记的列表，请参阅 [受支持的 HTML 标记](supported-html-tags.md)。

### <a name="privacy-policy-link"></a>隐私策略链接

输入组织的隐私策略的 Web 地址 (URL)。 确保你的产品/服务符合隐私法律法规。 还必须在网站上发布有效的隐私策略。

## <a name="useful-links"></a>有用链接

提供有关你的产品/服务的补充联机文档。 若要添加链接，请选择“添加链接”，然后填写以下字段：

- **Name**：客户将在详细信息页上看到该名称。
- **链接 (URL)** ：输入链接，可供客户查看联机文档。

## <a name="customer-support-links"></a>客户支持链接

提供支持网站，客户可在其中联系支持团队。

- Azure 全球支持网站
- Azure 政府支持网站

## <a name="partner-support-contact"></a>合作伙伴支持联系人

提供客户打开支持票证时 Microsoft 合作伙伴要使用的联系信息。 Azure 市场中未列出此信息。

- 名称
- 电子邮件
- 电话

## <a name="engineering-contact"></a>工程联系人

提供产品/服务出现问题（包括认证问题）时 Microsoft 要使用的联系信息。 Azure 市场中未列出此信息。

- 名称
- 电子邮件
- 电话

## <a name="azure-marketplace-media"></a>Azure 市场媒体

提供要用于产品/服务的徽标和图像。 所有图像都必须为 PNG 格式。 模糊图像会导致提交遭到拒绝。

[!INCLUDE [logo tips](includes/graphics-suggestions.md)]

>[!NOTE]
>如果上传文件时遇到问题，请确保本地网络未阻止合作伙伴中心所使用的 https://upload.xboxlive.com 服务。

### <a name="azure-marketplace-logos"></a>Azure 市场徽标

提供 **大** 徽标徽标的 PNG 文件。 合作伙伴中心将使用此来创建 **小型** 和 **中型** 徽标。 稍后可以选择将它们替换为不同的图像。

- **大型** (从 216 x 216 到 350 x 350 px，必需) 
- **中型** (90 x 90 px，可选) 
- **小写** (48 x 48 px，可选) 

在列表中的不同位置使用这些徽标：

[!INCLUDE [logos-azure-marketplace-only](includes/logos-azure-marketplace-only.md)]

[!INCLUDE [Logo tips](includes/graphics-suggestions.md)]

### <a name="screenshots"></a>屏幕截图

最多添加五个显示了产品/服务的工作方式的屏幕截图。 每个屏幕截图大小必须为 1280 x 720 像素并采用 PNG 格式。 每个屏幕截图都必须包含一个标题。

### <a name="videos"></a>视频

最多添加五个视频来演示产品/服务。 这些视频应在外部视频服务上进行托管。 输入每个视频的名称、Web 地址和视频的缩略图 PNG 图像（为 1280 x 720 像素）。

有关其他市场列表资源，请参阅[市场产品/服务列表最佳做法](gtm-offer-listing-best-practices.md)。

选择“保存草稿”，然后继续。

## <a name="next-steps"></a>后续步骤

- [创建计划](azure-vm-create-plans.md)
