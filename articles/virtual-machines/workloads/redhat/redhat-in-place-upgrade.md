---
title: Azure 上的 Red Hat Enterprise Linux 映像的就地升级
description: 查找从 Red Hat Enterprise 7、windows 映像执行就地升级到最新的2.x 版的步骤
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: beede74134affeb3ee0d4bdd20d5da3b4c5e6eda
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566616"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Red Hat Enterprise Linux 就地升级

本文提供了有关如何使用 Azure 上的 "Leapp" 实用程序执行从 Red Hat Enterprise Linux 7 到 Red Hat Enterprise Linux 8 的就地升级的分步说明。 在就地升级过程中，现有的 RHEL 7 操作系统将替换为 RHEL 8 版本。

>[!Note] 
> Red Hat Enterprise Linux 产品上的 SQL Server 在 Azure 上不支持就地升级。

## <a name="what-to-expect-during-the-upgrade"></a>升级过程中的预期情况
在升级过程中，系统将重新启动几次，这是正常的。 上次重新启动将 VM 升级到 RHEL 8 最新次要版本。 

升级过程可能需要20分钟到几小时的时间，这取决于若干因素，例如 VM 大小和系统上安装的包的数量。

## <a name="preparations-for-the-upgrade"></a>升级准备工作
使用 Red Hat 和 Azure 的推荐式升级方法，使客户能够将系统升级到下一个主要版本。 在执行升级之前，您应注意并考虑。 

>[!Important] 
> 请在执行升级前拍摄映像快照。

>[!NOTE]
> 本文中的命令需要使用根帐户运行

* 请确保使用的是最新的 RHEL 7 版本，当前为 RHEL 7.9。 如果你使用的是锁定的版本，并且不能升级到 RHEL 7.9，则可以使用 [此处的步骤切换到非 EUS 存储库](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock)。

* 运行以下命令来了解升级的方式，以及是否将完成。 此命令应在 "/var/log/leapp/leapp-report.txt" 下生成一个文件，该文件说明了该过程以及正在进行的操作以及升级是否可行
    ```bash
    leapp preupgrade --no-rhsm
    ```
* 确保串行控制台正常工作，因为这样可以在升级过程中进行监视。

* 在中启用 SSH 根访问 `/etc/ssh/sshd_config`
    1. 打开文件 `/etc/ssh/sshd_config`
    1. 搜索 "#PermitRootLogin 是"
    1. 删除 "#" 以取消注释

## <a name="steps-for-performing-the-upgrade"></a>执行升级的步骤

请仔细执行这些步骤。 在生产实例之前，建议在测试计算机上尝试升级。

1. 执行 yum 更新以提取最新的客户端包。
    ```bash
    yum update -y
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

1. 执行 "Leapp" 升级。
    ```bash
    leapp upgrade --no-rhsm
    ```
1.  `leapp upgrade`命令成功完成后，手动重新启动系统以完成该过程。 系统将重新启动几次，因为它将不可用。 使用串行控制台监视进程。

1.  验证升级是否已成功完成。
    ```bash
    uname -a && cat /etc/redhat-release
    ```

1. 升级完成后，请删除根 ssh 访问权限。
    1. 打开文件 `/etc/ssh/sshd_config`
    1. 搜索 "#PermitRootLogin 是"
    1. 将 "#" 添加到注释

1. 重新启动 sshd 服务以使更改生效
    ```bash
    systemctl restart sshd
    ```

## <a name="common-issues"></a>常见问题
这些是 `leapp preupgrade` 或 `leapp upgrade` 进程可能会失败的一些常见实例。

**错误：找不到以下已禁用的插件模式的匹配项**
```plaintext
STDERR:
No matches found for the following disabled plugin patterns: subscription-manager
Warning: Packages marked by Leapp for upgrade not found in repositories metadata: gpg-pubkey
```
**解决方案**\
编辑文件 `/etc/yum/pluginconf.d/subscription-manager.conf` 并将 "已启用" 更改为，禁用订阅管理器插件 `enabled=0` 。

这是因为启用了订阅管理器 yum 插件，该插件不用于 PAYG Vm。

**错误：使用 root 远程登录时可能出现的问题**`leapp preupgrade`可能会失败，并出现以下错误：
```structured-text
============================================================
                     UPGRADE INHIBITED
============================================================

Upgrade has been inhibited due to the following problems:
    1. Inhibitor: Possible problems with remote login using root account
Consult the pre-upgrade report for details and possible remediation.

============================================================
                     UPGRADE INHIBITED
============================================================
```
**解决方案**\
在中启用根访问 `/etc/sshd_conf` 。
这是由于未按照 `/etc/sshd_conf` "[升级准备](#preparations-for-the-upgrade)" 一节中的启用根 ssh 访问的原因。 

## <a name="next-steps"></a>后续步骤
* 详细了解 [Azure 中的 Red Hat 映像](./redhat-images.md)。
* 详细了解 [Red Hat 更新基础结构](./redhat-rhui.md)。
* 了解有关 [RHEL BYOS 产品/服务](./byos.md)的详细信息。
* 有关 Red Hat 就地升级过程的信息，可参阅 Red Hat 文档， [从 RHEL 7 升级到 RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index)
* 可以在 [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)（Red Hat Enterprise Linux 生命周期）页找到有关 RHEL 所有版本的 Red Hat 支持策略的信息。