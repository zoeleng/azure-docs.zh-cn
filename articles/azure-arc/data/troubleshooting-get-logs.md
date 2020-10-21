---
title: 获取用于排查启用了 Azure Arc 的数据控制器的日志
description: 获取用于排查启用了 Azure Arc 的数据控制器的服务日志。
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 625092e0557d40051e1ffd538a496c20edc0222f
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92320204"
---
# <a name="get-azure-arc-enabled-data-services-logs"></a>获取启用了 Azure Arc 的数据服务日志

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>先决条件

在继续之前，你需要：

* [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]. [安装说明](./install-client-tools.md)。
* 用于登录到已启用 Azure Arc 的数据服务控制器的管理员帐户。

## <a name="get-azure-arc-enabled-data-services-logs"></a>获取启用了 Azure Arc 的数据服务日志

可以在所有 pod 或特定的 pod 中获得启用了 Azure Arc 的数据服务日志，以便进行故障排除。 你可以使用标准的 Kubernetes 工具（如命令） `kubectl logs` 或在本文中使用该工具来执行此操作，这样可以 [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] 更轻松地一次获取所有日志。

1. 使用管理员帐户登录到数据控制器。

   ```console
   azdata login
   ```

2. 运行以下命令以转储日志：

   ```console
   azdata arc dc debug copy-logs --namespace <namespace name> --exclude-dumps --skip-compress
   ```

   例如：

   ```console
   #azdata arc dc debug copy-logs --namespace arc --exclude-dumps --skip-compress
   ```

数据控制器会在名为的子目录中的当前工作目录中创建日志文件 `logs` 。 

## <a name="options"></a>选项

`azdata arc dc debug copy-logs` 提供以下选项来管理输出。

* 使用参数将日志文件输出到不同的目录 `--target-folder` 。
* 通过省略参数来压缩文件 `--skip-compress` 。
* 通过省略来触发和包含内存转储 `--exclude-dumps` 。 除非 Microsoft 支持部门已请求内存转储，否则不建议使用此方法。 采用内存转储要求将数据控制器设置 `allowDumps` 设置为 `true` 创建数据控制器的时间。
* 筛选以仅收集特定 pod (`--pod`) 或容器 (`--container` 名称) 的日志。
* 通过传递和参数，筛选以收集特定自定义资源的日志 `--resource-kind` `--resource-name` 。 `resource-kind`参数值应为自定义资源定义名称之一，该名称可以通过命令来检索 `kubectl get customresourcedefinition` 。

利用这些参数，可以替换 `<parameters>` 以下示例中的。 

```console
azdata arc dc debug copy-logs --target-folder <desired folder> --exclude-dumps --skip-compress -resource-kind <custom resource definition name> --resource-name <resource name> --namespace <namespace name>
```

例如：

```console
#azdata arc dc debug copy-logs --target-folder C:\temp\logs --exclude-dumps --skip-compress --resource-kind postgresql-12 --resource-name pg1 --namespace arc
```

文件夹层次结构的示例。 文件夹层次结构按 pod 名称、then 容器，然后按目录层次结构在容器中进行组织。

```output
<export directory>
├───debuglogs-arc-20200827-180403
│   ├───bootstrapper-vl8j2
│   │   └───bootstrapper
│   │       ├───apt
│   │       └───fsck
│   ├───control-j2dm5
│   │   ├───controller
│   │   │   └───controller
│   │   │       ├───2020-08-27
│   │   │       └───2020-08-28
│   │   └───fluentbit
│   │       ├───agent
│   │       ├───fluentbit
│   │       └───supervisor
│   │           └───log
│   ├───controldb-0
│   │   ├───fluentbit
│   │   │   ├───agent
│   │   │   ├───fluentbit
│   │   │   └───supervisor
│   │   │       └───log
│   │   └───mssql-server
│   │       ├───agent
│   │       ├───mssql
│   │       ├───mssql-server
│   │       └───supervisor
│   │           └───log
│   ├───controlwd-ln6j8
│   │   └───controlwatchdog
│   │       └───controlwatchdog
│   ├───logsdb-0
│   │   └───elasticsearch
│   │       ├───agent
│   │       ├───elasticsearch
│   │       ├───provisioner
│   │       └───supervisor
│   │           └───log
│   ├───logsui-7gg2d
│   │   └───kibana
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───kibana
│   │       └───supervisor
│   │           └───log
│   ├───metricsdb-0
│   │   └───influxdb
│   │       ├───agent
│   │       ├───influxdb
│   │       └───supervisor
│   │           └───log
│   ├───metricsdc-2f62t
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-jznd2
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-n5vnx
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsui-h748h
│   │   └───grafana
│   │       ├───agent
│   │       ├───grafana
│   │       └───supervisor
│   │           └───log
│   └───mgmtproxy-r5zxs
│       ├───fluentbit
│       │   ├───agent
│       │   ├───fluentbit
│       │   └───supervisor
│       │       └───log
│       └───service-proxy
│           ├───agent
│           ├───nginx
│           └───supervisor
│               └───log
└───debuglogs-kube-system-20200827-180431
    ├───coredns-8bbb65c89-kklt7
    │   └───coredns
    ├───coredns-8bbb65c89-z2vvr
    │   └───coredns
    ├───coredns-autoscaler-5585bf8c9f-g52nt
    │   └───autoscaler
    ├───kube-proxy-5c9s2
    │   └───kube-proxy
    ├───kube-proxy-h6x56
    │   └───kube-proxy
    ├───kube-proxy-nd2b7
    │   └───kube-proxy
    ├───metrics-server-5f54b8994-vpm5r
    │   └───metrics-server
    └───tunnelfront-db87f4cd8-5xwxv
        ├───tunnel-front
        │   ├───apt
        │   └───journal
        └───tunnel-probe
            ├───apt
            ├───journal
            └───openvpn
```

## <a name="next-steps"></a>后续步骤

[azdata arc dc debug copy-logs](/sql/azdata/reference/reference-azdata-arc-dc-debug#azdata-arc-dc-debug-copy-logs?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json)