---
title: 如何在 Azure 春季云中使用权限
description: 本文介绍如何在 Azure 春季云中为权限创建自定义角色。
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/04/2020
ms.custom: devx-track-java
ms.openlocfilehash: edd9898580c60199b761b24b172a366069f09cb2
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92516182"
---
# <a name="how-to-use-permissions-in-azure-spring-cloud"></a>如何在 Azure 春季云中使用权限
本文介绍如何创建将权限委托给 Azure 春季云资源的自定义角色。 自定义角色利用各种股票权限扩展 [Azure 内置角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) 。

我们将实现以下自定义角色：

* **开发人员角色**： 
    * 部署
    * 测试
    * 重新启动应用
    * 可以应用并对 git 存储库中的应用配置进行更改
    * 可以获取日志流
* **Ops-站点可靠性工程**： 
    * 重新启动应用
    * 获取日志流
    * 无法对应用或配置进行更改
* **Azure Pipelines/Jenkins/Github 操作角色**：
    * 可以执行创建、读取、更新、删除操作
    * 可以在 Azure 春季云和应用内的服务实例中创建和配置所有内容： Azure Pipelines、Jenkins 或 GitHub 操作，使用 Terraform 或 ARM 模板

## <a name="define-developer-role"></a>定义开发人员角色

开发人员角色包括重新启动应用程序和查看其日志流的权限，但不能对应用程序、配置进行更改。

### <a name="navigate-subscription-and-resource-group-access-control-iam"></a> (IAM) 导航订阅和资源组访问控制

按照以下步骤开始定义角色。

1. 在 Azure 门户中，打开要将自定义角色分配到的订阅和资源组。
2. ** (IAM) 打开访问控制**。
3. 单击“+ 添加”。
4. 单击 " **添加自定义角色**"。
5. 单击“下一步”。

   ![创建自定义角色](media/spring-cloud-permissions/create-custom-role.png)

6. 单击“添加权限”。

   ![添加权限启动](media/spring-cloud-permissions/add-permissions.png)

### <a name="search-for-azure-spring-cloud-permissions"></a>搜索 Azure 春季 Cloud 权限：
7. 在搜索框中，搜索 " *Microsoft. 应用*"。
选择 *Microsoft Azure 春季云*"。

   ![选择 Azure 春季云](media/spring-cloud-permissions/spring-cloud-permissions.png)

8. 选择 "开发人员" 角色的权限：

从 **AppPlatform/春季**选择：
* 写入：创建或更新 Azure 春季云服务实例
* 读取：获取 Azure 春季云服务实例
* 其他：列出 Azure 春季云服务实例测试密钥

在 **AppPlatform/春季/apps**中，选择：
* 读取：读取 Microsoft Azure 春季云应用程序
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序资源上传 URL

从 **AppPlatform/弹簧/apps/bindings**中，选择：
* 读取：读取 Microsoft Azure 春季云应用程序绑定

从 **AppPlatform/春季/apps/部署**中选择：
* 编写：编写 Microsoft Azure 春季云应用程序部署
* 阅读：阅读 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：开始 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：停止 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：重启 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序部署日志文件 URL

从 " **AppPlatform/春季/apps/域**" 中，选择：
* 读取：读取 Microsoft Azure 春季 Cloud 应用程序自定义域

从 **AppPlatform/春季/证书**中，选择：
* 读取：读取 Microsoft Azure 春季云证书

从 **AppPlatform/位置/operationResults/弹簧**中，选择：
* Read：读取操作结果

