---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: b83951cd743afc7e249e3ee37182007bbe40cb5c
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971260"
---
## <a name="create-a-spatial-anchors-resource"></a>创建空间定位点资源

转到 <a href="https://portal.azure.com" target="_blank">Azure 门户</a>。

在左窗格中，选择“创建资源”。

使用搜索框以搜索“空间定位点”。

![显示空间定位点搜索结果的屏幕截图。](./media/spatial-anchors-get-started-create-resource/portal-search.png)

选择“空间定位点”，然后选择“创建” 。

在“空间定位点帐户”窗格中，执行以下操作：

* 使用常规字母数字字符输入唯一的资源名称。  
* 选择想要将资源附加到的订阅。  
* 选择“新建”可创建资源组。 将其命名为 myResourceGroup，然后选择“确定” 。  

  [!INCLUDE [resource group intro text](resource-group.md)]
  
* 选择可在其中放置资源的位置（区域）。  
* 选择“新建”开始创建资源。

![用于创建资源的“空间定位点”窗格的屏幕截图。](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

创建资源后，Azure 门户显示部署已完成。 
   
![显示资源部署已完成的屏幕截图。](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

选择“转到资源”。  你现在可以查看资源属性。 
   
将资源的“帐户 ID”值复制到文本编辑器中，供稍后使用。

![“资源属性”窗格的屏幕截图。](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

另外，将资源的“帐户域”值复制到文本编辑器中，供稍后使用。

![显示资源的帐户域值的屏幕截图。](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

在“设置”下，选择“密钥” 。 将“帐户密钥”的“主密钥”值复制到文本编辑器中，供稍后使用 。

![帐户的“密钥”窗格的屏幕截图。](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
