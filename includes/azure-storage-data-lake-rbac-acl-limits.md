---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 8b793a28c31df185555b9a548df52e4beeccf305
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131626"
---
| 机制 | 范围 |限制 | 支持的权限级别 |
|---|---|---|---|
| RBAC | 存储帐户和容器。 <br>在订阅或资源组级别跨资源 RBAC 角色分配。 | 2000订阅中的 RBAC 角色分配 | RBAC 角色 (内置或自定义)  |
| ACL| 目录、文件 |32 ACL 条目 (每个文件和每个目录) 有效地28个 ACL 条目。 访问和默认 Acl 每个都有其各自的 32 ACL 项限制。 |ACL 权限|
