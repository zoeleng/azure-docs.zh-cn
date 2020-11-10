---
title: include 文件
description: include 文件
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 10177dd949ac531027e13cf633b11c16674fd4ab
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94386599"
---
在预览版中，NFS 具有以下限制：

- NFS 4.1 当前仅支持 [协议规范](https://tools.ietf.org/html/rfc5661)中的必需功能。 不支持其他功能，例如委托和所有类型的回调、锁定升级和降级以及 Kerberos 身份验证和加密。
- 如果大多数请求都以元数据为中心，则与读取/写入/更新操作相比，延迟时间将更差。
- 必须创建新的存储帐户，才能创建 NFS 共享。
- 仅支持管理平面 REST Api。 数据平面 REST Api 不可用，这意味着存储资源管理器之类的工具将无法与 NFS 共享一起使用，也不能浏览 Azure 门户中的 NFS 共享数据。
- 仅适用于高级层。
- 目前仅适用于本地冗余存储 (LRS) 。

### <a name="azure-storage-features-not-yet-supported"></a>尚不支持 Azure 存储功能

此外，以下 Azure 文件功能不可用于 NFS 共享：

- 基于标识的身份验证
- Azure 备份支持
- 快照
- 软删除
- 完全加密传输支持 (详细信息，请参阅 [NFS 安全性](../articles/storage/files/storage-files-compare-protocols.md#security)) 
- Azure 文件同步 (仅适用于 NFS 4.1 不支持的 Windows 客户端) 
