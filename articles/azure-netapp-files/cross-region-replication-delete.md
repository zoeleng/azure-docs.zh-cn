---
title: 删除 Azure NetApp 文件跨区域复制的复制 |Microsoft Docs
description: 描述如何删除源卷和目标卷之间不再需要的复制连接。
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/17/2020
ms.author: b-juche
ms.openlocfilehash: e08b69271ba9d115c26418bc5e421ee6c94b031d
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695701"
---
# <a name="delete-replications"></a>删除副本

可以通过删除卷复制来终止源卷和目标卷之间的复制连接。 可以从源或目标卷执行删除操作。 删除操作仅删除复制的授权;它不会删除源或目标卷。 

## <a name="steps"></a>步骤

1. 删除卷复制之前，请确保复制对等互连已断开。    
    请参阅 [显示复制关系的运行状况状态](cross-region-replication-display-health-status.md) 和 [中断复制对等互连](cross-region-replication-manage-disaster-recovery.md#break-replication-peering-to-activate-the-destination-volume)。  

1. 若要删除卷复制，请从源或目标卷中选择 " **复制** "。  

2. 单击 **“删除”** 。    

3. 键入 Yes 并单击 **"** **删除**"，确认删除。   

    ![删除复制](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>后续步骤  

* [跨区域复制](cross-region-replication-introduction.md)
* [使用跨区域复制的要求和注意事项](cross-region-replication-requirements-considerations.md)
* [显示复制关系的运行状况](cross-region-replication-display-health-status.md)
* [跨区域复制故障排除](troubleshoot-cross-region-replication.md)

