---
title: 查看和配置适用于 FHIR 的 Azure IoT 连接器 (预览) 指标
description: 本文介绍了如何为 FHIR (预览版) 指标显示和配置 Azure IoT 连接器
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/29/2020
ms.author: jasteppe
ms.openlocfilehash: 76166fc5c525d36474a585179472e93b22dad647
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133507"
---
# <a name="view-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>查看和配置适用于 FHIR 的 Azure IoT 连接器 (预览) 指标 

本文介绍如何查看和配置 Azure IoT Connector for FHIR * 指标。 

> [!TIP]
> 按照 [导出用于 FHIR 的 Azure IoT 连接器 (预览版中的指南) 指标通过诊断设置](https://docs.microsoft.com/azure/healthcare-apis/iot-metrics-diagnostics-export) 来了解如何设置数据的导出。

## <a name="view-metrics-for-azure-iot-connector-for-fhir-preview"></a>查看适用于 FHIR 的 Azure IoT Connector 的指标 (预览版) 
1. 若要查看 IoT 连接器的指标，请在 Azure 门户中选择 Azure API for FHIR 服务。 

2. 导航到 **指标** 

3. 选择 " **IoT 连接器** " 选项卡。

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. 选择 IoT 连接器以查看其指标 (例如：有与此 Azure API for FHIR service) 关联的 (4) IoT 连接器。

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

> [!NOTE]
> " **自定义** " 选项卡允许创建用于查看 IoT 连接器指标的特定时间/日期组合。

5. 选择要显示的 IoT Connector 指标的时间段 (例如：1小时、24小时、7天或自定义) 。

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metrics-types-for-azure-iot-connector-for-fhir-preview"></a>适用于 FHIR 的 Azure IoT Connector 的指标类型 (预览版)  
显示的 IoT 连接器指标如下：

|度量值类型|指标用途| 
|-----------|--------------|
|传入消息数|收到的原始传入消息数 (例如：设备事件) 。|
|规范化消息数|规范化消息的数目。|
|消息组数|在指定的时间范围内聚合了消息的组的数目。|
|平均标准化阶段延迟|规范化阶段的平均延迟。 规范化阶段是对原始传入消息执行规范化。|
|平均组阶段延迟|组阶段的平均延迟。 组阶段用于对规范化消息执行缓冲、聚合和分组。| 
|Total Error Count|错误总数。| 

## <a name="focusing-and-configuring-azure-iot-connector-for-fhir-preview-metrics"></a>集中和配置 Azure IoT Connector for FHIR (预览版) 指标
在此示例中，我们将重点介绍 **传入消息度量值的数量** 。

1. 选择要关注的时间点。

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. 在此屏幕中，你可以 **添加度量值** 、 **添加筛选器** 并 **应用拆分** 以实现进一步的自定义。 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>结论 
有权访问数据平面指标对于监视和故障排除至关重要。  适用于 FHIR 的 Azure IoT 连接器可帮助你通过指标执行这些操作。 

## <a name="next-steps"></a>后续步骤

查看有关适用于 FHIR 的 Azure IoT 连接器的常见问题。

>[!div class="nextstepaction"]
>[适用于 FHIR 常见问题的 Azure IoT 连接器](fhir-faq.md)

*在 Azure 门户中，适用于 FHIR 的 Azure IoT 连接器称为 IoT Connector（预览版）。

FHIR 是 HL7 的注册商标，经 HL7 许可使用。