从 **AppPlatform/位置/operationStatus/operationId**中，选择：
* 读取：读取操作状态

    [![创建 Developler 权限 ](media/spring-cloud-permissions/developer-permissions-box.png)](media/spring-cloud-permissions/developer-permissions-box.png#lightbox)

9. 单击“添加”。

10. 查看权限。

11. 单击“审阅并创建”。

## <a name="define-devops-engineer-role"></a>定义 DevOps 工程师角色
此过程定义有权部署、测试和重新启动 Azure 春季云应用的角色。

1. 重复此过程，导航 "订阅"、"资源组" 和 "访问权限控制" (IAM) 。
2. 选择 DevOps 工程师角色的权限：

从 **AppPlatform/春季**选择：
* 写入：创建或更新 Azure 春季云服务实例
* 删除：删除 Azure 春季云服务实例
* 读取：获取 Azure 春季云服务实例
* 其他：启用 Azure 春季云服务实例测试终结点
* 其他：禁用 Azure 春季云服务实例测试终结点
* 其他：列出 Azure 春季云服务实例测试密钥
* 其他：重新生成 Azure 春季云服务实例测试密钥

在 **AppPlatform/春季/apps**中，选择：
* 写入：写入 Microsoft Azure 春季云应用程序
* 删除：删除 Microsoft Azure 春季云应用程序
* 读取：读取 Microsoft Azure 春季云应用程序
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序资源上传 URL
* 其他：验证 Microsoft Azure 春季 Cloud 应用程序自定义域

从 **AppPlatform/弹簧/apps/bindings**中，选择：
* 写入：写入 Microsoft Azure 春季云应用程序绑定
* 删除：删除 Microsoft Azure 弹簧云应用程序绑定
* 读取：读取 Microsoft Azure 春季云应用程序绑定

从 **AppPlatform/春季/apps/部署**中选择：
* 编写：编写 Microsoft Azure 春季云应用程序部署
* 删除：删除 Azure 春季 Cloud 应用程序部署
* 阅读：阅读 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：开始 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：停止 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：重启 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序部署日志文件 URL

从 **AppPlatform/春季/apps/部署/sku**中，选择：
* 阅读：列出应用程序部署可用 sku

从 " **AppPlatform"/"位置**" 中，选择：
* 其他：检查名称可用性

From AppPlatform/位置/operationResults/弹簧选择：读取：读取操作结果

从 **AppPlatform/位置/operationStatus/operationId**中，选择：
* 读取：读取操作状态

从 **AppPlatform/sku**中，选择：
* 读取：列出可用的 sku

   [![开发/操作权限 ](media/spring-cloud-permissions/dev-ops-permissions.png)](media/spring-cloud-permissions/dev-ops-permissions.png#lightbox)

3. 单击“添加”。

4. 查看权限。

5. 单击“审阅并创建”。

## <a name="define-ops---site-reliability-engineering-role"></a>定义 Ops-站点可靠性工程角色
此过程定义有权部署、测试和重新启动 Azure 春季云应用的角色。

1. 重复此过程，导航 "订阅"、"资源组" 和 "访问权限控制" (IAM) 。

2. 选择 Ops-Site 可靠性工程角色的权限：

从 **AppPlatform/春季**选择：
* 读取：获取 Azure 春季云服务实例
* 其他：列出 Azure 春季云服务实例测试密钥

在 **AppPlatform/春季/apps**中，选择：
* 读取：读取 Microsoft Azure 春季云应用程序

从 **AppPlatform/apps/部署**中选择：
* 阅读：阅读 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：开始 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：停止 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：重启 Microsoft Azure 春季 Cloud 应用程序部署

从 **AppPlatform/位置/operationResults/弹簧**中，选择：
* Read：读取操作结果

从 **AppPlatform/位置/operationStatus/operationId**中，选择：
* 读取：读取操作状态

   [![Ops/SRE 权限 ](media/spring-cloud-permissions/ops-sre-permissions.png)](media/spring-cloud-permissions/ops-sre-permissions.png#lightbox)

3. 单击“添加”。

4. 查看权限。

5. 单击“审阅并创建”。

## <a name="define-azure-pipelinesprovisioning-role"></a>定义 Azure Pipelines/设置角色
此 Jenkins/Github 操作角色可以通过服务实例在 Azure 春季云和应用中创建和配置所有内容。 此角色用于发布或部署代码。

1. 重复此过程，导航 "订阅"、"资源组" 和 "访问权限控制" (IAM) 。

2. 打开 **权限** 选项。

3. 选择 Azure Pipelines/设置角色的权限：
  
从 **AppPlatform/春季**选择：
* 写入：创建或更新 Azure 春季云服务实例
* 删除：删除 Azure 春季云服务实例
* 读取：获取 Azure 春季云服务实例
* 其他：启用 Azure 春季云服务实例测试终结点
* 其他：禁用 Azure 春季云服务实例测试终结点
* 其他：列出 Azure 春季云服务实例测试密钥
* 其他：重新生成 Azure 春季云服务实例测试密钥

在 **AppPlatform/春季/apps**中，选择：
* 写入：写入 Microsoft Azure 春季云应用程序
* 删除：删除 Microsoft Azure 春季云应用程序
* 读取：读取 Microsoft Azure 春季云应用程序
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序资源上传 URL
* 其他：验证 Microsoft Azure 春季 Cloud 应用程序自定义域

从 **AppPlatform/弹簧/apps/bindings**中，选择：
* 写入：写入 Microsoft Azure 春季云应用程序绑定
* 删除：删除 Microsoft Azure 弹簧云应用程序绑定
* 读取：读取 Microsoft Azure 春季云应用程序绑定

从 **AppPlatform/春季/apps/部署**中选择：
* 编写：编写 Microsoft Azure 春季云应用程序部署
* 删除：删除 Azure 春季 Cloud 应用程序部署
* 阅读：阅读 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：开始 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：停止 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：重启 Microsoft Azure 春季 Cloud 应用程序部署
* 其他：获取 Microsoft Azure 春季 Cloud 应用程序部署日志文件 URL

从 **AppPlatform/sku**中，选择：
* 读取：列出可用的 sku

从 " **AppPlatform"/"位置**" 中，选择：
* 其他：检查名称可用性

从 **AppPlatform/位置/operationResults/弹簧**中，选择：
* Read：读取操作结果

从 **AppPlatform/位置/operationStatus/operationId**中，选择：
* 读取：读取操作状态

从 **AppPlatform/sku**中，选择：
* 读取：列出可用的 sku

   [![管道权限 ](media/spring-cloud-permissions/pipelines-permissions-box.png)](media/spring-cloud-permissions/pipelines-permissions-box.png#lightbox)  

4. 单击“添加”。

5. 查看权限。

6. 单击“审阅并创建”。


## <a name="see-also"></a>另请参阅
* [使用 Azure 门户创建或更新 Azure 自定义角色](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-portal)

有关定义自定义权限的三种方法的详细信息，请参阅：
* [克隆角色](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-portal#clone-a-role)
* [从头开始](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-portal#start-from-scratch)
* [从 JSON 开始](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-portal#start-from-json)