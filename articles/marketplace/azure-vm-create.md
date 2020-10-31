---
title: 在 Azure Marketplace 中创建虚拟机产品/服务。
description: 了解如何在 Microsoft 商业应用商店中创建虚拟机产品/服务。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/20/2020
ms.openlocfilehash: bfd67688ce30b62002e26cea9f7be4df1cb6e622
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93126441"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>如何在 Azure Marketplace 上创建虚拟机产品/服务

本文介绍如何为 [Azure Marketplace](https://azuremarketplace.microsoft.com/)创建 azure 虚拟机产品/服务。 它介绍基于 Windows 和基于 Linux 的虚拟机（包含操作系统、虚拟硬盘 (VHD) 以及多达 16 个数据磁盘）。

在开始之前，请 [在合作伙伴中心创建一个商用 marketplace 帐户](partner-center-portal/create-account.md)。 确保帐户已注册加入商业市场计划。

## <a name="before-you-begin"></a>开始之前

如果尚未这样做，请查看 [规划虚拟机产品/服务](marketplace-virtual-machines.md)。 它将介绍虚拟机的技术要求，并列出创建产品/服务时所需的信息和资产。 

## <a name="create-a-new-offer"></a>创建新套餐

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard/home)。
2. 在左侧窗格中，选择“商业市场” > “概述”。
3. 在“概述”页上，选择“新建产品/服务”  > “Azure 虚拟机”。

    ![显示左窗格菜单选项和“新建产品/服务”按钮的屏幕截图。](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> 发布产品/服务后，在合作伙伴中心对其进行的任何编辑仅会在重新发布该产品/服务后出现在 Azure 市场中。 确保对其进行更改后始终重新发布产品/服务。

输入“产品/服务 ID”。 这是你帐户中的每个产品/服务的唯一标识符。

- 客户可以在 Azure 市场产品/服务、Azure PowerShell 和 Azure CLI 的 Web 地址中看到此 ID（若有）。
- 只使用小写字母和数字。 该 ID 可以包含连字符和下划线字符，但不能包含空格，并且不得超过 50 个字符。 例如，如果你输入“test-offer-1”，则产品/服务 Web 地址为 `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`。
- 在选择“创建”后，就无法更改产品/服务 ID 了。

输入“产品/服务别名”。 该产品/服务别名是用于合作伙伴中心内产品/服务的名称。

- 此名称不用于 Azure 市场。 它不同于向客户显示的产品/服务名称和其他值。

## <a name="enable-a-test-drive-optional"></a> (可选) 启用测试驱动器

测试驱动器是向潜在客户展示您的产品/服务的一种很好的方式，通过在固定的小时数内提供对预配置环境的访问权限。 提供一个测试驱动器将导致转换速率增加，并生成高度合格的潜在顾客。 若要了解有关测试驱动器的详细信息，请参阅 [什么是测试驱动器？](./what-is-test-drive.md)。

> [!TIP]
> 测试驱动器与免费试用版不同。 可以提供测试驱动器、免费试用版或同时提供两者。 它们都向客户提供固定时间段的解决方案。 但在实际实施方案中，一个测试驱动器还包括一项针对您的产品关键功能的动手自学教程。

**启用测试驱动器**
1.  在 " **测试驱动器** " 下，选中 " **启用测试驱动器** " 复选框。
1.  从显示的列表中选择 "测试驱动器" 类型。

## <a name="configure-lead-management"></a>配置潜在顾客管理

将产品/服务发布到合作伙伴中心的商业市场时，将其连接到客户关系管理 (CRM) 系统。 这样，只要有人对你的产品表示兴趣或使用你的产品，你就可以收到客户的联系信息。 如果要启用测试驱动器，则需要连接到 CRM (参阅前面的) 部分。 否则，连接到 CRM 是可选的选项。

1. 在 " **客户主管** " 下，选择 " **连接** " 链接。
1. 在 " **连接详细信息** " 对话框中，从列表中选择潜在顾客目标。
1. 填写显示的字段。 有关详细步骤，请参阅以下文章：

   - [将你的产品/服务配置为将潜在顾客发送到 Azure 表](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
   - [将你的产品/服务配置为将潜在顾客发送到 dynamics 365 客户参与](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (以前的 Dynamics CRM Online) 
   - [配置你的产品/服务以将潜在客户发送到 HTTPS 终结点](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
   - [配置你的产品/服务以将潜在顾客发送到 Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
   - [配置你的产品/服务以将潜在顾客发送到 Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

1. 若要验证提供的配置，请选择 " **验证** " 链接。
1. 若要关闭对话框，请选择 **"确定"** 。

## <a name="resell-through-csps"></a>通过 CSP 转售

可通过向[云解决方案提供商](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP) 计划中的合作伙伴提供产品/服务来扩展其覆盖范围。 所有自带许可 (BYOL) 计划将自动选择加入该计划。 还可选择加入到非 BYOL 计划。

选择“创建”，以生成产品/服务，并继续操作。

## <a name="next-steps"></a>后续步骤

- [配置虚拟机产品/服务属性](azure-vm-create-properties.md)
- [套餐列出最佳做法](gtm-offer-listing-best-practices.md)