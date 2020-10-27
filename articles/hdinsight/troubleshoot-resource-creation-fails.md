---
title: 排查 Azure HDInsight 中的资源创建失败问题
description: 本文提供常见的容量问题错误和缓解方法。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: a47781bcdf60d863d6dcf77d70bc0ee496a51388
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92538793"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>排查 Azure HDInsight 中的资源创建失败问题

本文介绍在与 Azure HDInsight 交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>错误：部署将超过配额“800”。

Azure 的配额限制为每个资源组 800 个部署。 将按资源组、订阅、帐户和其他范围应用不同的配额。 例如，订阅可能配置为限制某个区域的核心数目。 尝试部署内核数超过允许数量的虚拟机时将导致错误消息，错误消息指出已超出配额。

若要解决此问题，请使用 Azure 门户、CLI 或 PowerShell 删除不再需要的部署。

有关详细信息，请参阅[解决资源配额错误](../azure-resource-manager/templates/error-resource-quota.md)。

## <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>错误：最大节点已超过此区域中的可用核心数目

订阅可能配置为限制某个区域的核心数目。 尝试部署内核数超过允许数量的资源时将导致错误消息，错误消息指出已超出配额。

若要请求增加配额，请按以下步骤操作：

1. 转到 

1. 选择“新建支持请求”  。

1. 在“新建支持请求”页的“基本信息”选项卡上提供以下信息：  

   * <bpt id="p1">**</bpt>Issue type:<ept id="p1">**</ept> Select <bpt id="p2">**</bpt>Service and subscription limits (quotas)<ept id="p2">**</ept>.
   * **订阅：** 选择要修改的订阅。
   * <bpt id="p1">**</bpt>Quota type:<ept id="p1">**</ept> Select <bpt id="p2">**</bpt>HDInsight<ept id="p2">**</ept>.

有关详细信息，请参阅[创建支持票证来增加核心](hdinsight-capacity-planning.md#quotas)。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 联系 [@AzureSupport](https://twitter.com/azuresupport)，这是用于改进客户体验的官方 Microsoft Azure 帐户。 它可以将 Azure 社区成员连接到适当的资源，为他们提供解答、支持和专家建议。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”，或打开“帮助 + 支持”中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](../azure-portal/supportability/how-to-create-azure-support-request.md)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。