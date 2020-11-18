---
title: 教程：Azure Synapse Analytics 入门 - 监视 Synapse 工作区
description: 在本教程中，你将了解如何监视 Synapse 工作区中的活动。
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: monitoring
ms.topic: tutorial
ms.date: 10/15/2020
ms.openlocfilehash: 2970bb58bb73d52c75729b00a8209a9c576d4ec0
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427405"
---
# <a name="monitor-your-synapse-workspace"></a>监视 Synapse 工作区

在本教程中，你将了解如何监视 Synapse 工作区中的活动。 你可以监视 SQL、Apache Spark 和 Pipelines 的当前和历史活动。 

## <a name="introduction-to-the-monitor-hub"></a>监视中心简介

打开 Synapse Studio 并导航到“监视”中心。 在此处，你可以查看工作区中所有活动的历史记录，以及目前处于有效状态的活动。 

* 在“集成”下，可监视管道、触发器和集成运行时
* 在“活动”下，可监视 Spark 和 SQL 活动。 

## <a name="integration"></a>集成

1. 导航到“集成”>“管道”。 在此视图中，你可以查看每次管道在工作区中运行时的状况。 
1. 找到在先前步骤中运行的管道，然后单击其管道名称以查看详细信息。
1. 单击 Synapse Studio 顶部附近的“痕迹导航栏”，再单击“所有管道运行”以返回到先前视图 。

## <a name="apache-spark-activities"></a>Apache Spark 活动

1. 导航到“集成”>“活动”>“Apache Spark 应用程序”。 现在，你可以查看正在工作区中运行或已经在工作区中运行的所有 Spark 应用程序。
1. 找到一个已不再运行的应用程序，然后单击其应用程序名称。 现在，你可以查看 Spark 应用程序的详细信息。
1. 如果你熟悉 Apache Spark，则可以通过单击 Spark 历史记录服务器来找到标准 Apache Spark 历史记录服务器 UI。

## <a name="sql-activities"></a>SQL 活动

1. 导航到“集成”>“活动”>“SQL 请求”。
1. 在此视图中，你可以查看 SQL 请求。
1. 选择要监视的池。 现在，你可以查看正在池的工作区中运行或已经在池的工作区中运行的所有 SQL 请求。
1. 查找特定的 SQL 请求，并将鼠标悬停在该项上。 悬停后，将看到“SQL 脚本”图标出现。
1. 单击“SQL 脚本”图标以查看 SQL 请求全文。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [探索知识中心](get-started-knowledge-center.md)
