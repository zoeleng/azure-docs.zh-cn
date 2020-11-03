---
title: 启用了 Azure Arc 的数据服务-发行说明
description: 最新发行说明
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 10/29/2020
ms.topic: conceptual
ms.openlocfilehash: e7312ffd4d55f0403359f8aad2d0a8433a716f77
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280373"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>发行说明-启用了 Azure Arc 的数据服务 (预览) 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="october-2020"></a>2020 年 10 月 

### <a name="breaking-changes"></a>中断性变更

此版本引入了以下重大更改： 

* PostgreSQL 自定义资源定义 (.CRD) 文件替换此术语将 `shards` 重命名为 `workers` 。 此术语 (`workers`) 与命令行参数名称匹配。

* `azdata arc postgres server delete` 删除 postgres 实例之前提示确认。  使用 `--force` 跳过提示。

### <a name="additional-changes"></a>其他更改

* 添加了一个名为的新可选参数 `azdata arc postgres server create` `--volume-claim mounts` 。 该值是以逗号分隔的卷声明装载列表。 卷声明装载是一对卷类型和 PVC 名称。 目前的卷类型仅允许 `backup` 。  在 PostgreSQL 中，当卷类型为时 `backup` ，PVC 将装载到 `/mnt/db-backups` 。  这样就可以在 Postgres 实例之间共享备份，以便可以在另一个 Postgres 实例中还原备份。

* 新的 PostgresSQL 自定义资源定义的短名称： 

  * `pg11` 

  * `pg12`

* 通过使用以下任一方法为用户提供遥测数据：

   * 上载到 Azure 的点数

     或 

   * 如果没有数据加载到 Azure，则会提示再次尝试。

* `azdata arc dc debug copy-logs` 现在还会读取 `/var/opt/controller/log` 文件夹并收集 postgres 日志。

*   在 postgres 创建和还原备份过程中显示工作指示器。

* `azdata arc postrgres backup list` 现在包含备份大小信息。

* 已将 SQL 托管实例 admin name 属性添加到 Azure 门户中 "概述" 边栏选项卡的右侧列。



## <a name="september-2020"></a>2020 年 9 月

已为公共预览版发布启用了 Azure Arc 的数据服务。 启用 Arc 的数据服务允许你在任何位置管理数据服务。

- SQL 托管实例
- PostgreSQL 超大规模

有关说明，请参阅 [什么是启用了 Azure Arc 的数据服务？](overview.md)

## <a name="known-limitations-and-issues"></a>已知限制和问题

- SQL 托管实例名称不能超过13个字符
- Azure Arc 数据控制器或数据库实例没有就地升级。
- 启用了 Arc 的数据服务容器映像未签名。  你可能需要将 Kubernetes 节点配置为允许拉取未签名的容器映像。  例如，如果使用 Docker 作为容器运行时，则可以设置 DOCKER_CONTENT_TRUST = 0 环境变量并重新启动。  其他容器运行时具有类似的选项，例如在 [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration) 中就是如此。
- 无法从 Azure 门户创建启用了 Azure Arc 的 SQL 托管实例或 PostgreSQL 超大规模服务器组。
- 现在，如果使用的是 NFS，则需要 `allowRunAsRoot` `true` 在创建 Azure Arc 数据控制器之前在部署配置文件中将设置为。
- 仅限 SQL 和 PostgreSQL 登录身份验证。  不支持 Azure Active Directory 或 Active Directory。
- 在 OpenShift 上创建数据控制器需要严格的安全约束。  有关详细信息，请参阅文档。
- 不支持缩放 PostgresSQL 超大规模辅助 _角色节点的_ 数量。
- 如果使用 azure Kubernetes 服务引擎 (使用 Azure Arc 数据控制器和数据库实例的 Azure Stack 集线器上的 AKS Engine) ，则不支持升级到较新的 Kubernetes 版本。 请先卸载 Azure Arc 数据控制器和所有数据库实例，然后再升级 Kubernetes 群集。
- 预览不支持 Postgres 版本11引擎的备份/还原。 它仅支持 Postgres 版本12的备份/还原。
- Azure Kubernetes Service (AKS) ，支持支持 Azure Arc 的数据服务目前不支持跨 [多个可用性区域](../../aks/availability-zones.md) 的群集。 若要避免此问题，在 Azure 门户中创建 AKS 群集时，如果选择区域可用的区域，请从选择控件中清除所有区域。 参看下图：

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="清除每个区域的复选框以指定 &quot;无&quot;。":::

## <a name="next-steps"></a>后续步骤
  
> 想尝试一下吗？  
> 在 Azure Kubernetes 服务 (AKS)、AWS Elastic Kubernetes 服务 (EKS)、Google Cloud Kubernetes Engine (GKE) 或 Azure VM 中，通过 [Azure Arc 快速入门](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services)快速开始操作。

[安装客户端工具](install-client-tools.md)

[创建 Azure Arc 数据控制器](create-data-controller.md)（首先需要安装客户端工具）

[在 Azure Arc 上创建 Azure SQL 托管实例](create-sql-managed-instance.md)（首先需要创建 Azure Arc 数据控制器）

[在 Azure Arc 上创建 Azure Database for PostgreSQL 超大规模服务器组](create-postgresql-hyperscale-server-group.md)（首先需要创建 Azure Arc 数据控制器）
