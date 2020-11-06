---
title: 为专用 SQL 池创建用户定义的还原点
description: 如何创建专用 SQL 池的还原点。
services: synapse-analytics
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 10/29/2020
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c0835fb48d00fe5277732f34fd6b0de1448f6a78
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94332054"
---
# <a name="user-defined-restore-points"></a>用户定义的还原点

在本文中，你将学习如何使用 Azure 门户在 Azure Synapse Analytics 中为专用 SQL 池创建一个新的用户定义的还原点。

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>通过 Azure 门户创建用户定义的还原点

还可以通过 Azure 门户创建用户定义的还原点。

1. 登录到 [Azure 门户](https://portal.azure.com/)帐户。

2. 导航到要为其创建还原点的专用 SQL 池。

3. 从左窗格中选择 " **概述** "，选择 " **+ 新建还原点** "。 如果 "新还原点" 按钮未启用，请确保专用 SQL 池未暂停。

    ![新建还原点](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. 为用户定义的还原点指定名称，然后单击“应用”  。 用户定义的还原点的默认保留期为七天。

    ![还原点的名称](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>后续步骤

- [还原现有专用 SQL 池](restore-sql-pool.md)

