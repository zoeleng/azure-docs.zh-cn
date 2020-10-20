---
author: baanders
description: Azure 数字孪生安装程序中的步骤概述的包含文件
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 5ab6812d144122c61018ad740892db869a7ba43d
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205469"
---
>[!NOTE]
>这些操作旨在由有权管理 Azure 订阅上的资源和用户访问权限的用户完成。 尽管某些步骤可以使用较低权限完成，但需要具有这些权限的人员才能完全设置一个可用的实例。 有关详细信息，请查看下面的 [*先决条件： Required 权限*](#prerequisites-permission-requirements) 部分。

新的 Azure 数字孪生实例的完全安装包括两个部分：
1. **创建实例**
2. **设置用户访问权限**： azure 用户需要具有 Azure 数字 *孪生所有者 (预览 * azure 数字孪生实例上的) 角色，才能管理该实例及其数据。 在此步骤中，你作为 Azure 订阅的所有者/管理员，会将此角色分配给将管理 Azure 数字孪生实例的人员。 这可能是你自己或组织中的其他人。