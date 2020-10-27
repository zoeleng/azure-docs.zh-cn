---
title: include 文件
description: include 文件
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 36c05badb3b2292a29b8227c7f03b841474c97ad
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548106"
---
可以使用 REST API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) 获取终结点的运行状况状态。 当终结点运行状况不佳或运行不正常时，建议使用与路由消息延迟相关的 [IoT 中心路由指标](../articles/iot-hub/monitor-iot-hub-reference.md#routing-metrics) 来识别和调试错误，因为当终结点处于这些状态之一时，会预计延迟较高。 若要了解有关使用 IoT 中心指标的详细信息，请参阅 [监视 Iot 中心](../articles/iot-hub/monitor-iot-hub.md)。

|运行状况状态|说明|
|---|---|
|healthy|终结点按预期方式接受消息。|
|不正常|终结点未接受消息，IoT 中心正重试将消息发送到此终结点。|
|未知|IoT 中心尚未尝试将消息传递到此终结点。|
|已降级|终结点正在接受比预期慢的消息或正在从不正常状态恢复。|
|不活动|IoT 中心不再向此终结点传递消息。 重新尝试将消息发送到此终结点时失败。|
