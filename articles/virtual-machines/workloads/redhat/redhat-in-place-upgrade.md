---
title: Azure 上的 Red Hat Enterprise Linux 映像的就地升级
description: 查找从 Red Hat Enterprise 7、windows 映像执行就地升级到最新的2.x 版的步骤
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: 48884e6faa5f26f027c772b44d5f960979a40d1d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447575"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Red Hat Enterprise Linux 就地升级

本文提供了有关如何使用 Azure 上的 "Leapp" 实用程序执行从 Red Hat Enterprise Linux 7 到 Red Hat Enterprise Linux 8 的就地升级的分步说明。 在就地升级过程中，现有的 RHEL 7 操作系统将替换为 RHEL 8 版本。

>[!Note] 
> Red Hat Enterprise Linux 产品上的 SQL Server 在 Azure 上不支持就地升级。

## <a name="what-to-expect-during-the-upgrade"></a>升级过程中的预期情况
在升级过程中，系统将重新启动几次，这是正常的。 上次重新启动将 VM 升级到 RHEL 8 最新次要版本。

## <a name="preparations-for-the-upgrade"></a>升级准备工作
Red Hat 和 Azure 的推荐使用就地升级，使客户能够将系统升级到下一个主要版本。 在执行升级之前，您应注意并考虑。 

>[!Important] 
> 请在执行升级前拍摄映像快照。

>[!NOTE]
> 本文中的命令需要使用根帐户运行

* 请确保使用的是最新的 RHEL 7 版本，当前为 RHEL 7.9。 如果你使用的是锁定的版本，并且不能升级到 RHEL 7.9，则可以使用 [此处的步骤切换到非 EUS 存储库](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock)。

* 运行以下命令来了解升级的方式，以及是否将完成。 此命令应在 "/var/log/leapp/leapp-report.txt" 下生成一个文件，该文件说明了该过程以及正在进行的操作以及升级是否可行
    ```bash
    leapp preupgrade --no-rhsm
    ```

## <a name="steps-for-performing-the-upgrade"></a>执行升级的步骤

请仔细执行这些步骤。 在生产实例之前，建议在测试计算机上尝试升级。

1. 执行 yum 更新以提取最新的客户端包。
    ```bash
    yum update
    ```

1. 安装 leapp。
    ```bash
    yum install leapp-rhui-azure
    ```
    
1. 在 [RedHat 门户](https://access.redhat.com/articles/3664871)中使用 repomap.csv 和 pes-events.js的 leapp-data 文件，并将其解压缩。 
    1. 下载文件。
    1. 使用以下命令提取内容并删除文件：
    ```bash
     tar -xzf leapp-data12.tar.gz -C /etc/leapp/files && rm leapp-data12.tar.gz
    ```
    


1. 为 "Leapp" 添加 "答案" 文件。
    ```bash
    leapp answer --section remove_pam_pkcs11_module_check.confirm=True --add
    ```
    
1. 在/etc/ssh/中启用 PermitRootLogin sshd_config
    1. 打开文件/etc/ssh/sshd_config
    1. 搜索 "#PermitRootLogin 是"
    1. 删除 "#" 以取消注释



1. 执行 "Leapp" 升级。
    ```bash
    leapp upgrade --no-rhsm
    ```
1. 重新启动 sshd 服务以使更改生效
    ```bash
    systemctl restart sshd
    ```
1. 在/etc/ssh/中注释掉 PermitRootLogin sshd_config
    1. 打开文件/etc/ssh/sshd_config
    1. 搜索 "#PermitRootLogin 是"
    1. 将 "#" 添加到注释

## <a name="next-steps"></a>后续步骤
* 详细了解 [Azure 中的 Red Hat 映像](./redhat-images.md)。
* 详细了解 [Red Hat 更新基础结构](./redhat-rhui.md)。
* 了解有关 [RHEL BYOS 产品/服务](./byos.md)的详细信息。
* 有关 Red Hat 就地升级过程的信息，可参阅 Red Hat 文档， [从 RHEL 7 升级到 RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index)
* 可以在 [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)（Red Hat Enterprise Linux 生命周期）页找到有关 RHEL 所有版本的 Red Hat 支持策略的信息。