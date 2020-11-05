---
title: 比较 Azure 存储队列和服务总线队列
description: 分析 Azure 提供的两种队列类型之间的差异和相似性。
ms.topic: article
ms.date: 11/04/2020
ms.openlocfilehash: 5c65cf5ef2d572417ea70d0e0259cf2c03ab590e
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93379564"
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>存储队列和服务总线队列 - 比较与对照
本文分析 Microsoft Azure 提供的两种队列类型之间的差异和相似性：存储队列和服务总线队列。 通过使用此信息，你可以更明智地决定哪种解决方案最符合你的需求。

## <a name="introduction"></a>简介
Azure 支持两种队列机制：“存储队列”和“服务总线队列” 。

**存储队列** 是 [Azure 存储](https://azure.microsoft.com/services/storage/) 基础结构的一部分。 它们允许存储大量消息。 可以使用 HTTP 或 HTTPS 通过经验证的调用从世界任何位置访问消息。 队列消息大小最大可为 64 KB。 一个队列可以包含数百万条消息，直至达到存储帐户的总容量限值。 队列通常用于创建要异步处理的积压工作 (backlog)。 有关详细信息，请参阅 [什么是 Azure 存储队列](../storage/queues/storage-queues-introduction.md)。

**服务总线队列** 是更广的 [Azure 消息传送](https://azure.microsoft.com/services/service-bus/) 基础结构的一部分，它支持队列、发布/订阅和更高级的集成模式。 它们旨在集成可能跨多个通信协议、数据约定、信任域或网络环境的应用程序或应用程序组件。 有关服务总线队列/主题/订阅的详细信息，请参阅 [服务总线队列、主题和订阅](service-bus-queues-topics-subscriptions.md)。


## <a name="technology-selection-considerations"></a>技术选择注意事项
存储队列和服务总线队列具有略微不同的功能集。 你可以选择一个或全部，具体取决于你的特定解决方案的需求。

在确定哪种队列技术适合给定的解决方案时，解决方案架构师和开发人员应考虑这些建议。 

### <a name="consider-using-storage-queues"></a>考虑使用存储队列
作为解决方案架构师/开发人员，在以下情况下， **应考虑使用存储队列** ：

* 应用程序必须在队列中存储超过 80 gb 的消息。
* 您的应用程序需要跟踪队列中消息的处理进度。 如果处理消息的工作线程发生崩溃，此方法非常有用。 然后，另一个工作人员可以使用该信息从之前的工作线程停止处继续。
* 你需要针对队列执行的所有事务的服务器端日志。

### <a name="consider-using-service-bus-queues"></a>考虑使用服务总线队列
作为解决方案架构师/开发人员，在以下情况下， **应考虑使用服务总线队列** ：

* 你的解决方案需要在无需轮询队列的情况下接收消息。 使用服务总线，可以通过使用服务总线支持的基于 TCP 的协议的长轮询接收操作来实现此目的。
* 解决方案要求队列必须遵循先入先出 (FIFO) 的传递顺序。
* 解决方案需要支持自动重复检测。
* 希望应用程序将消息作为长时间运行的并行流进行处理（使用消息的 [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid) 属性，将消息与流相关联）。 在这种模式下，消费应用程序中的每个节点会竞争流而不是消息。 当流被提供给某个消费节点时，该节点可以使用事务检查应用程序流的状态。
* 解决方案在发送或接收来自队列的多个消息时，需要事务行为和原子性。
* 您的应用程序处理的消息可能超过 64 KB，但不太可能接近 256 KB。
* 需要向队列提供基于角色的访问模型，为发送方和接收方提供不同权限。 有关详细信息，请参阅以下文章：
    - [使用托管标识进行身份验证](service-bus-managed-service-identity.md)
    - [从应用程序进行身份验证](authenticate-application.md)
* 队列大小不会增长到 80 GB。
* 希望使用基于 AMQP 1.0 标准的消息传送协议。 有关 AMQP 的详细信息，请参阅[服务总线 AMQP 概述](service-bus-amqp-overview.md)。
* 构思从基于队列的点到点通信到发布-订阅消息传送模式的最终迁移。 此模式可将其他接收方)  (订阅服务器集成。 每个接收方都接收发送到该队列的某些消息或所有消息的独立副本。 
* 消息传送解决方案需要支持 "最多一次" 传递保证，而无需构建其他基础结构组件。
* 解决方案需要发布并使用消息批处理。

## <a name="compare-storage-queues-and-service-bus-queues"></a>比较存储队列和服务总线队列
以下部分中的表格提供队列功能的逻辑分组。 它们使你可以一目了然地比较 Azure 存储队列和服务总线队列中可用的功能。

## <a name="foundational-capabilities"></a>基础功能
本部分比较存储队列和服务总线队列提供的一些基础队列功能。

| 比较条件 | 存储队列 | 服务总线队列 |
| --- | --- | --- |
| 排序保障 |**否** <br/><br>有关详细信息，请参阅 " [其他信息](#additional-information) " 部分中的第一个注意事项。</br> | **是 - 先进先出 (FIFO)**<br/><br>通过使用 [消息会话](message-sessions.md) ()  |
| 传递保障 |**至少一次** |使用 PeekLock 接收模式进行 **至少一次** (。 这是默认)  <br/><br/>**最多一次** （使用 ReceiveAndDelete 接收模式） <br/> <br/> 详细了解各种[接收模式](service-bus-queues-topics-subscriptions.md#receive-modes)  |
| 原子操作支持 |**否** |**是**<br/><br/> |
| 接收行为 |**非阻止**<br/><br/>（如果没有发现新消息，则立即完成） |**使用或不使用超时进行阻止**<br/><br/>（提供长轮询，或[“Comet 技术”](https://go.microsoft.com/fwlink/?LinkId=613759)）<br/><br/>**非阻止**<br/><br/>（通过仅使用 .NET 托管的 API） |
| 推送样式 API |**否** |**是**<br/><br/>[QueueClient.OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) 和 [MessageSessionHandler.OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagesessionhandler.onmessage#Microsoft_ServiceBus_Messaging_MessageSessionHandler_OnMessage_Microsoft_ServiceBus_Messaging_MessageSession_Microsoft_ServiceBus_Messaging_BrokeredMessage__) 会话 .NET API。 |
| 接收模式 |**扫视与租赁** |**扫视与锁定**<br/><br/>**接收并删除** |
| 独占访问模式 |**基于租赁** |**基于锁定** |
| 租赁/锁定持续时间 |**30 秒（默认值）**<br/><br/>**7 天（最大值）** （可使用 [UpdateMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.updatemessage) API 续订或释放消息租赁。） |**60 秒（默认值）**<br/><br/>可使用 [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API 续订消息锁。 |
| 租赁/锁定精度 |**消息级别**<br/><br/>每条消息可以具有不同的超时值，你可以在处理消息时根据需要使用 [UpdateMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.updatemessage) API 来更新超时值。 |**队列级别**<br/><br/>（每个队列都具有一个适用于其中所有消息的锁定精度，但是可使用 [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API 续订该锁。） |
| 成批接收 |**是**<br/><br/>（在检索消息时显式指定消息计数，最多可达 32 条消息） |**是**<br/><br/>（隐式启用预提取属性或通过使用事务显式启用） |
| 成批发送 |**否** |**是**<br/><br/>（通过使用事务或客户端批处理） |

### <a name="additional-information"></a>其他信息
* 存储队列中的消息通常是先进先出的，但有时它们可能会有序。 例如，当消息的可见性超时持续时间到期，因为客户端应用程序在处理消息时崩溃。 当可见性超时到期时，消息会再次变成在队列上可见，让另一个工作进程能够取消它的排队。 此时，可能会在队列中放入新的可见消息，使其再次取消排队。
* 服务总线队列中有保障的 FIFO 模式要求使用消息传送会话。 如果应用程序在处理 " **速览 & 锁定** " 模式下收到的消息时出现故障，则在下一次队列接收方接受消息传送会话时，它将在消息的生存时间 (TTL) 期限到期后从失败消息开始。
* 存储队列旨在支持标准队列方案，例如以下方案：
    - 分离应用程序组件以提高故障的可伸缩性和容错能力
    - 负载分级
    - 生成过程工作流。
* 可以避免在服务总线会话上下文中处理消息时出现的不一致，方法是：使用会话状态来存储应用程序的状态（与处理会话的消息序列的进程相关），以及使用与处理接收的消息和更新会话状态相关的事务。 在其他供应商的产品中处理此类一致性功能时，有时会将其标记为 *一次* 。 任何事务故障都将明显导致消息被重新传送，这就是该术语无法完全满足的原因。
* 存储队列可在多个队列、表和 Blob 上提供统一和一致的编程模型 – 对于开发人员和运营团队都是如此。
* 服务总线队列为单个队列的上下文中的本地事务提供支持。
* 服务总线支持的“接收与删除”模式提供了减少消息传送操作计数（和相关成本）以换取降低安全传递保证的能力。
* 存储队列提供租赁且可延长消息租赁时间。 此功能允许工作进程对消息保持短的租用。 因此，如果某个工作线程崩溃，则其他工作线程可以再次快速处理该消息。 此外，如果工作线程需要处理的时间比当前租约时间长，则工作线程可以延长该消息的租约。
* 存储队列提供了可见性超时，可针对消息入队或出队进行设置。 此外，还可以在运行时更新具有不同租约值的消息，并跨同一队列中的消息更新不同值。 服务总线锁定超时在队列元数据中定义。 但是，可以通过调用 [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) 方法来续订该锁。
* 服务总线队列中阻塞接收操作的最大超时值为 24 天。 但是，基于 REST 的最大超时值为 55 秒。
* 服务总线提供的客户端批处理允许队列客户端将多个消息纳入一个发送操作中进行批处理。 批处理仅适用于异步发送操作。
* 当你虚拟化) 帐户时，如 200-TB 的存储队列上限 (更多的功能，而不限制队列则使其成为 SaaS 提供商的理想平台。
* 存储队列提供灵活且性能良好的委托访问控制机制。

## <a name="advanced-capabilities"></a>高级功能
本部分对存储队列和服务总线队列提供的高级功能进行了比较。

| 比较条件 | 存储队列 | 服务总线队列 |
| --- | --- | --- |
| 计划的传递 |**是** |**是** |
| 自动死信 |**否** |**是** |
| 提高队列生存时间值 |**是**<br/><br/>（通过就地更新可见性超时） |**是**<br/><br/>（通过一个专用 API 功能提供） |
| 有害消息支持 |**是** |**是** |
| 就地更新 |**是** |**是** |
| 服务器端事务日志 |**是** |**否** |
| 存储度量值 |**是**<br/><br/>**分钟度量值** 为可用性、TPS、API 调用计数、错误计数等提供实时度量值。 它们都是实时的，每分钟聚合一次，并在几分钟内与生产中发生的时间报告。 有关详细信息，请参阅[关于存储分析度量值](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics)。 |**是**<br/><br/>（通过调用 [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues) 进行大容量查询） |
| 状态管理 |**否** |**是**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus)、[Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus)、[Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus)、[Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus) |
| 消息自动转发 |**否** |**是** |
| 清除队列函数 |**是** |**否** |
| 消息组 |**否** |**是**<br/><br/>（通过使用消息传送会话） |
| 每个消息组的应用程序状态 |**否** |**是** |
| 重复检测 |**否** |**是**<br/><br/>（可在发送端配置） |
| 浏览消息组 |**否** |**是** |
| 按 ID 提取消息会话 |**否** |**是** |

### <a name="additional-information"></a>其他信息
* 两种队列技术都允许将消息安排在以后传送。
* Queue 自动转发使上千个队列可以将其消息 autoforward 到一个队列中，接收方应用程序从该队列中使用该消息。 可以使用此机制来实现安全性和控制流，并且隔离每个消息发布者的存储。
* 存储队列为更新消息内容提供支持。 可以使用此功能将状态信息和递增进度更新持久保留到消息中，以便能够从上一个已知的检查点处理该消息，而不是从头开始。 借助服务总线队列，可以通过使用消息会话实现相同的方案。 会话允许保存和检索应用程序处理状态（通过使用 [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) 和 [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)）。
* 服务总线队列支持 [死信](service-bus-dead-letter-queues.md)。 这对于隔离满足以下条件的消息可能很有用：
    - 接收应用程序无法成功处理消息 
    - 消息无法到达其目标，因为生存时间 (TTL) 属性过期。 TTL 值指定消息在队列中保留的时间。 对于服务总线，在 TTL 期限过期时，该消息将移到一个特殊的队列（称为 $DeadLetterQueue）。
* 为了在存储队列中查找“病毒”消息，在将某个消息取消排队时，应用程序将检查该消息的 [DequeueCount](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage.dequeuecount) 属性。 如果 **DequeueCount** 大于给定的阈值，应用程序会将消息移到应用程序定义的“死信”队列。
* 利用存储队列，你可以获取针对该队列执行的所有事务的详细日志以及聚合度量值。 这两个选项可用于调试和了解应用程序如何使用存储队列。 它们还可用于对应用程序进行性能优化并降低使用队列的成本。
* 服务总线支持的消息会话允许属于逻辑组的消息与接收方关联。 它在消息与其各自的接收方之间创建类似于会话的关联。 可以通过对消息设置 [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) 属性，在服务总线中启用此高级功能。 然后，接收者可以侦听特定会话 ID，并接收共享特定会话标识符的消息。
* 根据 [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) 属性的值，服务总线队列的重复检测功能会自动删除发送到队列或主题的重复消息。

## <a name="capacity-and-quotas"></a>容量和配额
本节从适用的[容量和配额](service-bus-quotas.md)角度比较存储队列和服务总线队列。

| 比较条件 | 存储队列 | 服务总线队列 |
| --- | --- | --- |
| 最大队列大小 |**500 TB**<br/><br/>（限制为[单个存储帐户容量](../storage/common/storage-introduction.md#queue-storage)） |**1 GB 到 80 GB**<br/><br/>（在创建队列和[启用分区](service-bus-partitioning.md)时定义 – 请参阅“其他信息”部分） |
| 最大消息大小 |**64 KB**<br/><br/>（使用 **Base64** 编码时为 48 KB）<br/><br/>Azure 可以通过合并队列和 Blob 支持大消息 – 单个项目排队的消息最多达到 200 GB。 |**256 KB** 或 **1 MB**<br/><br/>（包含标头和正文，最大标头大小：64 KB）。<br/><br/>取决于[服务层级](service-bus-premium-messaging.md)。 |
| 最大消息 TTL |**无限** (api 版本2017-07-27 或更高版本)  |**TimeSpan.Max** |
| 最大队列数 |**不受限制** |**10,000**<br/><br/>（按服务命名空间） |
| 并发客户端的最大数目 |**无限制** |**无限制**<br/><br/>（100 个并发连接限制仅适用于基于 TCP 协议的通信） |

### <a name="additional-information"></a>其他信息
* 服务总线强制实施队列大小限制。 在创建队列时指定最大队列大小。 它可以介于 1 GB 到 80 GB 之间。 如果队列的大小达到此限制，则将拒绝其他传入消息，并且调用方会收到一个异常。 有关服务总线中配额的详细信息，请参阅[服务总线配额](service-bus-quotas.md)。
* [高级层](service-bus-premium-messaging.md)不支持分区。 在标准消息传送层中，你可以在 1 (默认) 、2、3、4或 5 GB 大小中创建服务总线队列和主题。 启用分区后，服务总线将创建实体的 16 个副本（16 个分区），每个副本具有指定的相同大小。 因此，如果你创建了一个大小为 5 GB 的队列，共有 16 个分区，最大队列大小为 (5 * 16) = 80 GB。  可以在 [Azure 门户][Azure portal]中查看分区队列或主题的最大大小。
* 在存储队列中，如果消息的内容不是 XML 安全的，则必须对其进行 **Base64** 编码。 如果使用 **Base64** 编码此消息，则用户负载可高达 48 KB，而不是 64 KB。
* 对于服务总线队列，存储在队列中的每条消息由两个部分组成：标头和正文。 消息的总大小不能超过服务层支持的最大消息大小。
* 当客户端通过 TCP 协议与服务总线队列进行通信时，到单个服务总线队列的最大并发连接数不得超过 100。 此数值在发送方和接收方之间共享。 如果达到此配额，则将拒绝对其他连接的请求，并且调用代码将收到异常。 此限制不适用于使用基于 REST 的 API 连接到队列的客户端。
* 如果在单个服务总线服务命名空间中需要超过 10,000 个队列，可以联系 Azure 支持团队并请求增加数目。 若要使用服务总线扩展到 10,000 个以上的队列，还可使用 [Azure 门户][Azure portal]创建其他服务命名空间。

## <a name="management-and-operations"></a>管理和操作
本部分对存储队列和服务总线队列提供的管理功能进行了比较。

| 比较条件 | 存储队列 | 服务总线队列 |
| --- | --- | --- |
| 管理协议 |**基于 HTTP/HTTPS 的 REST** |**基于 HTTPS 的 REST** |
| 运行时协议 |**基于 HTTP/HTTPS 的 REST** |**基于 HTTPS 的 REST**<br/><br/>**AMQP 1.0 标准（具有 TLS 的 TCP）** |
| .NET API |**是**<br/><br/>（.NET 存储客户端 API） |**是**<br/><br/>（.NET 服务总线 API） |
| 本机 C++ |**是** |**是** |
| Java API |**是** |**是** |
| PHP API |**是** |**是** |
| Node.js API |**是** |**是** |
| 任意元数据支持 |**是** |**否** |
| 队列命名规则 |**长度最多为 63 个字符**<br/><br/>（队列名称中的字母必须小写。） |**长度最多为 260 个字符**<br/><br/>（队列路径和名称不区分大小写。） |
| 获取队列长度函数 |**是**<br/><br/>（在消息超出 TTL 但未删除的情况下的近似值。） |**是**<br/><br/>（精确时间点值。） |
| Peek 函数 |**是** |**是** |

### <a name="additional-information"></a>其他信息
* 存储队列为可应用于队列说明的任意属性提供支持（以名称/值对形式）。
* 两种队列技术还提供无需锁定消息即可进行消息扫视的功能，这在实现队列资源管理器/浏览器工具时可能非常有用。
* Service Bus .NET 中转消息传递 Api 使用全双工 TCP 连接，以便在与 HTTP 上的 REST 相比提高性能，并支持 AMQP 1.0 标准协议。
* 存储队列名称长度可以在 3-63 个字符之间，可以包含小写字母、数字和连字符。 有关详细信息，请参阅 [命名队列和元数据](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata)。
* 服务总线队列名称长度最大可达 260 个字符，且命名规则限制较少。 服务总线队列名称可以包含字母、数字、句点、连字符和下划线。

## <a name="authentication-and-authorization"></a>身份验证和授权
本部分讨论存储队列和服务总线队列支持的身份验证和授权功能。

| 比较条件 | 存储队列 | 服务总线队列 |
| --- | --- | --- |
| 身份验证 |**对称密钥** |**对称密钥** |
| 安全模型 |通过 SAS 令牌进行的委托访问。 |SAS |
| 标识提供者联合 |**否** |**是** |

### <a name="additional-information"></a>其他信息
* 针对任一队列技术的每个请求都必须进行身份验证。 不支持使用匿名访问的公共队列。 使用 [SAS](service-bus-sas.md)，可以通过发布只写 SAS、只读 SAS 甚至完全访问权限 SAS 来满足这种方案的需求。
* 存储队列提供的身份验证方案涉及使用对称密钥。 此密钥是基于哈希的消息验证代码 (HMAC) ，使用 SHA-256 算法计算并编码为 **Base64** 字符串。 有关相应协议的详细信息，请参阅 [Authentication for the Azure Storage Services](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services)（Azure 存储服务的身份验证）。 服务总线队列支持使用对称密钥的类似模型。 有关详细信息，请参阅 [使用服务总线进行共享访问签名身份验证](service-bus-sas.md)。

## <a name="conclusion"></a>结论
通过更深入地了解这两种技术，你可以对使用哪种队列技术以及何时使用做出更明智的决策。 决定何时使用存储队列或服务总线队列时，显然要考虑很多因素。 这些因素很大程度上取决于应用程序及其体系结构的独特需要。 

您可能更倾向于选择存储队列，原因如下：

- 如果你的应用程序已使用 Microsoft Azure 的核心功能 
- 如果在服务之间需要基本通信和消息传送 
- 需要的队列大小可以大于 80 GB

服务总线队列提供许多高级功能，例如以下。 如果你正在构建混合应用程序，或者如果你的应用程序需要这些功能，则它们可能是首选选项。

- [会话](message-sessions.md)
- [事务](service-bus-transactions.md)
- [重复检测](duplicate-detection.md)
- [自动死信](service-bus-dead-letter-queues.md)
- [持久发布和订阅功能](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) 



## <a name="next-steps"></a>后续步骤
以下文章提供了有关使用存储队列或服务总线队列的更多指导和信息。

* [服务总线队列入门](service-bus-dotnet-get-started-with-queues.md)
* [如何使用队列存储服务](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [使用服务总线中转消息传送改进性能的最佳实践](service-bus-performance-improvements.md)
* [Introducing Queues and Topics in Azure Service Bus (blog post)](https://www.serverless360.com/blog/azure-service-bus-queues-vs-topics)（Azure 服务总线中的队列和主题简介 – 博客文章）
* [服务总线开发人员指南](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [在 Azure 中使用队列服务](https://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

