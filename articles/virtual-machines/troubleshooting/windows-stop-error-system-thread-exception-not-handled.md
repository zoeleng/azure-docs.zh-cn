---
title: Windows stop 错误-未处理0x0000007E 系统线程异常
description: 本文提供了解决以下问题的步骤：来宾 OS 遇到问题并希望重启 Azure VM。 消息将声明 "系统线程异常未处理"。
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/04/2020
ms.author: v-mibufo
ms.openlocfilehash: dd18d69009b9c150c4c12a755e9060cd5dfaccae
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423953"
---
# <a name="windows-stop-error---0x0000007e-system-thread-exception-not-handled"></a>Windows stop 错误-未处理0x0000007E 系统线程异常

本文提供了解决以下问题的步骤：来宾 OS 遇到问题并希望重启 Azure VM。 消息将声明 "系统线程异常未处理"。

## <a name="symptoms"></a>症状

使用 " [启动诊断](./boot-diagnostics.md) " 查看 VM 的屏幕截图时，你会看到屏幕截图显示 Windows 需要使用 " **未处理** 的停止代码" 异常或错误代码 **0x0000007e** 来重新启动。

![屏幕截图显示在启动过程中 Windows 停滞，并显示以下消息： "你的电脑遇到问题，需要重新启动。 我们将重新启动。 " 停止代码： "系统线程异常未处理"](media/windows-stop-error-system-thread-exception-not-handled/windows-stop-error-system-thread-exception-not-handled-1.png)

## <a name="cause"></a>原因

在分析内存转储文件之前无法确定原因。 继续收集内存转储文件。

## <a name="solution"></a>解决方案

### <a name="collect-the-memory-dump-file"></a>收集内存转储文件

若要解决此问题，首先需要收集崩溃的内存转储文件，然后与内存转储文件联系支持人员。 若要收集转储文件，请执行以下步骤：

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>将 OS 磁盘附加到新的修复 VM

1. 按照 [VM 修复命令的步骤 1-3](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 来准备一个修复 VM。
2. 使用远程桌面连接来连接到修复 VM。

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>找到转储文件并提交支持票证

1. 在修复 VM 上，转到附加的 OS 磁盘中的 Windows 文件夹。 如果分配给附加 OS 磁盘的驱动器号标记为 F，则需转到 `F:\Windows`。
2. 找到 memory.dmp 文件，然后使用该内存转储文件[提交支持票证](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)  。
3. 如果在查找 **内存 dmp** 文件时遇到问题，请按照指南 [使用不可屏蔽中断生成故障转储文件 (NMI) 调用](/windows/client-management/generate-kernel-or-complete-crash-dump)。

有关 NMI 调用的详细信息，请参阅 [串行控制台用户指南中的非屏蔽中断 (NMI) 调用](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) 。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [排查 Azure 虚拟机启动错误](./boot-error-troubleshoot.md)