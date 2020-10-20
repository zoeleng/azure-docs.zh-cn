---
title: 通过诊断设置导出用于 FHIR (预览) 指标的 Azure IoT 连接器
description: 本文介绍如何通过诊断设置导出适用于 FHIR (preview) 指标的 Azure IoT 连接器
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/16/2020
ms.author: jasteppe
ms.openlocfilehash: c81dcdd2e79f5d89a0766415b47ad118874e5ad2
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209457"
---
# <a name="export-azure-iot-connector-for-fhir-preview-metrics-through-diagnostic-settings"></a>通过诊断设置导出用于 FHIR (预览) 指标的 Azure IoT 连接器

本文介绍如何导出适用于 FHIR * 度量值日志的 Azure IoT 连接器。 启用指标日志记录的功能是 Azure 门户中的 [**诊断设置**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) 。 

> [!TIP]
> 按照在用于 [FHIR 的 AZURE API 和用于 FHIR 的 Azure IoT 连接器中的启用诊断日志记录中](enable-diagnostic-logging.md#enable-diagnostic-logging-in-azure-api-for-fhir) 的指南设置审核日志记录。

## <a name="enable-metric-logging-for-the-azure-iot-connector-for-fhir-preview"></a>为适用于 FHIR 的 Azure IoT 连接器 (预览启用指标日志记录) 
1. 若要为 FHIR 的 Azure IoT 连接器启用指标日志记录，请在 Azure 门户中选择用于 FHIR 服务的 Azure API 

2. 导航到 "**诊断设置**" 

3. 选择“+ 添加诊断设置”

   :::image type="content" source="media/iot-metrics-export/diagnostic-settings-main.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-export/diagnostic-settings-main.png"::: 

4. 在 " **诊断设置名称** " 对话框中输入名称。

5. 选择要用于访问诊断日志的方法：

    1. **存档到存储帐户** 进行审核或手动检查。 需要使用的存储帐户需要已创建。
    2. **流式传输到事件中心** ，以供第三方服务或自定义分析解决方案引入。 你将需要创建事件中心命名空间和事件中心策略，然后才能配置此步骤。
    3. **流到 Azure Monitor 中的 Log Analytics** 工作区。 你将需要创建日志分析工作区，然后才能选择此选项。

6. 为 FHIR 的 Azure IoT 连接器选择 **错误、流量和延迟** ，并为用于 FHIR 的 azure API 选择要捕获的任何其他度量值类别。

7. 选择“保存”

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="IoT Connector1" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

> [!Note] 
> 最多可能需要15分钟才能在所选存储库中显示第一个指标日志。  
 
有关如何使用诊断日志的详细信息，请参阅 [Azure 资源日志文档](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview)

## <a name="conclusion"></a>结论 
对指标日志的访问权限对于监视和故障排除至关重要。  使用适用于 FHIR 的 Azure IoT 连接器，可以通过指标日志执行这些操作。 

## <a name="next-steps"></a>后续步骤

查看有关适用于 FHIR 的 Azure IoT 连接器的常见问题。

>[!div class="nextstepaction"]
>[适用于 FHIR 常见问题的 Azure IoT 连接器](fhir-faq.md#azure-iot-connector-for-fhir-preview)

*在 Azure 门户中，适用于 FHIR 的 Azure IoT 连接器称为 IoT Connector（预览版）。

FHIR 是 HL7 的注册商标，经 HL7 许可使用。
