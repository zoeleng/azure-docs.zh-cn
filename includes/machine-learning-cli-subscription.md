---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 428a3ad17c81b465635207de622398e814289d87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "81616819"
---
> [!TIP]
> 登录后，你将看到与你的 Azure 帐户关联的订阅列表。 在 `isDefault: true` 的情况下显示的订阅信息是 Azure CLI 命令的当前已激活的订阅。 此订阅必须与包含 Azure 机器学习工作区的订阅相同。 可以通过访问工作区的概述页从 [Azure 门户](https://portal.azure.com)找到订阅 ID。 还可以使用 SDK 从工作区对象获取订阅 ID。 例如，`Workspace.from_config().subscription_id`。
> 
> 若要选择另一个订阅，请使用 `az account set -s <subscription name or ID>` 命令并指定要切换到的订阅名称或 ID。 有关订阅选择的详细信息，请参阅[使用多个 Azure 订阅](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)。