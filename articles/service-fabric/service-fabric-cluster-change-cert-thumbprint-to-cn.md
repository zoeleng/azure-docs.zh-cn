---
title: 更新群集以使用证书公用名称
description: 了解如何将 Service Fabric 群集证书从基于指纹的声明转换为公用名。
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 224798565921593d3c91dfcc187efa71a71b1fdd
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92368054"
---
# <a name="convert-cluster-certificates-from-thumbprint-based-declarations-to-common-names"></a>将群集证书从基于指纹的声明转换为公用名
证书 (俗称称为 "指纹" ) 的签名是唯一的，这意味着指纹声明的群集证书是指证书的特定实例。 这进而使证书滚动更新和管理成为一项常见的难题：每个更改都需要协调群集和基础计算主机的升级。 将 Service Fabric 群集的证书声明从基于指纹的证书声明转换为基于证书的使用者公用名的声明可简化管理，特别是，滚动更新证书不再需要群集升级。 本文介绍如何在不停机的情况下将现有群集转换为常见的基于名称的声明。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="moving-to-certificate-authority-ca-signed-certificates"></a>转到证书颁发机构 (CA) 签名证书
由指纹声明其证书的群集的安全将停留在不可能的情况下，或者通过计算不可行来伪造证书，使其与另一证书具有相同的签名。 在这种情况下，证书的 provenance 不太重要，因此，自签名证书就足够了。 相反，具有公用名声明的证书的群集的安全性从公钥基础结构服务流到颁发该证书的 (PKI) ，并包括其证书做法的某些方面，无论其操作安全是否经过审核以及其他许多方面。 出于此原因，PKI 的选择非常重要，需要特别了解颁发者 (证书颁发机构或 CA) ，而自签名证书实质上是毫无意义的。 如果证书链可以成功生成，则由公用名 (CN) 声明的证书通常被视为有效，该主题具有所需的 CN 元素，并且其颁发者 (链) 中的立即或更高级别由执行验证的代理信任。 Service Fabric 支持以具有 "隐性" 颁发者的 CN 声明证书 (链必须以信任密钥) 结束，或使用指纹声明的颁发者 ( "颁发者固定" ) ;有关更多详细信息，请参阅此  [文](cluster-security-certificates.md#common-name-based-certificate-validation-declarations) 。 若要使用指纹声明为公用名的自签名证书转换群集，必须先通过指纹将目标、CA 签名证书引入到群集;只有这样，才能实现从 TP 到 CN 的转换。

出于测试目的，自签名证书可由 CN 声明，将颁发者固定到其自己的指纹;从安全角度来看，这几乎相当于按 TP 声明相同的证书。 但请注意，这种类型的成功转换并不保证使用 CA 签名的证书成功地从 TP 转换到 CN。 因此，建议使用) 的 (可用选项，使用正确的 CA 签名证书测试转换。

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>上传证书并将其安装在规模集中
在 Azure 中，用于获取和设置证书的推荐机制涉及到 Azure Key Vault 服务及其工具。 与群集证书声明匹配的证书必须预配到组成群集的虚拟机规模集的每个节点;有关更多详细信息，请参阅 [虚拟机规模集上的机密](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-securely-ship-a-certificate-to-the-vm) 。 在群集的证书声明中进行更改之前，当前和目标群集证书都安装在群集的每个节点类型的 Vm 上，这一点很重要。 [此处](cluster-security-certificate-management.md#the-journey-of-a-certificate)深入讨论了从证书颁发到预配到 Service fabric 节点的旅程。

## <a name="bring-cluster-to-an-optimal-starting-state"></a>使群集进入最佳启动状态
将证书声明从基于指纹的转换转换为基于常见名称的影响两者：

- 群集中的每个节点如何查找并向其他节点显示其凭据
- 建立安全连接时，每个节点如何验证其对应项的凭据  

在继续操作之前，请查看 [这两种配置的表示和验证规则](cluster-security-certificates.md#certificate-configuration-rules) 。 执行指纹到公用名称转换时，最重要的注意事项是升级和未升级的节点 (也就是说，属于不同升级域) 的节点必须能够在升级过程中的任何时候执行成功的相互身份验证。 为实现此目的，推荐的方法是在初始升级中按指纹声明目标/目标证书，并在后续的升级中完成过渡到公用名。 如果群集已处于建议的启动状态，则可以跳过此部分。

有多个有效的转换起始状态;固定条件是，在升级到公用名时，群集已在使用指纹) 声明 (目标证书。 我们考虑 `GoalCert` ， `OldCert1` ， `OldCert2` ：

#### <a name="valid-starting-states"></a>有效的起始状态
- `Thumbprint: GoalCert, ThumbprintSecondary: None`
- `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`，其中的 `GoalCert` 日期晚 `NotAfter` 于 `OldCert1`
- `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`，其中的 `GoalCert` 日期晚 `NotAfter` 于 `OldCert1`

如果你的群集未处于上述有效状态之一，请参阅本文末尾的 "实现该状态的附录"。

## <a name="select-the-desired-common-name-based-certificate-validation-scheme"></a>选择所需的基于通用名称的证书验证方案
如前文所述，Service Fabric 支持使用隐式信任定位点或显式固定颁发者指纹来声明证书。 请参阅 [此文](cluster-security-certificates.md#common-name-based-certificate-validation-declarations) 以了解详细信息，并确保你充分了解这些差异，以及选择任一机制的含义。 从语法上讲，这种差异/选择由参数的值确定 `certificateIssuerThumbprintList` ： empty 表示依赖受信任的根 CA (信任密钥) ，而一组指纹限制允许的群集证书直接颁发者。

   > [!NOTE]
   > "CertificateIssuerThumbprint" 字段允许指定由使用者公用名声明的证书的预期直接颁发者。 可接受的值为一个或多个以逗号分隔的 SHA1 指纹。 请注意，这是加强证书验证的方式-如果未指定颁发者/列表为空，则如果证书链可以生成，则接受身份验证证书，并最终出现在验证程序信任的根中。 如果指定了一个或多个颁发者指纹，则将接受该证书，前提是该证书的直接颁发者的指纹（从链中提取）与此字段中指定的任何值匹配，无论根是否可信都是如此。 请注意，PKI 可以使用不同的证书颁发机构 ( "颁发者" ) 对具有给定主题的证书进行签名，因此请务必为该使用者指定所有预期的颁发者指纹。 换句话说，证书续订不保证由与要续订的证书相同的颁发者进行签名。
   >
   > 指定颁发者被认为是最佳做法，但对于可以链接成受信任的根证书的证书，省略颁发者也是可行的，此行为存在限制，在不久的将来可能会被淘汰。 另请注意，如果在 Azure 中部署的群集受由某个专用 PKI 颁发且通过使用者声明的 X509 证书保护，并且该 PKI 的证书策略不可发现、不可用且无法访问，则 Azure Service Fabric 服务可能无法对群集进行验证（对于群集到服务通信）。 

## <a name="update-the-clusters-azure-resource-management-arm-template-and-deploy"></a>更新群集的 Azure 资源管理 (ARM) 模板并进行部署
建议使用 ARM 模板管理 Azure Service Fabric 群集;另一种方法（也使用 json 项目）是 [Azure 资源浏览器 (预览版) ](https://resources.azure.com)。 目前 Azure 门户中不提供等效的体验。 如果与现有群集相对应的原始模板不可用，则可以通过导航到包含该群集的资源组，从 "**自动化**" 左侧菜单中选择 "**导出模板**"，然后进一步选择所需的资源，在 Azure 门户中获取等效模板：至少应导出虚拟机规模集和群集资源。 还可以下载生成的模板。 请注意，此模板可能需要在完全可部署之前进行更改，并且可能不完全与原始群集资源完全匹配-它是群集资源的当前状态的反射。

必要的更改如下所示：
    - 更新虚拟机资源 (下的 Service Fabric 节点扩展的定义) ;如果群集定义了多个节点类型，则需要更新每个相应虚拟机规模集的定义
    - 更新群集资源定义

下面提供了详细的示例。

### <a name="updating-the-virtual-machine-scale-set-resources"></a>正在更新 (的虚拟机规模集资源) 
源
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "thumbprint": "[parameters('certificateThumbprint')]",
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```
功能
```json
"virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            ...
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            ...
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        ...
                    }
                },
```

### <a name="updating-the-cluster-resource"></a>更新群集资源
在**ServiceFabric/群集**资源中，添加具有**CommonNames**设置的**certificateCommonNames**属性，并 (所有其设置) 中完全删除**证书**属性：

源
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```
功能
```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            ...
        ],
        "properties": {
            "addonFeatures": [
                ...
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprintList')]"
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
```

有关详细信息，请参阅 [部署使用证书公用名而不是指纹的 Service Fabric 群集。](./service-fabric-create-cluster-using-cert-cn.md)

## <a name="deploy-the-updated-template"></a>部署已更新的模板
在进行更改后，重新部署已更新的模板。

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="appendix-achieve-a-valid-starting-state-for-converting-a-cluster-to-cn-based-certificate-declarations"></a>附录：实现将群集转换为基于 CN 的证书声明的有效开始状态

| 开始状态 | 升级 1 | 升级 2 |
| :--- | :--- | :--- |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` 和的 `GoalCert` 日期晚 `NotAfter` 于 `OldCert1` | `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: None` 和的 `OldCert1` 日期晚 `NotAfter` 于 `GoalCert` | `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1` | `Thumbprint: GoalCert, ThumbprintSecondary: None` |
| `Thumbprint: OldCert1, ThumbprintSecondary: GoalCert`，其中的 `OldCert1` 日期晚 `NotAfter` 于 `GoalCert` | 升级到 `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: GoalCert, ThumbprintSecondary: OldCert1`，其中的 `OldCert1` 日期晚 `NotAfter` 于 `GoalCert` | 升级到 `Thumbprint: GoalCert, ThumbprintSecondary: None` | - |
| `Thumbprint: OldCert1, ThumbprintSecondary: OldCert2` | 删除 `OldCert1` 或中的一个 `OldCert2` 以获取状态 `Thumbprint: OldCertx, ThumbprintSecondary: None` | 从新的起始状态继续 |

有关如何执行这些升级的说明，请参阅 [此文档](service-fabric-cluster-security-update-certs-azure.md)。


## <a name="next-steps"></a>后续步骤
* 了解[群集安全性](service-fabric-cluster-security.md)。
* 了解如何 [按公用名滚动群集证书](service-fabric-cluster-rollover-cert-cn.md)
* 了解如何为 [touchless autorollover 配置群集](cluster-security-certificate-management.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
