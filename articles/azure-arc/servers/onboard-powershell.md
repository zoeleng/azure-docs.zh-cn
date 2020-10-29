---
title: 使用 PowerShell 将混合计算机连接到 Azure
description: 本文介绍如何安装代理，以及如何通过 PowerShell 使用启用了 Azure Arc 的服务器将计算机连接到 Azure。
ms.date: 10/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0755846ef02377edade98b69e478908a111ab247
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92901530"
---
# <a name="connect-hybrid-machines-to-azure-using-powershell"></a>使用 PowerShell 将混合计算机连接到 Azure

可以通过执行一系列手动步骤为环境中的一个或多个 Windows 或 Linux 计算机启用启用了 Azure Arc 的服务器。 或者，可以使用 PowerShell cmdlet [AzConnectedMachine](/powershell/module/az.connectedmachine/remove-azconnectedmachine) 下载连接的计算机代理、安装代理，并使用 Azure Arc 注册计算机。Cmdlet 从 Microsoft 下载中心下载 Windows 代理 Windows Installer 包，并从 Microsoft 的包存储库下载 Linux 代理包。

这种安装和配置代理的方法要求你在计算机上拥有管理员权限。 在 Linux 上，需使用 root 帐户；在 Windows 上，你需是“本地管理员组”的成员。 你可以使用 [PowerShell 远程](/powershell/scripting/learn/ps101/08-powershell-remoting)处理在 Windows server 上以交互方式或远程方式完成此过程。

在开始之前，请务必查看[先决条件](agent-overview.md#prerequisites)，并验证你的订阅和资源是否符合要求。 有关支持的区域和其他相关注意事项的信息，请参阅 [支持的 Azure 区域](overview.md#supported-regions)。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

- 具有 Azure PowerShell 的计算机。 有关说明，请参阅[安装和配置 Azure PowerShell](/powershell/azure/)。

使用 Azure PowerShell 在启用了 Arc 的服务器管理的混合服务器上管理 VM 扩展之前，需要安装该 `Az.ConnectedMachine` 模块。 在启用 Arc 的服务器上运行以下命令：

```powershell
Install-Module -Name Az.ConnectedMachine
```

安装完成后，将返回以下消息：

`The installed extension ``Az.ConnectedMachine`` is experimental and not covered by customer support. Please use with discretion.`

## <a name="install-the-agent-and-connect-to-azure"></a>安装代理并连接到 Azure

1. 使用提升的权限打开 PowerShell 控制台。

2. 通过运行命令登录到 Azure `Connect-AzAccount` 。

3. 若要安装已连接的计算机代理，请将 `Connect-AzConnectedMachine` 与 `-Name` 、 `-ResourceGroupName` 和参数一起使用 `-Location` 。 使用 `-SubscriptionId` 参数重写默认订阅，作为在登录后创建的 Azure 上下文的结果。 运行下列命令之一：

    * 若要在可直接与 Azure 通信的目标计算机上安装已连接的计算机代理，请运行：

        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e
        ```
    
    * 若要在通过代理服务器进行通信的目标计算机上安装连接的计算机代理，请运行：
        
        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e -proxy http://<proxyURL>:<proxyport>
        ```

如果完成安装后代理无法启动，请检查日志以获取详细的错误信息。 在 Windows 上的 *%ProgramData%\AzureConnectedMachineAgent\Log\himds.log* ，在 Linux 的 */var/opt/azcmagent/log/himds.log* 中。

## <a name="install-and-connect-using-powershell-remoting"></a>使用 PowerShell 远程处理进行安装和连接

执行以下步骤，以配置一台或多台启用了 Azure Arc 的服务器的 Windows server。 必须在远程计算机上启用 PowerShell 远程处理。 使用 `Enable-PSRemoting` cmdlet 启用 PowerShell 远程处理。

1. 以管理员身份打开 PowerShell 控制台。

2. 通过运行命令登录到 Azure `Connect-AzAccount` 。

3. 若要安装已连接的计算机代理，请将 `Connect-AzConnectedMachine` 与 `-Name` 、 `-ResourceGroupName` 和参数一起使用 `-Location` 。 使用 `-SubscriptionId` 参数重写默认订阅，作为在登录后创建的 Azure 上下文的结果。

    * 若要在可直接与 Azure 通信的目标计算机上安装已连接的计算机代理，请运行以下命令：
    
        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```
    
    * 若要同时在多台远程计算机上安装连接的计算机代理，请添加用逗号分隔的远程计算机名称的列表。

        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName1, myMachineName2, myMachineName3
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```

    下面的示例是针对单个计算机的命令的结果：
    
    ```azurepowershell
    time="2020-08-07T13:13:25-07:00" level=info msg="Onboarding Machine. It usually takes a few minutes to complete. Sometimes it may take longer depending on network and server load status."
    time="2020-08-07T13:13:25-07:00" level=info msg="Check network connectivity to all endpoints..."
    time="2020-08-07T13:13:29-07:00" level=info msg="All endpoints are available... continue onboarding"
    time="2020-08-07T13:13:50-07:00" level=info msg="Successfully Onboarded Resource to Azure" VM Id=f65bffc7-4734-483e-b3ca-3164bfa42941
    
    Name           Location OSName   Status     ProvisioningState
    ----           -------- ------   ------     -----------------
    myMachineName  eastus   windows  Connected  Succeeded
    ```

## <a name="verify-the-connection-with-azure-arc"></a>验证是否与 Azure Arc 连接

安装代理并将其配置为连接到启用了 Azure Arc 的服务器后，请转到 Azure 门户，验证是否已成功连接服务器。 在 [Azure 门户](https://portal.azure.com)中查看计算机。

![服务器连接成功](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>后续步骤

* 有关疑难解答信息，请参阅 [连接计算机代理疑难解答指南](troubleshoot-agent-onboard.md)。

* 了解如何使用 [Azure Policy](../../governance/policy/overview.md) 管理计算机，例如，进行 VM [来宾配置](../../governance/policy/concepts/guest-configuration.md)，验证计算机是否向预期的 Log Analytics 工作区报告，使用[用于 VM 的 Azure Monitor](../../azure-monitor/insights/vminsights-enable-policy.md) 启用监视等。

* 详细了解 [Log Analytics 代理](../../azure-monitor/platform/log-analytics-agent.md)。 需要收集操作系统和工作负荷监视数据、使用自动化 runbook 或功能（如更新管理）管理该数据，或使用 [Azure 安全中心](../../security-center/security-center-introduction.md)之类的其他 azure 服务时，需要使用适用于 Windows 和 Linux 的 Log Analytics 代理。