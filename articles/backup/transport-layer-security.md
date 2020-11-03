---
title: Azure 备份中的传输层安全性
description: 了解如何启用 Azure 备份，以便使用加密协议传输层安全性 (TLS) 在通过网络传输数据时保护数据的安全。
ms.topic: conceptual
ms.date: 11/01/2020
ms.openlocfilehash: 9881fd3532cbc7d67c7d5adbce75e02fc62e0bcf
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280667"
---
# <a name="transport-layer-security-in-azure-backup"></a>Azure 备份中的传输层安全性

传输层安全 (TLS) 是一种加密协议，可在通过网络传输数据时保护数据的安全。 Azure 备份使用传输层安全性来保护正在传输的备份数据的隐私。 本文介绍了启用 TLS 1.2 协议的步骤，该协议提供了比以前版本更高的安全性。

## <a name="earlier-versions-of-windows"></a>早期版本的 Windows

如果计算机运行的是早期版本的 Windows，则必须安装以下所述的相应更新，并且必须应用知识库文章中所述的注册表更改。

|操作系统  |知识库文章 |
|---------|---------|
|Windows Server 2008 SP2   |   <https://support.microsoft.com/help/4019276>      |
|Windows Server 2008 R2、Windows 7、Windows Server 2012   | <https://support.microsoft.com/help/3140245>         |

>[!NOTE]
>此更新将安装所需的协议组件。 安装完成后，您必须对上述知识库文章中提到的注册表项进行更改，以正确启用所需的协议。

## <a name="verify-windows-registry"></a>验证 Windows 注册表

### <a name="configuring-schannel-protocols"></a>配置 SChannel 协议

以下注册表项可确保在 SChannel 组件级别启用 TLS 1.2 协议：

```reg
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]
    "Enabled"=dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]
    "DisabledByDefault"=dword:00000000
```

>[!NOTE]
>默认情况下，在 Windows Server 2012 R2 和更高版本中设置显示的值。 对于这些版本的 Windows，如果注册表项不存在，则无需创建它们。

### <a name="configuring-net-framework"></a>配置 .NET Framework

以下注册表项将 .NET Framework 配置为支持强加密。 可在此处阅读有关 [配置 .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/tls#configuring-schannel-protocols-in-the-windows-registry)的详细信息。

```reg
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
    "SystemDefaultTlsVersions"=dword:00000001
    "SchUseStrongCrypto" = dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
    "SystemDefaultTlsVersions"=dword:00000001
    "SchUseStrongCrypto" = dword:00000001
```

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="why-enable-tls-12"></a>为什么要启用 TLS 1.2？

TLS 1.2 比以前的加密协议（例如 SSL 2.0、SSL 3.0、TLS 1.0 和 TLS 1.1）更安全。 Azure 备份服务已完全支持 TLS 1.2。

### <a name="what-determines-the-encryption-protocol-used"></a>确定使用的加密协议是什么？

协商客户端和服务器支持的最高协议版本以建立加密会话。 有关 TLS 握手协议的详细信息，请参阅 [使用 Tls 建立安全会话](https://docs.microsoft.com/windows/win32/secauthn/tls-handshake-protocol#establishing-a-secure-session-by-using-tls)。

### <a name="what-is-the-impact-of-not-enabling-tls-12"></a>不启用 TLS 1.2 有什么影响？

为了更好地防御协议降级攻击，Azure 备份将以分阶段的方式开始禁用超过1.2 的 TLS 版本。 这是跨服务的长期转换的一部分，禁止旧协议和密码套件连接。 Azure 备份服务和组件完全支持 TLS 1.2。 但是，缺少所需更新或某些自定义配置的 Windows 版本仍会阻止提供 TLS 1.2 协议。 这可能会导致失败，其中包括但不限于以下一项或多项操作：

- 备份和还原操作可能会失败。
- 备份组件连接失败，并出现错误 10054 (现有连接被远程主机强制关闭) 。
- 与 Azure 备份相关的服务不会立即停止或启动。

## <a name="additional-resources"></a>其他资源

- [传输层安全协议](https://docs.microsoft.com/windows/win32/secauthn/transport-layer-security-protocol)
- [确保在已部署的操作系统中支持 TLS 1。2](https://docs.microsoft.com/security/engineering/solving-tls1-problem#ensuring-support-for-tls-12-across-deployed-operating-systems)
- [.NET Framework 中的传输层安全性 (TLS) 最佳做法](https://docs.microsoft.com/dotnet/framework/network-programming/tls)
