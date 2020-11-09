---
title: 快速入门 - 探索示例方案
titleSuffix: Azure Digital Twins
description: 快速入门 - 使用 ADT Explorer 示例直观显示和探索预生成方案。
author: baanders
ms.author: baanders
ms.date: 9/24/2020
ms.topic: quickstart
ms.service: digital-twins
ms.openlocfilehash: 466129e8435ef694821b078592a100a111a43f3a
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242273"
---
# <a name="quickstart---explore-a-sample-azure-digital-twins-scenario-using-adt-explorer"></a>快速入门 - 使用 ADT Explorer 探索示例 Azure 数字孪生方案

通过 Azure 数字孪生，可创建真实环境的实时模型并与之交互。 要实现这一点，也可将单个元素建模为数字孪生，然后将它们连接到可响应实时事件且可查询来获取信息的知识图 。

在本快速入门中，你将通过名为 [Azure 数字孪生 (ADT) 资源管理器](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/)的示例应用程序来探索预生成的 Azure 数字孪生图。 借助 ADT Explorer 可上传环境的数字表示形式，查看孪生的可视化映像，以及创建用于表示 Azure 数字孪生中环境的图形，并通过基于浏览器的视觉体验执行其他管理活动。

本快速入门包含以下主要步骤：

1. 设置 Azure 数字孪生实例和 ADT 资源管理器
1. 上传预生成的模型和图数据来构造示例方案
1. 探索已创建的方案图
1. 对图进行更改

你将使用的示例图表示具有两个楼层和两个房间的建筑。 该图将如下所示：

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="由箭头连接的 4 个圆形节点组成的图形视图。标记为“楼层 1”的圆圈通过标记为“包含”的箭头连接到标记为“房间 1”的圆圈；标记为“楼层 0”的圆圈通过标记为“包含”的箭头连接到标记为“房间 0”的圆圈。未连接“楼层 1”和“楼层 0”。":::

## <a name="prerequisites"></a>必备知识

