---
title: IoT 中心设备预配服务-角色和操作
description: 本文提供使用 IoT 设备预配服务 (DPS) 开发和 IoT 解决方案时所涉及的角色和操作的概念性概述。
author: wesmc7777
ms.author: wesmc
ms.date: 09/14/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: eliotga
ms.openlocfilehash: 52c0dcf4bd9f7b2ca8928eab5272a6500cbb379d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "90533040"
---
# <a name="roles-and-operations"></a>角色和操作

由于生产现实（如制造时间、发货、海关处理等），开发 IoT 解决方案的阶段可能跨越数周或数月。此外，如果涉及到各种实体，它们可以跨多个角色跨越活动。 本主题将深入探讨与每个阶段相关的各种角色和操作，然后在序列图中说明流。 

预配还要求设备制造商的要求，具体取决于启用 [证明机制](concepts-service.md#attestation-mechanism)。 制造操作也可以独立于自动预配阶段的时间发生，尤其是在建立自动预配后采购新设备的情况下。

左侧目录中提供了快速入门系列教程，旨在帮助通过实践解释自动预配。 为了促进/简化学习过程，我们将使用软件来模拟用于登记和注册的物理设备。 某些快速入门要求完成多个角色的操作，包括不存在的角色的操作（因为快速入门中所述的设备是模拟性的）。

| 角色 | 操作 | 说明 |
|------| --------- | ------------|
| 制造商 | 为标识和注册 URL 编码 | 根据所用的认证机制，制造商需负责为设备标识信息和设备预配服务注册 URL 编码。<br><br>**快速入门**：由于设备是模拟的，因此不存在制造商角色。 有关如何获取这些信息以便在编写示例注册应用程序代码时使用的详细信息，请参阅“开发人员角色”。 |
| | 提供设备标识 | 作为设备标识信息的发起者，制造商需负责通信向操作员（或指定的代理）传递这些信息，或者通过 API 将其直接登记到设备预配服务。<br><br>**快速入门**：由于设备是模拟的，因此不存在制造商角色。 有关如何获取在设备预配服务实例中登记模拟设备时所用的设备标识的详细信息，请参阅“操作员角色”。 |
| 运算符 | 配置自动预配 | 此操作对应于自动预配的第一阶段。<br><br>**快速入门**：你需要履行操作员角色，在 Azure 订阅中配置设备预配服务和 IoT 中心实例。 |
|  | 登记设备标识 | 此操作对应于自动预配的第二个阶段。<br><br>**快速入门**：你需要履行操作员角色，在设备预配服务实例中登记模拟设备。 设备标识由快速入门中模拟的认证方法（TPM 或 X.509）确定。 有关认证详细信息，请参阅“开发人员角色”。 |
| 设备预配服务<br>IoT 中心 | \<all operations\> | 对于使用物理设备的生产实现和使用模拟设备的快速入门，需要通过 Azure 订阅中配置的 IoT 服务履行这些角色。 角色/操作的功能完全相同，因为 IoT 服务预配物理设备和模拟设备的方式没有差别。 |
| 开发人员 | 生成/部署注册软件 | 此操作对应于自动预配的第三个阶段。 开发人员负责使用相应的 SDK 生成注册软件并将其部署到设备。<br><br>**快速入门**：生成的示例注册应用程序模拟适用于所选平台/语言的、在工作站中运行的真实设备（而不是将其部署到物理设备）。 注册应用程序执行的操作与部署到物理设备的应用程序相同。 需要指定认证方法（TPM 或 X.509 证书），以及设备预配服务实例的注册 URL 和“ID 范围”。 设备标识由 SDK 认证逻辑在运行时根据指定的方法确定： <ul><li>**TPM 认证** - 开发工作站运行 [TPM 模拟器应用程序](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator)。 运行后，将使用单独的应用程序来提取 TPM 的“认可密钥”和“注册 ID”用于登记设备标识。 SDK 认证逻辑在注册期间也使用模拟器来提供签名的 SAS 令牌用于身份验证和登记验证。</li><li>**X509 认证** - 使用工具[生成证书](how-to-use-sdk-tools.md#x509-certificate-generator)。 生成后，需创建用于登记的证书文件。 SDK 认证逻辑在注册期间也使用该证书来提供身份验证和登记验证。</li></ul> |
| 设备 | 启动和注册 | 此操作对应于自动预配的第三个阶段，由开发人员生成的设备注册软件执行。 有关详细信息，请参阅“开发人员角色”。 首次启动时： <ol><li>应用程序根据开发期间指定的全局 URL 和服务“ID 范围”，与设备预配服务实例建立连接。</li><li>连接后，将会根据登记期间指定的认证方法和标识对设备进行身份验证。</li><li>完成身份验证后，设备将注册到预配服务实例指定的 IoT 中心实例。</li><li>注册成功后，向注册应用程序返回唯一的设备 ID 和 IoT 中心终结点，以便与 IoT 中心通信。</li><li> 从此时起，设备可以提取用于配置的初始[设备孪生](~/articles/iot-hub/iot-hub-devguide-device-twins.md)状态，并开始执行报告遥测数据的过程。</li></ol>**快速入门**：由于设备是模拟的，因此注册软件在开发工作站上运行。|

下图汇总了设备自动预配期间的角色和操作顺序：
<br><br>
[![设备的自动预配顺序](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png)](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png#lightbox) 

> [!NOTE]
> （可选）制造商还可以使用设备预配服务 API（而不是通过操作员）执行“登记设备标识”操作。 有关此顺序和其他要素的详细讨论，请观看[自动将设备注册到 Azure IoT](https://youtu.be/cSbDRNg72cU?t=2460) 视频（从 41:00 标记处开始）

## <a name="roles-and-azure-accounts"></a>角色和 Azure 帐户

每个角色映射到 Azure 帐户的方式取决于方案，而涉及的方案可能有很多。 可以通过下面的常见模式大致了解角色通常情况下是如何映射到 Azure 帐户的。

#### <a name="chip-manufacturer-provides-security-services"></a>芯片制造商提供安全服务

在此方案中，制造商为一级客户管理安全。 一级客户可能首选此方案，因为不需管理详细的安全性。 

制造商将安全性引入硬件安全模块 (HSM)。 此安全性可能包括制造商从可能的客户处获取密钥、证书等，这些客户已经设置 DPS 实例和注册组。 制造商也可以为其客户生成此安全信息。

在此方案中，可能涉及两个 Azure 帐户：

- **帐户 1**：可能在操作员和开发人员角色之间存在某种程度的共享。 该方可能从制造商处购买 HSM 芯片。 这些芯片指向与帐户 1 关联的 DPS 实例。 使用 DPS 注册，该方可以将设备租赁给多个二级客户，只需在 DPS 中重新配置设备注册设置即可。 该方还可能为最终用户后端系统分配了可以连接的 IoT 中心，用于访问设备遥测数据等内容。如果是后一种情况，则可能不需要第二个帐户。

- **帐户 2**：最终用户（二级客户）可以有自己的 IoT 中心。 与帐户 1 相关联的一方可以直接将租赁的设备指向此帐户中的正确中心。 此配置要求跨 Azure 帐户链接 DPS 和 IoT 中心，该操作可以通过 Azure 资源管理器模板来完成。

#### <a name="all-in-one-oem"></a>一体化 OEM

制造商可能是“一体化 OEM”，只需一个制造商帐户。 制造商负责端到端的安全性和预配。

制造商可能为购买设备的客户提供基于云的应用程序。 此应用程序会与制造商分配的 IoT 中心连接。

售货机或自动咖啡机是此方案的示例。




## <a name="next-steps"></a>后续步骤

我们建议将本文加入书签，以便在学习相应自动预配快速入门的过程中参考。 

首先请完成最适合你偏好的管理工具的“设置自动预配”快速入门，其中逐步讲解了“服务配置”阶段：

- [使用 Azure CLI 设置自动预配](quick-setup-auto-provision-cli.md)
- [使用 Azure 门户设置自动预配](quick-setup-auto-provision.md)
- [使用资源管理器模板设置自动预配](quick-setup-auto-provision-rm.md)

然后，继续使用 "预配设备" 快速入门，它符合设备认证机制和设备预配服务 SDK/语言首选项。 此快速入门逐步讲解“设备登记”与“设备注册和配置”阶段： 

| 设备证明机制 | 快速入门 SDK/语言 | 
| ------------------------------- | -------------------- |
| 对称密钥 | [C](quick-create-simulated-device-symm-key.md)<br>[Java](quick-create-simulated-device-symmetric-key-java.md)<br>[Python](quick-create-device-symmetric-key-python.md) |
| X.509 证书 | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |
| 模拟受信任的平台模块 (TPM)  | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |




