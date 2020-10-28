---
title: 教程 - 部署和配置 VMware HCX
description: 了解如何为 Azure VMware 解决方案私有云部署和配置 VMware HCX 解决方案。
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: c78eae11497702054bb54b5980228fd0a3962577
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367765"
---
# <a name="deploy-and-configure-vmware-hcx"></a>部署和配置 VMware HCX

本文将演练为 Azure VMware 解决方案私有云部署和配置 VMware HCX 本地连接器的过程。 使用 VMware HCX，可以通过各种迁移类型将 VMware 工作负载迁移到 Azure VMware 解决方案和其他已连接站点。 由于 Azure VMware 解决方案部署并配置 HCX 云管理器，因此必须在本地 VMware 数据中心下载、激活和配置 HCX 连接器。

Azure VMware 解决方案中已预先部署 VMware HCX 高级连接器。 它最多支持三个站点连接（本地到云或云到云）。 如果需要三个以上的站点连接，请提交[支持请求](https://rc.portal.azure.com/#create/Microsoft.Support)以启用 [VMware HCX Enterprise](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/) 外接程序。 此加载项目前以预览版提供。 

>[!NOTE]
>VMware HCX Enterprise Edition (EE) 随 Azure VMware 解决方案以预览版服务的形式提供。 它是免费的，并受预览版服务的条款和条件的约束。 在 VMware HCX EE 服务正式发布后，你会收到一个 30 天的通知，指出计费将会进行切换。 你还可以选择关闭或退出服务。

首先，请仔细查看本文的[准备工作](#before-you-begin)、[软件版本要求](#software-version-requirements)和[先决条件](#prerequisites)部分。 

然后，我们会演练所有必需过程，以便：

> [!div class="checklist"]
> * 部署本地 VMware HCX OVA（HCX 连接器）。
> * 激活 VMware HCX 连接器。
> * 将本地 HCX 连接器与 Azure VMware 解决方案 HCX 云管理器配对。
> * 配置互连（网络配置文件、计算配置文件和服务网格）。
> * 通过检查设备状态并验证是否可以迁移来完成设置。

完成后，可以按照本文末尾建议的后续步骤进行操作。  

## <a name="before-you-begin"></a>准备工作
   
* 查看基本的 Azure VMware 解决方案软件定义数据中心 (SDDC) [教程系列](tutorial-network-checklist.md)。
* 查看并参阅 [VMware HCX 文档](https://docs.vmware.com/en/VMware-HCX/index.html)，包括 HCX 用户指南。
* 查看 VMware 文档中的[使用 VMware HCX 迁移虚拟机](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)。
* （可选）查看 [VMware HCX 部署注意事项](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html)。
* （可选）查看 HCX 的相关 VMware 材料，如 VMware vSphere [博客系列](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html)。 
* （可选）通过 Azure VMware 解决方案支持渠道请求 Azure VMware 解决方案 HCX 企业版激活。
* （可选）[查看 HCX 所需的网络端口](https://ports.vmware.com/home/VMware-HCX)。
* 虽然为 Azure VMware 解决方案私有云提供的 /22 网络预配置了 Azure VMware 解决方案 HCX 云管理器，但 HCX 本地连接器要求从本地网络分配网络范围。 这些网络和范围均在本文后面部分进行介绍。

根据计算和存储资源调整工作负荷的大小是一个重要的规划步骤。 此调整大小步骤问题应在私有云环境初始规划过程中解决。 

可以通过在 Azure Migrate 门户中完成 [Azure VMware 解决方案评估](../migrate/how-to-create-azure-vmware-solution-assessment.md)来调整工作负载的大小。

## <a name="software-version-requirements"></a>软件版本要求

基础结构组件必须运行所需的最低版本。 
                                                         
| 组件类型    | 源环境要求    | 目标环境要求   |
| --- | --- | --- |
| vCenter Server   | 5.1<br/><br/>如果使用 5.5 U1 或更低版本，请使用独立 HCX 用户界面进行 HCX 操作。  | 6.0 U2 及更高版本   |
| ESXi   | 5.0    | ESXi 6.0 及更高版本   |
| NSX    | 对于源上的逻辑交换机的 HCX 网络扩展：NSXv 6.2+ 或 NSX-T 2.4+。   | NSXv 6.2+ 或 NSX-T 2.4+<br/><br/>对于 HCX 邻近路由：NSXv 6.4+。 （NSX-T 不支持邻近路由。） |
| vCloud Director   | 不需要。 源站点上不与 vCloud Director 进行互操作。 | 将目标环境与 vCloud Director 集成时，最低版本为 9.1.0.2。  |

## <a name="prerequisites"></a>先决条件

### <a name="network-and-ports"></a>网络和端口

* 在本地与 Azure VMware 解决方案 SDDC ExpressRoute 线路之间配置 [Azure ExpressRoute Global Reach](tutorial-expressroute-global-reach-private-cloud.md)。

* 若要在本地组件与 Azure VMware 解决方案 SDDC 之间进行通信，应打开[所有必需的端口](https://ports.vmware.com/home/VMware-HCX)。

有关详细信息，请参阅 [VMware HCX 文档](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)。


### <a name="ip-addresses"></a>IP 地址

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>在本地部署 VMware HCX 连接器 OVA

> [!NOTE]
> 将虚拟设备部署到本地 vCenter 之前，必须下载 VMware HCX 连接器 OVA。 

1. 打开浏览器窗口，使用 cloudadmin 用户凭据通过 `https://x.x.x.9` 端口 443 登录到 Azure VMware 解决方案 HCX 管理器，然后转到“支持” 。

   > [!TIP]
   > 记下 Azure VMware 解决方案中 HCX 云管理器的 IP 地址。 若要确定该 IP 地址，请在“Azure VMware 解决方案”窗格上转到“管理” > “连接性”，然后选择“HCX”选项卡  。 
   >
   >vCenter 密码是在设置私有云时定义的。

1. 选择“下载”链接以下载 VMware HCX 连接器 OVA 文件。

1. 转到本地 vCenter。 选择 OVF 模板（即你下载的 OVA 文件），以将 HCX 连接器部署到本地 vCenter。  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. 选择名称和位置，然后选择需要在其中部署 HCX 连接器的资源/群集。 然后查看详细信息和所需资源。  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. 查看许可条款。 如果同意，则选择所需的存储和网络，然后选择“下一步”。

1. 在“自定义模板”中，输入所有必需的信息。 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. 选择“下一步”，验证配置，然后选择“完成”以部署 HCX 连接器 OVA 。
     
   > [!NOTE]
   > 通常，你现在部署的 VMware HCX 连接器将部署到群集的管理网络上。  
   
   > [!IMPORTANT]
   > 可能需要手动打开虚拟设备。  如果是这种情况，请等待 10-15 分钟，然后继续下一步。

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：HCX 设备部署](https://www.youtube.com/embed/BwSnQeefnso)视频。 


## <a name="activate-vmware-hcx"></a>激活 VMware HCX

在本地部署 VMware HCX 连接器 OVA 并启动设备后，即可激活。 首先，需要从 Azure VMware 解决方案门户获取许可密钥。

1. 在 Azure VMware 解决方案门户中，转到“管理” > “连接性”，选择“HCX”选项卡，然后选择“添加”   。

1. 使用 admin 凭据登录到 `https://HCXManagerIP:9443` 处的本地 VMware HCX 管理器。 

   > [!IMPORTANT]
   > 确保在 VMware HCX 管理器 IP 地址中包括 `9443` 端口号。

1. 在“许可”中，对于“HCX 高级密钥”输入自己的密钥 。  
   
    > [!NOTE]
    > VMware HCX 管理器必须开放 Internet 访问权限或配置一个代理。

1. 在“数据中心位置”中，提供在本地安装 VMware HCX 管理器的最近位置。

1. 在“系统名称”中，修改名称或接受默认名称。
   
1. 选择“是，继续”。
    
1. 在“连接 vCenter”中，提供你的 vCenter 服务器的 FQDN 或 IP 地址和相应的凭据，然后选择“继续”。
   
1. 在“配置 SSO/PSC”中，提供你的 Platform Services Controller 的 FQDN 或 IP 地址，然后选择“继续” 。
   
   >[!NOTE]
   >通常，此条目与 vCenter FQDN 或 IP 地址相同。

1. 验证所有输入是否正确，并选择“重启”。
    
   > [!NOTE]
   > 重启后，会有一段延迟，然后系统才会提示你执行下一步。

在服务重启后，你会在出现的屏幕上看到 vCenter 显示为绿色，这一点非常重要。 VCenter 和 SSO 必须都具有相应的配置参数，它们应该与上一个屏幕相同。

:::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：激活 HCX](https://www.youtube.com/embed/BkAV_TNYxdE) 视频。


## <a name="configure-the-vmware-hcx-connector"></a>配置 VMware HCX 连接器

现在，你已准备好添加站点配对、创建网络和计算配置文件，以及启用服务，例如迁移、网络扩展或灾难恢复。 

### <a name="add-a-site-pairing"></a>添加站点配对

你可以将 Azure VMware 解决方案中的 VMware HCX 云管理器与数据中心内的 VMware HCX 连接器进行连接（配对）。 

1. 登录到你的本地 vCenter，然后在“主页”下选择“HCX”。

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. 在“基础结构”下，选择“站点配对”，然后选择“连接到远程站点”选项（在屏幕中间）  。 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. 输入之前记下的远程 HCX URL 或 IP 地址、Azure VMware 解决方案的 cloudadmin@vsphere.local 用户名以及密码。 然后选择“连接”  。

   > [!NOTE]
   > 若要成功建立站点对，HCX 连接器必须能够通过端口 443 路由到 HCX 云管理器 IP。
   >
   > 密码与用于登录到 vCenter 的密码相同。 你已在初始部署屏幕上定义了此密码。

   你会看到一个屏幕，其中显示了 Azure VMware 解决方案中的 HCX 云管理器和本地 HCX 连接器已连接（配对）。

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="浏览到 OVF 模板的屏幕截图。":::

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：HCX 站点配对](https://www.youtube.com/embed/sKizDCRHOko)视频。



### <a name="create-network-profiles"></a>创建网络配置文件

VMware HCX 会（自动）部署虚拟设备的子集，这需要多个 IP 段。 创建网络配置文件时，需要定义在 [VMware HCX 网络段预先部署准备和规划阶段](production-ready-deployment-steps.md#vmware-hcx-network-segments)确定的 IP 段。

将创建四个网络配置文件：

   - 管理
   - vMotion
   - 复制
   - 上行

1. 在“基础结构”下，选择“互连” > “多站点服务网格” > “网络配置文件” > “创建网络配置文件”。    

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. 对于每个网络配置文件，请选择网络、端口组，提供名称，并为该段创建 IP 池。 然后选择“创建”。 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。":::

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：HCX 网络配置文件](https://www.youtube.com/embed/NhyEcLco4JY)视频。


### <a name="create-a-compute-profile"></a>创建计算配置文件

1. 选择“计算配置文件” > “创建计算配置文件”。

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. 输入配置文件的名称，然后选择“继续”。  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. 选择要启用的服务，例如迁移、网络扩展或灾难恢复，然后选择“继续”。
  
   > [!NOTE]
   > 通常，不会有任何变化。

1. 在“选择服务资源”中，选择用于启用选定的 VMware HCX 服务的一个或多个服务资源（群集）。  

1. 看到本地数据中心的群集后，选择“继续”。

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. 从“选择数据存储”中，选择用于部署 VMware HCX 互连设备的数据存储资源。 然后选择“继续”。

   选择了多个资源时，VMware HCX 会使用所选的第一个资源，直到其容量用尽。   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. 从“选择管理网络配置文件”中，选择在前面的步骤中创建的管理网络配置文件。 然后选择“继续”。  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > 管理网络配置文件允许 VMware HCX 设备与 vCenter 通信。 可通过此配置文件访问 ESXi 主机。

1. 从“选择上行网络配置文件”中，选择在前面的步骤中创建的上行网络配置文件。 然后选择“继续”。

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. 从“选择 vMotion 网络配置文件”中，选择在前面的步骤中创建的 vMotion 网络配置文件。 然后选择“继续”。

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. 从“选择 vSphere 复制网络配置文件”中，选择在前面的步骤中创建的复制网络配置文件。 然后选择“继续”。

   在大多数情况下，复制网络配置文件与管理网络配置文件相同。  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. 从“选择用于网络扩展的分布式交换机”中，选择要迁移到第 2 层扩展网络上的 Azure VMware 解决方案的虚拟机所在的分布式虚拟网络。 然后选择“继续”。

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. 查看连接规则，然后选择“继续”。  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. 选择“完成”以创建完整配置文件。


   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：计算配置文件](https://www.youtube.com/embed/qASXi5xrFzM)视频。

### <a name="create-a-service-mesh"></a>创建服务网格

现在，可以在本地与 Azure VMware 解决方案 SDDC 之间配置服务网格了。

   > [!NOTE]
   > 若要使用 Azure VMware 解决方案成功建立服务网格，请执行以下操作：
   >
   > 在本地 HCX 连接器定义的“上行”网络配置文件地址与 Azure VMware 解决方案 HCX 云“上行”网络配置文件地址之间打开端口 UDP 500/4500。
   >
   > 请务必查看 [HCX 所需的端口](https://ports.vmware.com/home/VMware-HCX)。

1. 在“基础结构”下，选择“互连” > “服务网格” > “创建服务网格”。      

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. 查看预填充的站点，然后选择“继续”。 

   >[!NOTE]
   >如果这是你的第一个服务网格配置，则无需修改此屏幕。  

1. 从下拉列表选择源计算配置文件和远程计算配置文件，然后选择“继续”。  

   这些选择定义 VM 可以在其中消耗 VMware HCX 服务的资源。  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. 查看将启用的服务，然后选择“继续”。  

1. 在“高级配置 - 替代上行网络配置文件”中，选择“继续” 。  

   上行网络配置文件连接到网络，可以通过该网络访问远程站点的互连设备。  
  
1. 在“高级配置 - 网络扩展设备横向扩展”中进行检查，然后选择“继续”。 

1. 在“高级配置 - 流量工程”中进行检查并做出必要的修改，然后选择“继续” 。

1. 查看拓扑预览，然后选择“继续”。

1. 为此服务网格输入一个用户易记名称，然后选择“完成”以完成此操作。  

1. 选择“查看任务”以监视部署。 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="浏览到 OVF 模板的屏幕截图。":::

   当服务网格部署成功完成时，你会看到服务为绿色。

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. 通过检查设备状态验证服务网格的运行状况。 
1. 选择“互联” > “设备” 。

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

有关此过程的端到端概述，请观看 [Azure VMware 解决方案：服务网格](https://www.youtube.com/embed/FyZ0d3P_T24)视频。



### <a name="optional-create-a-network-extension"></a>（可选）创建网络扩展

如果希望将任何网络从本地环境扩展到 Azure VMware 解决方案，请执行以下步骤：

1. 在“服务”下，选择“网络扩展”，然后选择“创建网络扩展”。

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. 选择要将其扩展到 Azure VMware 解决方案的每个网络，然后选择“下一步”。

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="浏览到 OVF 模板的屏幕截图。":::

1. 输入要扩展的每个网络的本地网关 IP，然后选择“提交”。 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="浏览到 OVF 模板的屏幕截图。":::

   只需几分钟时间即可完成网络扩展。 然后，你会看到状态更改为“扩展完成”。

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="浏览到 OVF 模板的屏幕截图。" lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

有关此步骤的端到端概述，请观看 [Azure VMware 解决方案：网络扩展](https://www.youtube.com/embed/cNlp0f_tTr0)视频。


## <a name="next-steps"></a>后续步骤

如果你已完成到这里，并且设备互连隧道状态为“正常”且其颜色为绿色，则可以使用 VMware HCX 来迁移和保护 Azure VMware 解决方案 VM。 Azure VMware 解决方案支持工作负载迁移（带有或不带网络扩展）。 你仍然可以在 vSphere 环境中迁移工作负载，以及在本地创建网络并将 VM 部署到这些网络上。  

有关如何使用 HCX 的详细信息，请参阅 VMware 技术文档：

* [VMware HCX 文档](https://docs.vmware.com/en/VMware-HCX/index.html)
* [使用 VMware HCX 迁移虚拟机](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)。
* [HCX 所需的端口](https://ports.vmware.com/home/VMware-HCX)
