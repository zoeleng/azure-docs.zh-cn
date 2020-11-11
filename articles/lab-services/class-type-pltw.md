---
title: 在 Azure 实验室服务实验室中设置项目主管
description: 了解如何设置实验室来向项目主管讲授类的方式。
ms.topic: article
ms.date: 10/28/2020
ms.openlocfilehash: 8585d09759319eef04da5ed68fec603cfa390093
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94496577"
---
# <a name="set-up-labs-for-project-lead-the-way-classes"></a>为项目主管设置实验室，方法为类

[项目主管 (PLTW) ](https://www.pltw.org/) 是一个非赢利组织，它在计算机科学、工程和生物医学科学美国提供 PreK-12 个课程。  在每个 PLTW 类中，学生使用各种软件应用程序作为其动手学习体验的一部分。  许多软件应用程序都需要快速 CPU，或在某些情况下为 GPU。  本文介绍如何为以下 PLTW 类设置实验室，这些类通常在成绩为9-12 的学生中提供：

- **工程设计简介**

    学员会引入工程设计过程，其中包括使用 [Autodesk 的发明者计算机辅助设计 (CAD) ](https://www.autodesk.com/products/inventor/new-features) 软件进行3d 建模。

- **工程原则**
    
    学生了解工程机制、structural\material 强度和自动化。  此类使用诸如 [MD 实体](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/MD+Solids/MD+Solids+Software+Installation+Guide.pdf)、 [西点桥设计器](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/West+Point+Bridge+Builder/Installation+Guide+for+West+Point+Bridge+Designer.pdf)和 [美国军队模拟这样的](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/America's+Army/Installation+Guide+for+Americas+Army+Simulation+17-18.pdf)软件。

- **民事工程和体系结构**

    学生通过 Autodesk 的 Revit 体系结构设计软件，使用 [的](https://www.autodesk.com/products/revit/overview) 体系结构设计软件构建和开发和开发， (model.bim) 建模。

- **计算机集成制造**

    学员探索了涉及机器人和自动化的新式制造流程。   在此类中，学生使用 [Autodesk 的发明者 CAD](https://www.autodesk.com/products/inventor/new-features) 和 [Autodesk 发明者计算机辅助制造 (CAM) ](https://www.autodesk.com/products/inventor-cam/overview) 软件。 

- **数字电子设备**

    学生使用 [国家仪器的 Multisim](https://www.ni.com/en-us/shop/electronic-test-instrumentation/application-software-for-electronic-test-and-instrumentation-category/what-is-multisim.html) 模拟和线路设计软件来研究电子逻辑电路和设备。

- **工程设计和开发**

    学生提供一种端到端解决方案，将研究、设计和测试这些解决方案组合到工程师的面板中。  在此类中，学生将使用 [Autodesk 的发明者 CAD](https://www.autodesk.com/products/inventor/new-features) 软件。

- **计算机科学基础知识**

    学生被引入了计算概念和工具。  它们从基于块的编程开始，然后使用编码环境（如 [VEXcode V5 块](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/VEXcode+V5+Blocks/VexCode+V5+Blocks+Installation+Guide.pdf)）迁移到使用基于文本的编码。

- **计算机科学原则**
    
    学生使用[Microsoft 的 Visual Studio Code 开发环境](https://code.visualstudio.com/)，利用[Python](https://www.python.org/)提高其编程专业知识。 

- **计算机科学 A**

    学生通过学习移动应用开发，使其在此类中的编程专业技能增长。  在此类中，它们使用[Microsoft 的 Visual Studio Code 开发环境](https://code.visualstudio.com/)来学习[Java](https://www.java.com/) 。  学生还使用模拟器来运行和测试其移动应用代码。  有关如何在 Azure 实验室中设置模拟器的信息，请联系我们 azlabspilot@microsoft.com 。

有关每个类的软件的 [完整列表](https://www.pltw.org/pltw-software) ，请参阅 PLTW 的站点。

## <a name="lab-configuration"></a>实验室配置
若要设置 PLTW 的实验室，需要一个 Azure 订阅和实验室帐户才能开始使用。 如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/)。 获取 Azure 订阅后，可以在 Azure 实验室服务中创建新的实验室帐户。 有关创建新实验室帐户的详细信息，请参阅有关 [如何设置实验室帐户](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account)的教程。 你还可以使用现有的实验室帐户。

获得实验室帐户后，应为 school 提供的 PLTW 类的每个会话创建单独的实验室。  还建议为每种类型的 PLTW 类创建单独的映像。  有关如何构建实验室和映像的详细信息，请阅读博客文章： [从物理实验室迁移到 Azure 实验室服务](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)。

### <a name="lab-account-settings"></a>实验室帐户设置
为实验室帐户启用下表中所述的设置。 有关如何启用 marketplace 映像的详细信息，请参阅有关 [如何指定可用于实验室创建者的 marketplace 映像](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)的文章。

| 实验室帐户设置 | Instructions |
| -------------------- | ----- |
| 市场映像 | 启用 Windows 10 专业版映像，以便在实验室帐户中使用。 |

### <a name="lab-settings"></a>实验室设置
建议用于 PLTW 类的 VM 大小取决于你的学生在类中执行的工作负荷类型。  对于上述类，建议使用大、小 GPU (可视化) VM 大小。  设置 PLTW 类的实验室时，请参阅下表中的指南。

| 实验室设置 | 值/说明 |
| ------------ | ------------------ |
|虚拟机大小| **小型 GPU (可视化)** 。  此 VM 最适合用于使用框架（如 OpenGL 和 DirectX）进行远程可视化、流式处理、游戏、编码。  建议为以下 PLTW 类使用此大小：民事工程和体系结构、数字电子设备、计算机集成的制造;和工程设计和开发。
|虚拟机大小| **大** 。  此大小最适用于需要 CPU 更快、本地磁盘性能更佳、大型数据库、大型内存缓存的应用程序。  建议为以下 PLTW 类使用此大小：工程设计简介、工程设计原则、计算机科学要点、计算机科学原则和计算机科学 A。

### <a name="licensing-server"></a>授权服务器
以上 PLTW 类中使用的大多数软件 *_都不需要_* 访问授权服务器。  但是，如果你计划将 Autodesk 的网络许可模型用于以下软件，你将需要访问许可服务器：
-   Revit
-   发明者
-   发明者凸轮

若要在 Autodesk 的软件中使用网络许可， [PLTW 提供了](https://www.pltw.org/pltw-software) 在许可服务器上安装 Autodesk 许可证管理器的详细步骤。  此授权服务器通常位于本地网络中，或托管在 azure 虚拟网络 (VNet) 中 (VM) 。

设置许可证服务器后，需要将 VNet 与[实验室帐户](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account)[对等](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network)。 必须先完成网络对等互连 _before * 创建实验室，以便实验室虚拟机可以访问许可证服务器和其他方法。

Autodesk 生成的许可证文件嵌入授权服务器的 MAC 地址。  如果决定使用 Azure VM 托管授权服务器，请务必确保授权服务器的 MAC 地址不会更改。   否则，在 MAC 地址更改时，将需要重新生成授权文件。  请遵循以下提示来防止 MAC 地址发生变化：

- 为托管授权服务器的 Azure VM[设置静态专用 IP 和 MAC 地址](https://docs.microsoft.com/azure/lab-services/how-to-create-a-lab-with-shared-resource#static-private-ip-and-mac-address)。
- 请确保在具有足够 VM 容量的 region\location 中设置实验室帐户和授权服务器的 VNet，以便以后无需将这些资源移到新的 region\location 中。

有关详细信息，请参阅 [如何将授权服务器设置为共享资源](https://docs.microsoft.com/azure/lab-services/how-to-create-a-lab-with-shared-resource) 一文。

### <a name="template-machine"></a>模板计算机
PLTW 所需的某些安装文件很大，并且在将其下载到实验室的模板计算机时需要较长的时间进行复制。

建议在物理环境中创建 PLTW 映像，而不是将安装文件下载到模板计算机并在该计算机上安装任何内容。  然后，您可以将图像导入到共享图像库中，以便可以使用这些自定义映像创建实验室。  有关详细信息，请阅读以下文章： [将自定义映像上传到共享映像库](https://docs.microsoft.com/azure/lab-services/upload-custom-image-shared-image-gallery)。

按照此建议，以下是设置实验室的主要任务：

1. 在物理环境中，为类创建图像。

    a.  使用 PLTW 的详细步骤下载安装文件并安装所需的软件。

    > [!NOTE]    
    > 安装 Autodesk 的应用程序时，要在其上安装 Autodesk 的计算机需要能够与你的授权服务器通信 (Autodesk 的安装向导将提示你指定在) 上托管许可证服务器的计算机的计算机名称。  如果在 Azure VM 上托管授权服务器，则可能需要等待在实验室模板计算机上安装 Autodesk，以便 Autodesk 的安装向导可以访问许可服务器

    b.  [安装并配置](https://docs.microsoft.com/azure/lab-services/how-to-prepare-windows-template#install-and-configure-onedrive) 您的学校可能使用) 的 OneDrive (或其他备份选项。
    
    c.  [安装并配置 Windows 更新](https://docs.microsoft.com/azure/lab-services/how-to-prepare-windows-template#install-and-configure-updates)。

1.  将自定义映像上传到 [附加到实验室帐户的共享映像库](https://docs.microsoft.com/azure/lab-services/how-to-attach-detach-shared-image-gallery)。

1.  创建实验室并选择在上一步中上传的自定义映像。

1.  创建实验室后，启动并连接到模板计算机，以验证映像是否按预期方式工作。

1.  最后，发布模板计算机以创建学生的 Vm。

## <a name="student-devices"></a>学生设备
学生可以从 Windows\Mac 计算机和 Chromebook 连接到实验室 Vm。  下面是指向每个选项的说明的链接：

- [从 Windows 进行连接](https://docs.microsoft.com/azure/lab-services/how-to-use-classroom-lab#connect-to-the-vm)
- [从 Mac 连接](https://docs.microsoft.com/azure/lab-services/connect-virtual-machine-mac-remote-desktop)
- [从 Chromebook 连接](https://docs.microsoft.com/azure/lab-services/connect-virtual-machine-chromebook-remote-desktop)

## <a name="cost"></a>Cost
让我们来介绍上述 PLTW 类的可能的成本估算。  此估计不包括运行授权服务器或使用共享映像库的成本。  我们将使用一类25名学生。  计划的类时间有20小时。  此外，每个学生在计划的类时间之外获取用于家庭作业或分配的10小时配额。  有关 **大** **GPU GPU (可视化)** 大小，请参阅以下成本估算。

- **大型 VM**

    25名学生 x (20 个计划小时 + 10 个配额小时) x 70 实验室单位 x 0.01 美元/小时 = 525.00 USD

- **小型 GPU (可视化)**

    25名学生 x (20 个计划小时 + 10 个配额小时) x 160 实验室单位 x 0.01 美元/小时 = 1200.00 USD

> [!IMPORTANT] 
> 成本估算仅用于示例目的。  有关定价的最新详细信息，请参阅 [Azure 实验室服务定价](https://azure.microsoft.com/pricing/details/lab-services/)。

> [!NOTE] 
> 许多 PLTW 类使用通过浏览器访问的应用程序，如 MIT App 发明者。  这些基于浏览器的应用程序不需要快速的 CPU 或 GPU，并且可以从任何具有 internet 连接的设备进行访问。  当学生使用这种类型的应用程序时，我们建议他们在其物理设备上使用浏览器，而不是在实验室 Vm 上使用浏览器。  这将有助于降低成本，只需对需要快速 CPU 或 GPU 的应用程序使用实验室 Vm 即可。

## <a name="next-steps"></a>后续步骤
接下来的步骤是设置任何实验室时通用的：

- [添加用户](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [设置配额](how-to-configure-student-usage.md#set-quotas-for-users)
- [设置日程安排](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [通过电子邮件将注册链接发送给学生](how-to-configure-student-usage.md#send-invitations-to-users)。 