---
title: 创建连接监视器-Azure 门户
titleSuffix: Azure Network Watcher
description: 本文介绍如何使用 Azure 门户在连接监视器中创建监视器。
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: vinigam
ms.openlocfilehash: 8c4df64334957a78fcf05c11625335214f8e032a
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94699366"
---
# <a name="create-a-monitor-in-connection-monitor-by-using-the-azure-portal"></a>使用 Azure 门户在连接监视器中创建监视器

了解如何使用连接监视器来监视资源之间的通信。 本文介绍如何使用 Azure 门户创建监视器。 连接监视器支持混合和 Azure 云部署。

## <a name="before-you-begin"></a>在开始之前 

在使用连接监视器创建的连接监视器中，可以将本地计算机和 Azure Vm 添加为源。 这些连接监视器还可以监视与终结点的连接。 终结点可以位于 Azure 上，也可以位于任何其他 URL 或 IP 上。

下面是一些可帮助你入门的定义：

* **连接监视器资源**。 特定于区域的 Azure 资源。 以下所有实体都是连接监视器资源的属性。
* **终结点**。 参与连接检查的源或目标。 终结点的示例包括：
    * Azure Vm。
    * Azure 虚拟网络。
    * Azure 子网。
    * 本地代理。
    * 本地子网。
    * 包含多个子网的本地自定义网络。
    * Url 和 Ip。
* **测试配置**。 测试的协议特定配置。 根据所选择的协议，可以定义端口、阈值、测试频率和其他参数。
* **测试组**。 包含源终结点、目标终结点和测试配置的组。 连接监视器可包含多个测试组。
* **Test**。 源终结点、目标终结点和测试配置的组合。 测试是可用于监视数据的最精细级别。 监视数据包括检查失败的百分比和往返时间 (RTT)。

:::image type="content" source="./media/connection-monitor-2-preview/cm-tg-2.png" alt-text="显示连接监视器并定义测试组和测试之间的关系的关系图。":::


## <a name="create-a-connection-monitor"></a>创建连接监视器

使用 Azure 门户在连接监视器中创建监视器：

1. 在 Azure 门户主页上，转到“网络观察程序”。
1. 在左窗格中的 " **监视** " 部分，选择 " **连接监视器**"。

   你将看到在连接监视器中创建的所有连接监视器。 若要查看在经典连接监视器中创建的连接监视器，请访问 **连接监视器** 选项卡。

   :::image type="content" source="./media/connection-monitor-2-preview/cm-resource-view.png" alt-text="显示连接监视器中创建的连接监视器的屏幕截图。":::
   
    
1. 在 " **连接监视器** " 仪表板的左上角，选择 " **创建**"。

   

1. 在“基本信息”选项卡上，为连接监视器输入信息： 
   * **连接监视器名称**：输入连接监视器的名称。 使用针对 Azure 资源的标准命名规则。
   * **订阅**：为连接监视器选择订阅。
   * **区域**：选择连接监视器的区域。 只能选择在此区域创建的源 VM。
   * **工作区配置**：选择自定义工作区或默认工作区。 工作区保存监视数据。
       * 若要使用默认工作区，请选择此复选框。 
       * 若要选择自定义工作区，请清除此复选框。 然后选择自定义工作区的订阅和区域。 

   :::image type="content" source="./media/connection-monitor-2-preview/create-cm-basics.png" alt-text="显示连接监视器中的 &quot;基本信息&quot; 选项卡的屏幕截图。":::
   
1. 在选项卡底部，选择“下一步：测试组”。

