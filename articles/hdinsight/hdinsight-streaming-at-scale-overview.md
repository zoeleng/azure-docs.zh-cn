---
title: Azure HDInsight 中的大规模流式处理
description: 如何在 Azure HDInsight 中通过可缩放的 Apache 群集使用数据流。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: a04ce77c7e81a3a73b87eaf5790b383dece35d86
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535223"
---
# <a name="streaming-at-scale-in-hdinsight"></a>HDInsight 中的大规模流式处理

实时大数据解决方案对正在进行的数据进行操作。 通常，这些数据在抵达时的作用最大。 如果传入的数据流比当时可处理的大小更大，则可能需要限制资源。 或者，可以按需添加节点，根据流式处理解决方案的需要纵向扩展 HDInsight 群集。

在流式处理应用程序中，一个或多个数据源会生成事件（有时达到每秒几百万个事件），此时，需要在不丢弃任何有用信息的情况下快速引入这些事件。 [Apache Kafka](kafka/apache-kafka-introduction.md) 或[事件中心](https://azure.microsoft.com/services/event-hubs/)等服务使用流缓冲（也称为事件队列）来处理传入的事件。  收集事件后，可以使用流处理层中的实时分析系统（例如 [Apache Storm](storm/apache-storm-overview.md) 或 [Apache Spark Streaming](spark/apache-spark-streaming-overview.md)）来分析数据。 处理的数据可存储在长期存储系统（例如 [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)）中，并实时显示在商业智能仪表板（例如 [Power BI](https://powerbi.microsoft.com)、Tableau）或自定义的网页上。

![Azure HDInsight 流式处理模式](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka 提供高吞吐量、低延迟的消息队列服务，并且现已包含在 Apache 开源软件 (OSS) 套件中。 Kafka 使用发布-订阅消息传送模型，将分区数据流安全存储在分布式的复制群集中。 吞吐量增大时，Kafka 会线性扩展。

有关详细信息，请参阅 [HDInsight 上的 Apache Kafka 简介](kafka/apache-kafka-introduction.md)。

## <a name="apache-storm"></a>Apache Storm

Apache Storm 是经过优化的分布式容错开源计算系统，可以配合 Hadoop 实时处理数据流。 传入事件的核心数据单元是元组 - 不可变的键/值对集。 这些元组的无限制序列构成了源自 Spout 的流。 Spout 包装流式处理数据源（例如 Kafka），并发出元组。 Storm 拓扑是这些流的转换序列。

有关详细信息，请参阅[什么是 Azure HDInsight 上的 Apache Storm？](storm/apache-storm-overview.md)。

## <a name="spark-streaming"></a>Spark Streaming

Spark Streaming 是 Spark 的一个扩展，可让你重复使用执行批处理时所用的相同代码。 可以在同一个应用程序中结合使用批处理和交互式查询。 与风暴不同，Spark 流式处理只提供一次处理语义的状态。 与 [Kafka 直接 API](https://spark.apache.org/docs/latest/streaming-kafka-integration.html)结合使用时，这可确保 Spark 流式处理只接收一次 Kafka 的所有数据，可以完全实现一次端到端保证。 Spark Streaming 的优点之一体现在其容错功能。在群集中使用多个节点时，它可以快速恢复有故障的节点。

有关详细信息，请参阅[什么是 Apache Spark Streaming？](./spark/apache-spark-streaming-overview.md)。

## <a name="scaling-a-cluster"></a>缩放群集

尽管可以在创建过程中指定群集中的节点数，但可能需要扩展或缩减群集才能匹配工作负荷。 所有 HDInsight 群集允许[更改群集中的节点数](hdinsight-administer-use-portal-linux.md#scale-clusters)。 由于所有数据都存储在 Azure 存储或 Data Lake Storage 中，因此可以在不丢失数据的情况下删除 Spark 群集。

分离技术可以带来优势。 例如，Kafka 是一种事件缓冲技术，因此它非常耗费 IO，并且不需要处理能力大。 相比之下，Spark Streaming 等流处理器是计算密集型的，需要更强大的 VM。 将这些技术分离到不同的群集后，可以单独缩放每个群集，同时充分利用 VM。

### <a name="scale-the-stream-buffering-layer"></a>缩放流缓冲层

事件中心和 Kafka 流缓冲技术都使用分区，使用者从这些分区读取数据。 缩放输入吞吐量需要增大分区数目，而添加分区又能提供更高的并行度。 在事件中心中，部署后无法更改分区计数，因此请务必从目标规模开始。 通过 Kafka，可以 [添加分区](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion)，即使 Kafka 正在处理数据时也是如此。 Kafka 提供 `kafka-reassign-partitions.sh` 工具用于重新分配分区。 HDInsight 提供[分区副本重新均衡工具](https://github.com/hdinsight/hdinsight-kafka-tools)`rebalance_rackaware.py`。 此重新均衡工具在调用 `kafka-reassign-partitions.sh` 工具时，可让每个副本位于不同的容错域和更新域，以便可以识别 Kafka 机架，并提高容错能力。

### <a name="scale-the-stream-processing-layer"></a>缩放流处理层

Apache Storm 和 Spark Streaming 都支持将工作节点添加到其群集，即使正在处理数据。

若要利用通过缩放 Storm 添加的新节点，需要重新均衡在增加群集大小之前启动的任何 Storm 拓扑。 可以使用 Storm Web UI 或其 CLI 实现这种重新均衡。 有关详细信息，请参阅 [Apache Storm 文档](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)。

Apache Spark 根据应用程序的要求使用三个关键参数来配置其环境：`spark.executor.instances`、`spark.executor.cores` 和 `spark.executor.memory`。 执行器是针对 Spark 应用程序启动的进程。  执行器在工作节点上运行，负责执行应用程序的任务。 执行器的默认数目和每个群集的执行器大小是根据工作节点数目和工作节点大小计算的。 这些数字存储在每个群集头节点上的 `spark-defaults.conf` 文件中。

这三个参数可在群集级别进行配置（适用于群集上运行的所有应用程序），也可以针对每个应用程序指定。 有关详细信息，请参阅[管理 Apache Spark 群集的资源](spark/apache-spark-resource-manager.md)。

## <a name="next-steps"></a>后续步骤

* [在 Azure HDInsight 中创建和监视 Apache Storm 拓扑](storm/apache-storm-quickstart.md)
* [HDInsight 上的 Apache Storm 的示例拓扑](storm/apache-storm-example-topology.md)
* [HDInsight 上的 Apache Spark 简介](spark/apache-spark-overview.md)
* [HDInsight 上的 Apache Kafka 入门](kafka/apache-kafka-get-started.md)