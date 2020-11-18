---
title: 使用时序见解来存储和分析 Azure IoT 即插即用设备遥测 | Microsoft Docs
description: 设置时序见解环境并连接 IoT 中心，以便从 IoT 即插即用设备查看和分析遥测。
author: lyrana
ms.author: lyhughes
ms.date: 10/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: aa99b9059fe8e3cd5b0dfe6f7e62bd02012fd144
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422257"
---
# <a name="tutorial-create-and-connect-to-time-series-insights-gen2-to-store-visualize-and-analyze-iot-plug-and-play-device-telemetry"></a>教程：创建时序见解第 2 代并与其连接，以便存储、可视化和分析 IoT 即插即用设备遥测

本教程将介绍如何创建 [Azure 时序见解第 2 代](https://docs.microsoft.com/azure/time-series-insights/overview-what-is-tsi) (TSI) 环境并对其正确配置，以便与 IoT 即插即用解决方案集成。 借助 TSI，你可在物联网 (IoT) 规模上收集、处理、存储、查询和可视化时序数据。

首先，你将预配一个 TSI 环境，并将 IoT 中心作为流式处理事件源进行连接。 然后，你将根据用于温度控制器和恒温器设备的[数字孪生定义语言 (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) 示例模型文件，通过模型同步来创建 TSI 环境的时序模型。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

为了绕开在本地安装 Azure CLI 的要求，可使用 Azure Cloud Shell 设置云服务。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="time-series-id-selection"></a>选择时序 ID

预配 TSI 环境时，需要选择时序 ID。 选择适当的时序 ID 至关重要，因为属性不可变并且在设置后无法更改。 选择时序 ID 与为数据库选择分区键相同。 通常，TS ID 应为资产模型的叶节点。 换句话说，你通常需要选择发出遥测的最细粒度的资产或传感器的 ID 属性。

对于 IoT 即插即用用户，与选择 TS ID 相关的问题是设备模型中是否存在[组件](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#component)。 

![选择 TS ID](./media/tutorial-configure-tsi/ts-id-selection-pnp.png)

* 如果你已完成快速入门，并且 IoT 中心设备代表[恒温器](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/Thermostat.json)，请使用 `iot-hub-connection-device-id` 作为 TS ID。

* 如果你一直在完成多组件 [TemperatureController](https://raw.githubusercontent.com/Azure/opendigitaltwins-dtdl/master/DTDL/v2/samples/TemperatureController.json) 的其中一个教程，请在以下部分中使用编写为 `iot-hub-connection-device-id, dt-subject` 的组合键

## <a name="provision-your-azure-time-series-insights-gen2-environment"></a>预配 Azure 时序见解第 2 代环境

以下命令将执行以下操作：

* 为环境的[冷存储](https://docs.microsoft.com/azure/time-series-insights/concepts-storage#cold-store)创建一个 Azure 存储帐户，专用于历史数据的长期保留和分析。
  * 将 `mytsicoldstore` 替换为帐户的唯一名称。
* 创建 Azure 时序见解第 2 代环境，包括 7 天保留期的热存储和无限保留期的冷存储。 
  * 将 `my-tsi-env` 替换为 TSI 环境的唯一名称 
  * 将 `my-pnp-resourcegroup` 替换为在设置过程中使用的资源组的名称
  * 根据上面的选择条件，将 `my-ts-id-property` 替换为 TS ID 属性值

```azurecli-interactive
storage=mytsicoldstore
rg=my-pnp-resourcegroup
az storage account create -g $rg -n $storage --https-only
key=$(az storage account keys list -g $rg -n $storage --query [0].value --output tsv)
az timeseriesinsights environment longterm create --name my-tsi-env --resource-group $rg --time-series-id-properties my-ts-id-property --sku-name L1 --sku-capacity 1 --data-retention 7 --storage-account-name $storage --storage-management-key $key --location eastus2
```

现在将以前创建的 IoT 中心配置为环境的[事件源](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-ingestion-event-sources)。 当事件源已连接时，TSI 将开始对中心内的事件编制索引（从队列中最早的事件开始）。

首先在 IoT 中心为 TSI 环境创建唯一的使用者组。 将 `my-pnp-hub` 替换为之前使用的 IoT 中心的名称。

```azurecli-interactive
az iot hub consumer-group create --hub-name my-pnp-hub --name tsi-consumer-group 
```

连接 IoT 中心。 将 `my-pnp-resourcegroup`、`my-pnp-hub` 和 `my-tsi-env` 分别替换为你的值。

```azurecli-interactive
rg=my-pnp-resourcegroup
iothub=my-pnp-hub
env=my-tsi-env
es_resource_id=$(az iot hub create -g $rg -n $iothub --query id --output tsv)
shared_access_key=$(az iot hub policy list -g $rg --hub-name $iothub --query "[?keyName=='service'].primaryKey" --output tsv)
az timeseriesinsights event-source iothub create -g $rg --environment-name $env -n iot-hub-event-source --consumer-group-name tsi-consumer-group  --key-name iothubowner --shared-access-key $shared_access_key --event-source-resource-id $es_resource_id
```
导航到 [Azure 门户](https://portal.azure.com)中的资源组，然后选择新创建的时序见解环境。 访问实例概述中所示的“时序见解资源管理器 URL”。

![门户概述页](./media/tutorial-configure-tsi/view-environment.png)

在“资源管理器”中，“所有层次结构”下应会显示两个恒温器。 接下来，你将根据设备模型策展时序模型。

![资源管理器视图 1](./media/tutorial-configure-tsi/tsi-env-first-view.png)

## <a name="model-synchronization-between-digital-twins-definition-language-and-time-series-insights-gen2"></a>数字孪生定义语言与时序见解第 2 代之间的模型同步

接下来将 DTDL 设备模型转换为 Azure 时序见解 (TSI) 中的资产模型。 TSI 的时序模型是一种语义建模工具，用于 TSI 内的数据语境化。 时序模型包括三个核心组件：

* [时序模型实例](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-instances)。 实例是时序本身的虚拟表示形式。 实例将由 TS ID 进行唯一标识。
* [时序模型层次结构](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-hierarchies)。 层次结构通过指定属性名称及其关系来组织实例。
* [时序模型类型](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview#time-series-model-types)。 类型可帮助你定义用于执行计算的[变量](https://docs.microsoft.com/azure/time-series-insights/concepts-variables)或公式。 类型与某个特定实例关联。

> [!NOTE]
> 以下示例适用于多组件 TemperatureController。

### <a name="define-your-types"></a>定义类型

可以开始将数据引入到 Azure 时序见解第 2 代，无需预定义模型。 当遥测到达时，TSI 将尝试基于 TS ID 属性值自动解析时序实例。 所有实例都将被分配“默认类型”。 你将需要手动创建一个表示模型的新类型。 下图描绘了同步 DTDL 模型和 TSM 类型的一种简单方法：

![DTDL 到 TSM 类型](./media/tutorial-configure-tsi/DTDL-to-TSM-Type.png)

* 数字孪生模型标识符 (DTMI) 将成为你的类型 ID
* 类型名称可以是模型的名称，也可以是显示名称
* 模型说明将成为类型的说明
* 至少为每个具有数值架构的遥测组件创建一个类型变量。 
  * 变量只能使用 numeric 数据类型，但如果将值作为可分析的字符串发送（例如 `"0"`），可使用[转换](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#conversion-functions)函数，例如 `toDouble`
* 变量名称可以是遥测名称，也可以是显示名称
* 定义时序表达式 (TSX) 变量时，请参阅线上的遥测名称及其数据类型。

> [!NOTE]
> 此示例只显示两种变量 - 聚合变量和数值变量，但每种类型最多可包含 100 个。 不同的变量可以引用相同的遥测值，以根据需要执行不同的计算。 有关筛选器、聚合和标量函数的完整列表，请参阅[时序见解第 2 代时序表达式语法](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax)文档。

打开你选择的文本编辑器，并将下面的 JSON 保存到本地驱动器：

```JSON
{
  "put": [
    {
      "id": "dtmi:com:example:Thermostat;1",
      "name": "Thermostat",
      "description": "Reports current temperature and provides desired temperature control.",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Temperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event.temperature.Double"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

在时序见解资源管理器中，单击左侧的模型图标以导航到“模型”选项卡。 单击“类型”，然后单击“上传 JSON” ：

![上传](./media/tutorial-configure-tsi/upload-type.png)

选择“选择文件”，选择之前保存的 JSON，然后单击“上传” 

应会看到新定义的恒温器类型。

### <a name="optional---create-a-hierarchy"></a>可选 - 创建层次结构

可选择创建一个层次结构来组织 TemeraptureController 父项下的这两个恒温器组件。

单击“层次结构”并选择“添加层次结构” 。 输入 `Device Fleet` 作为名称并创建一个名为 `Device Name` 的级别，然后单击“保存”。

![添加层次结构](./media/tutorial-configure-tsi/add-hierarchy.png)

### <a name="assign-your-instances-to-the-correct-type"></a>为实例分配正确的类型

接下来将更改实例的类型，并可选择将其置于层次结构中

选择“实例”选项卡，然后单击最右侧的“编辑”图标 。

![编辑实例](./media/tutorial-configure-tsi/edit-instance.png)

单击“类型”下拉列表，然后选择 `Thermostat`。 

![更改实例类型](./media/tutorial-configure-tsi/change-type.png)

如果已创建层次结构，请选择“实例字段”并选中 `Device Fleet` 框。 输入 `Temperature Controller` 作为其父设备的值，然后单击“保存”。

![分配到层次结构](./media/tutorial-configure-tsi/assign-to-hierarchy.png)

对第二个恒温器重复上述步骤。

### <a name="view-your-data"></a>查看数据

导航回图表绘制窗格，并展开 Device Fleet 和 TemperatureController。 单击 thermostat1，选择 `Temperature` 变量，然后单击“添加”，为值绘图。 对 thermostat2 执行相同操作。

![更改 thermostat2 的实例类型](./media/tutorial-configure-tsi/charting-values.png)

## <a name="next-steps"></a>后续步骤

* 若要详细了解各种图表绘制选项，包括间隔大小调整和 Y 轴控件，请参阅 [Azure 时序见解资源管理器](https://docs.microsoft.com/azure/time-series-insights/concepts-ux-panels)文档。

* 若要深入了解环境的时序模型，请参阅[此文](https://docs.microsoft.com/azure/time-series-insights/concepts-model-overview)。

* 若要深入了解查询 API 和时序表达式语法，请[选择此文](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis)。