1. 在测试组中添加源、目标和测试配置。 若要了解如何设置测试组，请参阅 [在连接监视器中创建测试组](#create-test-groups-in-a-connection-monitor)。 

   :::image type="content" source="./media/connection-monitor-2-preview/create-tg.png" alt-text="显示连接监视器中的 &quot;测试组&quot; 选项卡的屏幕截图。":::

1. 在选项卡底部，选择“下一步：创建警报”。 若要了解如何创建警报，请参阅 [在连接监视器中创建警报](#create-alerts-in-connection-monitor)。

   :::image type="content" source="./media/connection-monitor-2-preview/create-alert.png" alt-text="显示 &quot;创建警报&quot; 选项卡的屏幕截图。":::

1. 在选项卡底部，选择“下一步：查看 + 创建”。

1. 先在“查看 + 创建”选项卡上查看基本信息和测试组，再创建连接监视器。 如果需要编辑连接监视器，可以返回到相应的选项卡来执行此操作。 
   :::image type="content" source="./media/connection-monitor-2-preview/review-create-cm.png" alt-text="显示连接监视器中的 &quot;查看&quot; 和 &quot;创建&quot; 选项卡的屏幕截图。":::
   > [!NOTE] 
   > " **查看** " 和 "创建" 选项卡显示连接监视器阶段每月的费用。 当前，" **当前成本/月份** " 列无需支付费用。 连接监视器公开上市之后，此列才会显示每月费用。 
   > 
   > 即使在连接监视器阶段，也会收取 Log Analytics 引入费用。

1. 准备好创建连接监视器后，在“查看 + 创建”选项卡的底部，选择“创建” 。

连接监视器在后台创建连接监视器资源。

## <a name="create-test-groups-in-a-connection-monitor"></a>在连接监视器中创建测试组

每个连接监视器中的测试组都包括按网络参数进行测试的源和目标。 它们测试的是失败检查的百分比和基于测试配置的 RTT。

在 Azure 门户中，若要在连接监视器中创建测试组，请为以下字段指定值：

* **禁用测试组**：可以选择此复选框，以对测试组指定的所有源和目标禁用监视。 默认情况下，此选择处于未选中状态。
* **名称**：命名测试组。
* **源**：如果在 Azure vm 和本地计算机上安装了代理，则可以将其指定为源。 若要了解如何为源安装代理，请参阅 [安装监视代理](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#install-monitoring-agents)。
   * 若要选择 Azure 代理，请选择“Azure 终结点”选项卡。此处只能看到绑定到创建连接监视器时指定的区域的 VM。 默认情况下，会将 VM 分组到它们所属的订阅中。 已折叠这些组。 
   
       您可以向下钻取到层次结构 **中的其他** 级别：

      **订阅**  > **资源组**  > **VNET**  > **子网**  > **具有代理的 vm**

      还可以更改 " **分组依据** " 选择器以从任何其他级别启动树。 例如，如果按虚拟网络分组，则会看到在层次结构 **VNET**  >  **子网**  >  **vm** 中具有代理的 vm。

       选择 VNET、子网或单个 VM 时，会将相应的资源 ID 设置为终结点。 默认情况下，所选 VNET 或子网中的所有 Vm 都具有 Azure 网络观察程序扩展。 若要减小范围，请选择 "特定子网或代理" 或更改 "作用域" 属性的值。 

      :::image type="content" source="./media/connection-monitor-2-preview/add-azure-sources.png" alt-text="显示 &quot;添加源&quot; 窗格和连接监视器中 &quot;Azure 终结点&quot; 选项卡的屏幕截图。":::

   * 若要选择本地代理，请选择 " **非 Azure 终结点** " 选项卡。默认情况下，代理按区域分组到工作区中。 所有这些工作区都配置了网络性能监视器。 
   
       如果需要将网络性能监视器添加到工作区，请通过 [Azure 市场](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)获取。 有关如何添加网络性能监视器的信息，请参阅 [Azure Monitor 中的监视解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions)。 
   
       在 " **创建连接监视器**" 下，在 " **基本** 信息" 选项卡上选择默认区域。 如果更改了区域，则可以在新区域的工作区中选择代理。 可选择一个或多个代理或子网。 在 " **子网** " 视图中，可以选择要监视的特定 ip。 如果添加多个子网，则将创建一个名为 **OnPremises_Network_1** 的自定义本地网络。 还可以更改 " **分组依据** " 选择器以按代理分组。

      :::image type="content" source="./media/connection-monitor-2-preview/add-non-azure-sources.png" alt-text="显示连接监视器中 &quot;添加源&quot; 窗格和 &quot;非 Azure 终结点&quot; 选项卡的屏幕截图。":::

   * 若要选择最近使用的终结点，可以使用 "最近使用的 **终结点** " 选项卡 
   
   * 完成源设置后，请在选项卡底部选择 " **完成** "。你仍可以通过在 " **创建测试组** " 视图中选择终结点来编辑基本属性（如终结点名称）。 

* **目标**：你可以通过将 Azure VM、本地计算机或任何终结点指定为目标来监视其连接性)  (公共 IP、URL 或 FQDN。 在单个测试组中，可以添加 Azure Vm、本地计算机、Office 365 Url、Dynamics 365 Url 和自定义终结点。

    * 若要选择 Azure Vm 作为目标，请选择 " **azure 终结点**" 选项卡。默认情况下，Azure Vm 将分组到你在 "**基本** 信息" 选项卡上的 "**创建连接监视器**" 下选择的区域中的订阅层次结构。可以更改区域，并从新区域选择 Azure Vm。 然后，你可以从 **订阅** 级别向下钻取到层次结构中的其他级别，就像设置源 Azure 终结点一样。

      你可以选择 Vnet、子网或单个 Vm，因为你可以在设置源 Azure 终结点时进行选择。 选择 VNET、子网或单个 VM 时，会将相应的资源 ID 设置为终结点。 默认情况下，所选 VNET 或子网中具有网络观察程序扩展的所有 Vm 都参与监视。 若要减小范围，请选择 "特定子网或代理" 或更改 "作用域" 属性的值。 

      :::image type="content" source="./media/connection-monitor-2-preview/add-azure-dests1.png" alt-text="<屏幕截图，显示 &quot;添加目标&quot; 窗格和 &quot;Azure 终结点&quot; 选项卡。 >":::

      :::image type="content" source="./media/connection-monitor-2-preview/add-azure-dests2.png" alt-text="<屏幕快照，其中显示了订阅级别的 &quot;添加目标&quot; 窗格。 >":::
       
    
    * 若要将非 Azure 代理选为目标，请选择 " **非 azure 终结点** " 选项卡。默认情况下，代理按区域分组到工作区中。 所有这些工作区都配置了网络性能监视器。 
    
      如果需要将网络性能监视器添加到工作区，请通过 Azure 市场获取。 有关如何添加网络性能监视器的信息，请参阅 [Azure Monitor 中的监视解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions)。 

      在 " **创建连接监视器**" 下，在 " **基本** 信息   " 选项卡上选择默认区域。 如果更改了区域，则可以在新区域的工作区中选择代理。 可选择一个或多个代理或子网。 在 " **子网** " 视图中，可以选择要监视的特定 ip。 如果添加多个子网，则将创建一个名为 **OnPremises_Network_1** 的自定义本地网络。  

      :::image type="content" source="./media/connection-monitor-2-preview/add-non-azure-dest.png" alt-text="显示 &quot;添加目标&quot; 窗格和 &quot;非 Azure 终结点&quot; 选项卡的屏幕截图。":::
    
    * 若要选择公共终结点作为目标，请选择“外部地址”选项卡。终结点列表包含 Office 365 测试 URL 和 Dynamics 365 测试 URL（按名称分组）。 还可以选择在同一连接监视器中的其他测试组中创建的终结点。 
    
        若要添加终结点，请在右上角选择 " **添加终结点**"。 然后提供终结点名称和 URL、IP 或 FQDN。

       :::image type="content" source="./media/connection-monitor-2-preview/add-endpoints.png" alt-text="显示将公共终结点添加为连接监视器中的目标的位置的屏幕截图。":::

    * 若要选择最近使用的终结点，请访问 " **最近的终结点**"   选项卡。
    * 选择完目标后，选择“完成”。 你仍可以通过在 " **创建测试组** " 视图中选择终结点来编辑基本属性（如终结点名称）。 

* **测试配置**：可以将一个或多个测试配置添加到测试组。 使用新的 " **配置** " 选项卡创建新的测试配置。或从 " **选择现有** " 选项卡上的同一连接监视器中的其他测试组添加测试配置。

    * **测试配置名称**：命名测试配置。
    * **协议**：选择 " **TCP**"、" **ICMP**" 或 " **HTTP**"。 若要将 HTTP 更改为 HTTPS，请选择 **http** 作为协议，然后选择 **443** 作为端口。
        * "**创建 TCP 测试配置**"：仅当在 "**协议**" 列表中选择 " **HTTP** " 时才会显示此复选框。 选中此复选框可创建另一个测试配置，该配置使用您在配置中其他地方指定的相同源和目标。 新测试配置名为 **\<name of test configuration> _networkTestConfig**。
        * **禁用 traceroute**：当协议为 TCP 或 ICMP 时，此复选框适用。 选择此框以阻止源发现拓扑和逐跳 RTT。
    * **目标端口**：可以提供所选的目标端口。
        * **侦听端口**：当协议为 TCP 时，此复选框适用。 选中此复选框，打开所选的 TCP 端口（如果尚未打开）。 
    * **测试频率**：在此列表中，指定源将对指定的协议和端口 ping 目标的频率。 可以选择 30 秒、1 分钟、5 分钟、15 分钟或 30 分钟。 选择 " **自定义** " 输入30秒到30分钟之间的另一个频率。 源将根据所选的值来测试与目标的连接。 例如，如果选择30秒，则源将在每隔30秒的时间段内至少检查到目标的连接。
    * **成功阈值**：可以在以下网络参数上设置阈值：
       * **检查失败**：设置在源使用指定的条件检查与目标的连接时可能失败的检查的百分比。 对于 TCP 或 ICMP 协议，失败检查的百分比可能会等同到数据包丢失的百分比。 对于 HTTP 协议，此值表示未接收到响应的 HTTP 请求的百分比。
       * **往返时间**：设置通过测试配置连接到目标所需的持续时间（以毫秒为单位）。
       
   :::image type="content" source="./media/connection-monitor-2-preview/add-test-config.png" alt-text="显示在连接监视器中设置测试配置的位置的屏幕截图。":::
       
## <a name="create-alerts-in-connection-monitor"></a>在连接监视器中创建警报

您可以根据测试配置中设置的阈值，设置失败的测试的警报。

在 Azure 门户中，若要为连接监视器创建警报，请为这些字段指定值： 

- **创建警报**：可以选中此复选框以在 Azure Monitor 中创建指标警报。 选中此复选框时，将启用其他字段进行编辑。 根据 [警报的定价](https://azure.microsoft.com/pricing/details/monitor/)，还可以查看警报的额外费用。 

- **作用域**  > **资源**  > **层次结构**：根据 "**基本** 信息" 选项卡上指定的值自动填充这些值。

- **条件名称**：根据 `Test Result(preview)` 度量值创建警报。 如果连接监视器测试的结果为失败结果，将触发警报规则。 

- **操作组名称**：可以直接输入电子邮件，也可以通过操作组创建警报。 如果直接输入电子邮件，则会创建一个名为 " **NPM Email 操作组** " 的操作组。 该电子邮件 ID 将添加到该操作组中。 如果选择使用 "操作组"，则需要选择以前创建的操作组。 若要了解如何创建操作组，请参阅 [在 Azure 门户中创建操作组](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)。 创建警报后，你可以 [管理警报](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric#view-and-manage-with-azure-portal)。 

- **警报规则名称**：连接监视器的名称。

- **创建时启用规则**：选中此复选框可以根据条件启用警报规则。 如果要在不启用规则的情况下创建规则，请禁用此复选框。 

:::image type="content" source="./media/connection-monitor-2-preview/create-alert-filled.png" alt-text="显示连接监视器中的 &quot;创建警报&quot; 选项卡的屏幕截图。":::

## <a name="scale-limits"></a>规模限制

连接监视器具有以下缩放限制：

* 每个区域每个订阅的最大连接监视器数：100
* 每个连接监视器的最大测试组：20 个
* 每个连接监视器的最大源和目标：100
* 每个连接监视器的最大测试组：2 个通过 Azure 门户

## <a name="next-steps"></a>后续步骤

* 了解 [如何分析监视数据并设置警报](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#analyze-monitoring-data-and-set-alerts)。
* 了解 [如何诊断网络中的问题](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#diagnose-issues-in-your-network)。
