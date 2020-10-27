---
title: VMware HCX 网段
description: VMware HCX 需要四个网络。
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 8137b4383d2a243d53db317db6f5a78b3bc68e67
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173630"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

VMware HCX 需要四个网络：

- **管理网络：** 通常，它与 vSphere 群集上使用的管理网络相同。 至少在此网段上为 VMware HCX 标识两个 IP。 （根据你的部署，可能需要更多数量。）

- **vMotion 网络：** 通常，它与 vSphere 群集上用于 vMotion 的网络相同。  至少在此网段上为 VMware HCX 标识两个 IP。 （根据你的部署，可能需要更多数量。）  

   必须在分布式虚拟交换机或 vSwitch0 上公开 vMotion 网络， 否则请修改环境。

   > [!NOTE]
   > 如果此网络未路由（专用），则没有问题。

- **上行网络：** 需要为 VMware HCX 上行创建一个新网络，并通过端口组将其扩展到 vSphere 群集。 至少在此网段上为 VMware HCX 标识两个 IP。 （根据你的部署，可能需要更多数量。）  

   > [!NOTE]
   > 推荐的方法是创建一个 /29 网络，但是任何网络大小都可以。

- **复制网络：** 需要为 VMware HCX 复制创建一个新网络，并通过端口组将该网络扩展到 vSphere 群集。 至少在此网段上为 VMware HCX 标识两个 IP。 （根据你的部署，可能需要更多数量。）

   > [!NOTE]
   > 推荐的方法是创建一个 /29 网络，但是任何网络大小都可以。