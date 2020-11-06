---
title: VM 在等待本地会话管理器时无响应
description: 本文提供了一些步骤，用于解决在启动 Azure VM 时来宾操作系统停滞等待本地会话管理器完成处理的问题。
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: ae3d35bea1c75c797447100316ceab9a27fcbe48
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423955"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager"></a>VM 在等待本地会话管理器时无响应

本文提供了一些步骤，用于解决在启动 Azure 虚拟机 (VM) 的情况下，来宾操作系统停滞等待本地会话管理器完成处理的问题。

## <a name="symptoms"></a>症状

使用 " [启动诊断](./boot-diagnostics.md) " 查看 VM 的屏幕截图时，你会看到屏幕截图显示消息 " *请等待本地会话管理器* " 的提示

![屏幕截图显示来宾操作系统停滞为 Windows Server 2012 R2 中的消息 "请等待本地会话管理器"。](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>原因

虚拟机停滞在等待本地会话管理器的原因有多种。 如果等待本地会话管理器是一个持久问题，则需要收集内存转储以进行分析。

## <a name="solution"></a>解决方案

在某些情况下，只需等待足够长的时间来完成该过程即可解决问题。 如果 VM 在等待屏幕上的等待时间超过一小时，则应收集内存转储，然后联系 Microsoft 支持部门。

### <a name="collect-the-memory-dump-file"></a>收集内存转储文件

若要解决此问题，首先需要收集崩溃的内存转储文件，然后与内存转储文件联系支持人员。 若要收集转储文件，请执行以下步骤：

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>将 OS 磁盘附加到新的修复 VM

1. 按照 [VM 修复命令的步骤 1-3](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 来准备一个修复 VM。
2. 使用远程桌面连接来连接到修复 VM。

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>找到转储文件并提交支持票证

1. 在修复 VM 上，转到附加的 OS 磁盘中的 Windows 文件夹。 如果分配给附加 OS 磁盘的驱动器号标记为 F，则需转到 `F:\Windows`。
2. 找到 memory.dmp 文件，然后使用该内存转储文件[提交支持票证](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)  。
3. 如果在查找 **内存 dmp** 文件时遇到问题，请按照指南 [使用不可屏蔽中断生成故障转储文件 (NMI) 调用](/windows/client-management/generate-kernel-or-complete-crash-dump)。

有关 NMI 调用的详细信息，请参阅 [串行控制台用户指南中 (NMI) 调用的不可屏蔽中断](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [排查 Azure 虚拟机启动错误](boot-error-troubleshoot.md)