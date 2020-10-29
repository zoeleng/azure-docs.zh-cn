---
title: Azure Monitor API 停用
description: 介绍旧版本的 Microsoft.operationalinsights 资源提供程序 API 的停用。
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 7ddfb3221c73c740a339dd8d9d4f60cca46c6425
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026762"
---
# <a name="operationalinsights-api-version-retirement"></a>Microsoft.operationalinsights API 版本停用
Microsoft 将在停用 API 之前至少12个月提供通知，以便顺利过渡到较新的/受支持的版本。 我们发布了新版本 (2020-08-01) **microsoft.operationalinsights** 资源提供程序 api，并将在2024年2月29日停用任何早期的 API 版本。

我们鼓励你立即开始使用版本2020-08-01，以获得新功能的优点，如 [专用群集](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters)、 [客户管理的密钥](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys)、 [专用链接](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security) 和 [数据导出](https://docs.microsoft.com/azure/azure-monitor/platform/logs-data-export)。 此外，新特性和功能以及优化仅添加到当前 API。

2024年2月29日后，Azure Monitor 将不再支持比2020-08-01 更早的 Api 版本。 如果不想升级，则在2024年2月29日之前，Azure Monitor 服务将继续提供从早期版本发送的请求。

## <a name="migration-steps"></a>迁移步骤
根据所使用的配置方法，应更新 **REST** 请求和 **资源管理器模板** 中的新版本。 请按照以下示例更新 API 版本：

1. REST API 请求在请求的 URL 中使用 API 版本。 将该版本替换为最新版本 (2020-08-01) ，如下面的示例中所示。

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure 资源管理器模板在资源的 **apiVersion** 属性中使用 API 版本。 将该版本替换为最新版本 (2020-08-01) ，如下面的示例中所示。


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>后续步骤

- 请参阅 [microsoft.operationalinsights 工作区 API 的参考](/rest/api/loganalytics/workspaces)。