要完成本快速入门，你需要一个 Azure 订阅。 如果还没有 Azure 订阅，可立即[免费创建一个](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

还需要在计算机上有 Node.js。 可以通过以下链接获取最新版本：[Node.js](https://nodejs.org/)。

最后，你还需要下载在快速入门期间使用的示例：ADT Explorer 示例应用程序。 此示例包含在快速入门中用于加载和探索 Azure 数字孪生方案的应用，以及示例方案文件。 若要获取此示例，请导航到：[Azure 数字孪生 (ADT) 资源管理器](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/)。 点击“下载 ZIP”按钮，将此示例代码的 .ZIP 文件下载到计算机 。 这会将 .ZIP 文件夹以 Azure_Digital_Twins__ADT__explorer.zip 的形式下载到计算机上。 解压缩文件夹并提取文件。

## <a name="set-up-azure-digital-twins-and-adt-explorer"></a>设置 Azure 数字孪生和 ADT 资源管理器

要使用 Azure 数字孪生，第一步是设置 Azure 数字孪生实例。 创建服务的实例并设置凭据以向 ADT Explorer 进行身份验证后，可以在 ADT Explorer 中连接到该实例，并且稍后可在快速入门中使用示例数据填充该实例 。

本部分的其余部分将指导你完成这些步骤。

### <a name="set-up-azure-digital-twins-instance"></a>设置 Azure 数字孪生实例

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="set-up-local-azure-credentials"></a>设置本地 Azure 凭据

当你在本地计算机上运行示例时，ADT Explorer 应用程序使用 [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet)（属于 `Azure.Identity` 库的一部分）对用户进行 Azure 数字孪生实例验证。 若要详细了解客户端应用可向 Azure 数字孪生进行验证的不同方法，请参阅[操作说明：编写应用身份验证码](how-to-authenticate-client.md)。

借助此身份验证类型，ADT Explorer 将在本地环境中搜索凭据，如本地 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 或 Visual Studio/Visual Studio Code 中的 Azure 登录。 这意味着，你应该通过这些机制之一在本地登录 Azure，以便设置 ADT Explorer 应用的凭据。

如果已通过其中一种方式登录到 Azure，则可以跳到[下一部分](#run-and-configure-adt-explorer)。

否则，可以使用以下步骤安装本地 Azure CLI：
1. 按照[此安装链接](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)上的过程进行操作，完成与你的操作系统相匹配的安装。
2. 在计算机上打开控制台窗口。
3. 运行 `az login` 并按照身份验证提示操作以登录到 Azure 帐户。

完成此操作后，在下一部分中运行 ADT Explorer 时，它会自动获取 Azure 凭据。

如果需要，你可以关闭身份验证控制台窗口，或者将其保持打开状态，以便在下一步中使用。

### <a name="run-and-configure-adt-explorer"></a>运行并配置 ADT 资源管理器

接下来，运行 ADT 资源管理器应用程序，并为 Azure 数字孪生实例配置该应用程序。

导航到已下载并解压缩的 Azure_Digital_Twins__ADT__explorer 文件夹。 在文件夹位置 Azure_Digital_Twins__ADT__explorer/client/src 打开控制台窗口。

运行 `npm install`，下载所有必需的依赖项。

然后，运行 `npm run start` 来启动该应用。

几秒钟后，将打开一个浏览器窗口，应用将显示在浏览器中。

:::image type="content" source="media/quickstart-adt-explorer/explorer-blank.png" alt-text="显示在 localhost:3000 运行的应用的浏览器窗口。此应用名为 ADT 资源管理器，其中包含查询资源管理器、模型视图、图形视图和属性资源管理器所对应的框。屏幕上尚无数据。" lightbox="media/quickstart-adt-explorer/explorer-blank.png":::

点击窗口顶部的“登录”按钮（如下图所示），以将 ADT Explorer 配置为使用已设置的实例。 

:::image type="content" source="media/quickstart-adt-explorer/sign-in.png" alt-text="在窗口顶部附近突出显示“登录”图标的 ADT 资源管理器。图标显示了一个简单的人员与密钥叠加的剪影。" lightbox="media/quickstart-adt-explorer/sign-in.png":::

输入先前在[先决条件](#prerequisites)部分中收集的 Azure 数字孪生实例 URL，格式为 https://{instance host name} 。

>[!NOTE]
> 可随时重新访问/编辑此信息，方法是选择相同图标以再次拉取“登录”框。 它将保留你传入的值。

> [!TIP]
> 如果在连接时显示 `SignalRService.subscribe` 错误消息，请确保 Azure 数字孪生 URL 以“https://”开头。

如果你看到来自 Microsoft 的“权限已请求”弹出窗口，请向此应用程序授予同意并接受以继续。

## <a name="add-the-sample-data"></a>添加示例数据

接下来，请将示例方案和图导入到 ADT 资源管理器中。 还可在你先前下载的 Azure_Digital_Twins__ADT__explorer 文件夹中找到该示例方案。

### <a name="models"></a>模型

Azure 数字孪生解决方案的第一步是为环境定义词汇。 要实现这一点，还可创建自定义[模型](concepts-models.md)，其中该模型描述了环境中存在的实体类型。 

每个模型均采用类似于 JSON-LD 的被称为“数字孪生定义语言 (DTDL)”的语言进行编写，并根据其属性、遥测、关系和组件来描述单一类型的实体   。 稍后，你将使用这些模型作为表示这些类型的特定实例的数字孪生的基础。

通常，在创建模型时，需要完成 3 个步骤：
1. 编写模型定义（在本快速入门中，此操作已作为示例解决方案的一部分完成）
2. 验证它以确保语法正确（在本快速入门中，此操作已作为示例解决方案的一部分完成）
3. 将它上传到 Azure 数字孪生实例
 
在本快速入门中，已为你编写并验证了模型文件，这些文件包含在已下载的解决方案中。 在本部分中，你要将两个预先编写的模型上传到实例，以定义构建环境的这些组件：
* Floor
* 会议室

#### <a name="upload-models"></a>上传模型

在“模型视图”框中，点击“上传模型”图标 。

:::image type="content" source="media/quickstart-adt-explorer/upload-model.png" alt-text="在“模型视图”框中，中间图标突出显示。它显示了一个指向云的箭头。" lightbox="media/quickstart-adt-explorer/upload-model.png":::
 
1. 在出现的“文件选择器”框中，导航到已下载的存储库中的“Azure_Digital_Twins__ADT__explorer/client/examples”文件夹。
2. 选择“Room.json”和“Floor.json”，然后点击“确定” 。 （可根据需要上传其他模型，但在本快速入门中不会用到它们。）
3. 按照要求登录到 Azure 帐户的弹出对话框的指示操作。

>[!NOTE]
>如果出现下面的错误消息：:::image type="content" source="media/quickstart-adt-explorer/error-models-popup.png" alt-text="弹出窗口显示“错误:提取模型时出错:ClientAuthError:打开弹出窗口时出错。如果你使用的是 IE 或浏览器中阻止了弹出窗口，则可能会发生这种情况。”，底部带有“关闭”按钮" border="false"::: 
> 请尝试禁用弹出窗口阻止程序或使用其他浏览器。

ADT 资源管理器现在会将这些模型文件上传到 Azure 数字孪生实例。 它们应显示在“模型视图”框中，并显示其友好名称和完整模型 ID。 可单击“查看模型”信息气泡，查看其后面的 DTDL 代码。

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/model-info.png" alt-text="“模型视图”框的视图，其中列出了两个模型定义，分别是楼层 (dtmi:example:Floor;1) 和房间 (dtmi:example:Room;1)。“查看模型”图标，其中每个模型都突出显示了一个带圆圈的字母“i”。" lightbox="media/quickstart-adt-explorer/model-info.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="twins-and-the-twin-graph"></a>孪生和孪生图

现在，一些模型已上传到 Azure 数字孪生实例，你可添加按照模型定义的[数字孪生](concepts-twins-graph.md)。 

数字孪生表示业务环境中的实际实体，例如农场中的传感器、汽车里的灯或者本例中建筑楼层上的房间。 你可创建任意给定模型类型的多个孪生（例如都使用房间模型的多个房间），并使用关系将它们连接到表示完整环境的孪生图中。

在本部分，将上传预先创建的孪生，而这些孪生已连接到预先创建的图中。 该图包含两个楼层和两个房间，并通过下列布局连接：
* 楼层 0
    - 包含房间 0
* 楼层 1
    - 包含房间 1

#### <a name="import-the-graph"></a>导入图

在“图形视图”框中，点击“导入图”图标 。

:::image type="content" source="media/quickstart-adt-explorer/import-graph.png" alt-text="在“图形视图”框中，一个图标突出显示。它显示了一个指向云的箭头。" lightbox="media/quickstart-adt-explorer/import-graph.png":::

在“文件选择器”框中，再次导航到 Azure_Digital_Twins__ADT__explorer/client/examples 文件夹，然后选择 buildingScenario.xlsx 电子表格文件。 此文件包含示例图的说明。 点击“确定”。

几秒钟后，ADT 资源管理器将打开“导入”视图，其中显示了要加载的图的预览。

若要确认图上传，请点击“图形视图”右上角的“保存”图标 ：

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/graph-preview-save.png" alt-text="在“图形预览”窗格中突出显示“保存”图标" lightbox="media/quickstart-adt-explorer/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

ADT 资源管理器现在将使用上传的文件来创建所请求的孪生及其相互之间的关系。 将出现一个对话框，表明已完成。 点击“关闭”。

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/import-success.png" alt-text="指示图导入成功的对话框。它显示为“导入成功。已导入 4 对孪生。已导入 2 对关系。”" lightbox="media/quickstart-adt-explorer/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

该图现在已上传到 ADT 资源管理器。 要查看该图，请在 ADT Explorer 窗口顶部附近的“图形资源管理器”框中点击“运行查询”按钮 。 

:::image type="content" source="media/quickstart-adt-explorer/run-query.png" alt-text="窗口顶部附近显示“运行查询”的按钮突出显示" lightbox="media/quickstart-adt-explorer/run-query.png":::

这将运行默认查询以选择并显示所有数字孪生。 ADT 资源管理器将检索服务中的所有孪生和关系，并在“图形视图”框中绘制它们所定义的图。

## <a name="explore-the-graph"></a>浏览图

现在，你可看到已上传的示例方案图：

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="“图形视图”框的视图，其中包含孪生图。标记为“楼层 1”的圆圈通过标记为“包含”的箭头连接到标记为“房间 1”的圆圈；标记为“楼层 0”的圆圈通过标记为“包含”的箭头连接到标记为“房间 0”的圆圈。":::

圆圈（“节点”图）表示数字孪生，线表示关系。 你将看到“楼层 0”孪生包含“房间 0”，“楼层 1”孪生包含“房间 1”   。

如果使用的是鼠标，可单击并拖动图的各个部分来移动它们。

### <a name="view-twin-properties"></a>查看孪生属性 

可选择一个孪生，在“属性资源管理器”框中查看其属性及其值的列表。 

下面是房间 0 的属性：

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room0.png" alt-text="显示房间 0 的属性的“属性资源管理器”框周围进行了突出显示，其中包括“房间 0”的 $dtId 字段、温度字段 70 和湿度字段 30 等等。" lightbox="media/quickstart-adt-explorer/properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

请注意，房间 0 的温度为 70。

下面是房间 1 的属性：

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room1.png" alt-text="显示房间 1 的属性的“属性资源管理器”框周围进行了突出显示，其中包括“房间 1”的 $dtId 字段、温度字段 80 和湿度字段 60 等等。" lightbox="media/quickstart-adt-explorer/properties-room1.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

请注意，房间 1 的温度为 80。

### <a name="query-the-graph"></a>查询图形

Azure 数字孪生的主要功能是能够轻松有效地[查询](concepts-query-language.md)孪生图，以解答有关环境的问题。 

查询图中孪生的一种方法是按其属性进行查询。 基于属性的查询可帮助回答各种问题，包括在环境中查找可能需要注意的离群值。

在本部分中，你将运行一个查询来回答以下问题：“环境中温度高于 75 的孪生都有哪些？”

若要查看答案，请在“查询资源管理器”框中运行以下查询：

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

回想一下之前查看过的孪生属性 - 房间 0 的温度为 70，房间 1 的温度为 80。 因此，此处的结果中仅显示房间 1。
    
:::image type="content" source="media/quickstart-adt-explorer/result-query-property-before.png" alt-text="属性查询的结果，其中仅显示房间 1" lightbox="media/quickstart-adt-explorer/result-query-property-before.png":::

>[!TIP]
> 上面的查询还支持其他比较运算符（<、>、= 或 !=）   。 你可尝试将这些不同的值或不同的孪生属性插入到查询中来试着回答自己的问题。

## <a name="edit-data-in-the-graph"></a>编辑图中的数据

可使用 ADT 资源管理器来编辑图中表示的孪生的属性。 在本部分中，我们将房间 0 的温度调高到 76。

为此，请选择“房间 0”，然后在“属性资源管理器”框中调出其属性列表 。

此列表中的属性是可编辑的。 选择温度值 70 来输入新值。 输入 76，然后点击“保存”图标，将温度更新为 76。

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/new-properties-room0.png" alt-text="显示房间 0 的属性的“属性资源管理器”框。温度值为一个显示 76 的可编辑框，“保存”图标周围突出显示。" lightbox="media/quickstart-adt-explorer/new-properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

成功保存后，你将看到“补丁信息”窗口，其中显示了在后台用于 Azure 数字孪生 [API](how-to-use-apis-sdks.md) 进行更新的补丁代码。 点击“关闭”。

### <a name="query-to-see-the-result"></a>查询以查看结果

若要验证该图是否已成功将更新内容注册为房间 0 的温度，请重新运行先前进行的查询，获取环境中温度高于 75 的所有孪生：

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

现在，房间 0 的温度已从 70 更改为 76，因此两个孪生都应出现在结果中 。

:::image type="content" source="media/quickstart-adt-explorer/result-query-property-after.png" alt-text="属性查询的结果，其中同时显示了房间 0 和房间 1" lightbox="media/quickstart-adt-explorer/result-query-property-after.png":::

## <a name="review-and-contextualize-learnings"></a>回顾所学内容并将其置于上下文中进行理解

在本快速入门中，你创建了一个 Azure 数字孪生实例、将它连接到了 ADT 资源管理器，并使用示例方案对其进行了填充。 

然后，你探索了图，方法是…
1. 使用查询以回答有关方案的问题。
2. 编辑数字孪生上的属性。
3. 再次运行查询，查看答案如何因更新而发生变化。

本练习的目的是演示如何使用 Azure 数字孪生图回答有关环境的问题（即使在环境持续变化时）。 

尽管在本快速入门中你是手动进行温度更新的，但在 Azure 数字孪生中，经常会将数字孪生连接到实际的 IoT 设备，以便它们能够根据遥测数据自动接收更新。 这使你可以生成始终反映环境实际状态的实时图，并使用查询来实时获取环境中所发生情况的相关信息。

## <a name="clean-up-resources"></a>清理资源

若要结束本快速入门的工作，请先结束正在运行的控制台应用。 这将断开浏览器中与 ADT 资源管理器应用的连接，而且你将无法再在浏览器中查看实时数据。 你可关闭浏览器标签页。

如果你计划继续学习 Azure 数字孪生教程，可将本快速入门中使用的实例重复用于那些文章，不用将它们删除。
 
[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

最后，删除已下载到本地计算机的项目示例文件夹 (Azure_Digital_Twins__ADT__explorer)。 可能需要既删除压缩版本，又删除解压缩版本。

## <a name="next-steps"></a>后续步骤 

接下来，请继续学习 Azure 数字孪生教程，生成你自己的 Azure 数字孪生方案和交互工具。

> [!div class="nextstepaction"]
> [*教程：* 为客户端应用编写代码](tutorial-code.md)