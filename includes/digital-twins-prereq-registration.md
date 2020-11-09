---
author: baanders
description: Azure 数字孪生的 include 文件 - 设置应用注册的先决条件
ms.service: digital-twins
ms.topic: include
ms.date: 10/29/2020
ms.author: baanders
ms.openlocfilehash: 0dd97e109ee907e8eec2d31c7d469e8fb1e70e7b
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93124828"
---
若要对本文中使用的所有资源进行身份验证，需要设置一个 [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) 应用注册。 按照[如何：创建应用注册](../articles/digital-twins/how-to-create-app-registration.md)中的说明进行此设置。 

具有应用注册后，需要注册的“应用程序(客户端) ID”和“Directory (租户) ID”（[可在 Azure 门户中找到](../articles/digital-twins/how-to-create-app-registration.md#collect-client-id-and-tenant-id)） 。 请记下这些值，以便稍后用它们授予对 Azure 数字孪生 API 的访问权限。