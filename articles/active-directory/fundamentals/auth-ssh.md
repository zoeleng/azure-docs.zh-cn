---
title: Azure Active Directory 的 SSH 身份验证
description: 有关如何实现与 Azure Active Directory 的 SSH 集成的体系结构指南
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/19/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb766150339820f9356fe94311cd1ff33dda5480
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462857"
---
# <a name="ssh"></a>SSH  

安全外壳 (SSH) 是一种网络协议，该协议通过不安全的网络安全地提供操作网络服务加密。 SSH 还提供命令行登录、执行远程命令以及安全传输文件。 它通常用于基于 UNIX 的系统，如 Linux®。 SSH 替换 Telnet 协议，该协议不提供不安全网络中的加密。 

Azure Active Directory (Azure AD) 为在 Azure 上运行的基于 Linux (的系统提供虚拟机) VM®扩展。 

## <a name="use-when"></a>何时使用 

* 使用需要远程登录的基于 Linux®的 Vm

* 在基于 Linux®的系统中执行远程命令

* 安全传输不安全网络中的文件

![具有 SSH 协议的 Azure AD 关系图](./media/authentication-patterns/ssh-auth.png)

SSH 与 Azure AD

## <a name="components-of-system"></a>系统组件 

* **User**：启动 SSH 客户端以设置与 Linux® vm 的连接，并提供用于身份验证的凭据。

* **Web 浏览器**：用户与之交互的组件。 它与标识提供者通信 (Azure AD) 安全地对用户进行身份验证和授权。

* **SSH 客户端**：驱动连接设置过程。

* **Azure AD**：使用设备流对用户身份进行身份验证，并向 Linux vm 颁发令牌。

* **LINUX VM**：接受令牌并提供成功的连接。

## <a name="implement-ssh-with-azure-ad"></a>实现 SSH 与 Azure AD 

* [使用 Azure Active Directory 凭据登录到 Linux® VM-Azure 虚拟机 ](https://docs.microsoft.com/azure/virtual-machines/linux/login-using-aad) 

* [OAuth 2.0 设备代码流-Microsoft 标识平台 ](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)

* [与 Azure Active Directory (akamai.com) 集成 ](https://learn.akamai.com/webhelp/enterprise-application-access/enterprise-application-access/GUID-6B16172C-86CC-48E8-B30D-8E678BF3325F.html)

 
