---
title: 处理云服务生命周期事件 | Microsoft Docs
description: 了解如何在 .NET 中使用云服务角色的生命周期方法，包括 RoleEntryPoint，它提供了用于响应生命周期事件的方法。
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.custom: devx-track-csharp
ms.topic: article
ms.date: 07/18/2017
ms.author: tagore
ms.openlocfilehash: d64414abfbc62e52b172a2c42796ec8d89d1719f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "88930054"
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>在 .NET 中自定义 Web 角色或辅助角色的生命周期
在创建辅助角色时，需要扩展 [RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) 类，重写该类提供的方法可以响应生命周期事件。 此类对于 Web 角色而言是可选的，因此，必须将它用于响应生命周期事件。

## <a name="extend-the-roleentrypoint-class"></a>扩展 RoleEntryPoint 类
[RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) 类包含由 Azure 在启动、运行或停止 Web 角色或辅助角色时调用的方法    。 可以选择重写这些方法，管理角色初始化、角色关闭序列或角色执行线程。 

在扩展 RoleEntryPoint 时，应该了解方法的以下行为  ：

* [OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) 方法会返回一个布尔值，因此，可以从此方法返回 false  。
  
   如果代码返回 false，则该角色进程将突然终止，而不会运行可能拥有的任何关闭序列  。 一般来说，应该避免从 OnStart 方法返回 false   。
* RoleEntryPoint 方法重载中未捕获的任何异常都被视为未经处理的异常  。
  
   如果某个生命周期方法中发生异常，则 Azure 将引发 [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) 事件，然后进程会终止。 角色脱机后，Azure 会将它重启。 如果出现未经处理的异常，则不会引发 [Stopping](/previous-versions/azure/reference/ee758136(v=azure.100)) 事件，并且不会调用 **OnStop** 方法。

如果角色未启动，或者在初始化、繁忙和停止状态之间循环，可能是因为每次角色重启生命周期事件时，代码都引发了未经处理的异常。 在这种情况下，请使用 [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) 事件确定异常的原因并采用适当的方法进行处理。 角色也可能会从 [Run](/previous-versions/azure/reference/ee772746(v=azure.100)) 方法返回，从而促使角色重启。 有关部署状态的详细信息，请参阅[导致角色回收的常见问题](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)。

> [!NOTE]
> 如果当前使用 Azure Tools for Microsoft Visual Studio 开发应用程序，则角色项目模板将自动在 WebRole.cs 和 WorkerRole.cs 文件中扩展 RoleEntryPoint 类     。
> 
> 

## <a name="onstart-method"></a>OnStart 方法
当 Azure 使角色实例联机时，就会调用 OnStart 方法  。 OnStart 代码执行时，角色实例被标记为“忙”，并且负载均衡器不会将外部流量引导到该角色  。 可以重写此方法以执行初始化工作，例如实现事件处理程序和启动 [Azure 诊断](cloud-services-how-to-monitor.md)。

如果 OnStart 返回 true，则该实例已成功初始化，并且 Azure 已调用 RoleEntryPoint.Run 方法    。 如果 OnStart 返回 false，则角色将立即终止，而不执行任何计划中的关闭序列   。

下面的代码示例演示如何重写 OnStart 方法  。 当角色实例启动并设置将日志记录数据传输到存储帐户时，此方法将配置并启动诊断监视器：

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop 方法
OnStop 方法在 Azure 使角色实例脱机之后并且在进程退出之前被调用  。 可以重写此方法调用角色实例所需的代码，从而彻底关闭此角色实例。

> [!IMPORTANT]
> 如果出于某些原因而不是用户启动的关闭而调用 OnStop 方法，则此方法中运行的代码必须在有限的时间内完成  。 这段时间过后，进程将被终止，因此，必须确保 OnStop 方法中的代码能够快速运行或者不必完全运行  。 引发 Stopping 事件后，会调用 OnStop 方法   。
> 
> 

## <a name="run-method"></a>Run 方法
可以重写 Run 方法，为角色实例实现长时间运行的线程  。

重写 Run 方法不是必需的；默认实现可以启动一个永远休眠的线程  。 如果重写 Run 方法，则代码可能会被无限期阻止  。 如果 Run 方法返回，角色将正常自动回收；也就是说，Azure 将引发 Stopping 事件并调用 OnStop 方法，因此，在角色脱机之前可能执行了关闭序列    。

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>为 Web 角色实现 ASP.NET 生命周期方法
除了 RoleEntryPoint 类提供的方法以外，还可以使用 ASP.NET 生命周期方法管理 Web 角色的初始化和关闭序列  。 如果要将现有的 ASP.NET 应用程序迁移到 Azure，此方法可能有利于解决兼容性问题。 ASP.NET 生命周期方法将从 RoleEntryPoint 方法中调用  。 Application\_Start 方法将在 RoleEntryPoint.OnStart 方法结束后调用   。 Application\_End 方法将在 RoleEntryPoint.OnStop 方法调用前调用   。

## <a name="next-steps"></a>后续步骤
了解如何[创建云服务包](cloud-services-model-and-package.md)。




