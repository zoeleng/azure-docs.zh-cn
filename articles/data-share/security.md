---
title: Azure Data Share 的安全性概述
description: Azure Data Share 的安全性概述
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 1fdf026e9271ef6eb30c2b4ca96a04880b65be75
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578087"
---
# <a name="security-overview-for-azure-data-share"></a>Azure Data Share 的安全性概述

本文提供 Azure 数据共享服务的安全性概述。

## <a name="security-overview"></a>安全概述

Azure Data Share 利用 Azure 提供的基础安全措施来保护静态数据和传输中的数据。 将会对静态数据加密，通过基础数据存储提供支持。 还会使用 TLS 1.2 在传输过程中加密数据。 此外还会对静态的和传输中的数据共享元数据加密。 Azure 数据共享不存储正在共享的客户数据的内容。

Azure 数据共享利用之前称为 MSI) 的托管标识来访问用于数据共享的数据存储 (。 在数据提供者和数据使用者之间没有凭据交换。 有关托管标识的详细信息，请参阅 [Azure 资源的托管标识](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)。 有关共享数据所需的角色和权限的详细信息，请参阅 [角色和要求](concepts-roles-permissions.md)。

可以在数据共享资源级别设置对 Azure 数据共享的访问控制，以确保其被授权的访问。 

## <a name="share-data-from-or-to-data-stores-with-firewall-enabled"></a>启用防火墙时，将数据从或共享到数据存储
若要在打开防火墙的情况下将数据从或共享到存储帐户，需要在存储帐户中启用 " **允许受信任的 Microsoft 服务** "。 有关详细信息，请参阅 [配置 Azure 存储防火墙和虚拟网络](
https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) 。


## <a name="next-steps"></a>后续步骤

若要了解如何开始共享数据，请继续阅读[共享数据](share-your-data.md)教程。
