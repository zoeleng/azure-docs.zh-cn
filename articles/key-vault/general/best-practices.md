---
title: 使用 Key Vault 的最佳做法 - Azure Key Vault | Microsoft Docs
description: 了解 Azure Key Vault 的最佳做法，包括如何控制访问、何时使用单独的密钥保管库、备份、日志记录和恢复选项。
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-key-vault
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 3d959cccd4fbce24e36b1ed62bc6bf417af23c82
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445500"
---
# <a name="best-practices-to-use-key-vault"></a>使用密钥保管库的最佳做法

## <a name="control-access-to-your-vault"></a>控制对保管库的访问权限

Azure 密钥保管库是一种云服务，用于保护加密密钥和机密（例如证书、连接字符串和密码）。 因为此数据是敏感数据和业务关键数据，所以需要保护对密钥保管库的访问，只允许得到授权的应用程序和用户进行访问。 [本文](secure-your-key-vault.md)提供密钥保管库访问模型的概述。 其中介绍了身份验证和授权，以及如何保护对密钥保管库的访问。

控制对保管库的访问权限的建议如下：
1. 锁定对订阅、资源组和密钥保管库的访问， (Azure RBAC) 
2. 为每个保管库创建访问策略
3. 使用最低特权访问主体授予访问权限
4. 打开防火墙和 [VNET 服务终结点](overview-vnet-service-endpoints.md)

## <a name="use-separate-key-vault"></a>使用单独的密钥保管库

我们的建议是对每个环境（开发环境、预生产环境和生产环境）的每个应用程序使用一个保管库。 这可以帮助你避免在不同环境之间共享机密，并可在出现安全漏洞时降低威胁。

## <a name="backup"></a>Backup

在保管库中更新/删除/创建对象时确保定期执行保管库的备份。

### <a name="azure-powershell-backup-commands"></a>Azure PowerShell 备份命令

* [备份证书](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultCertificate?view=azurermps-6.13.0)
* [备份密钥](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultKey?view=azurermps-6.13.0)
* [备份机密](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultSecret?view=azurermps-6.13.0)

### <a name="azure-cli-backup-commands"></a>Azure CLI 备份命令

* [备份证书](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-backup)
* [备份密钥](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-backup)
* [备份机密](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-backup)


## <a name="turn-on-logging"></a>启用日志记录

为保管库[启用日志记录](logging.md)。 同时设置警报。

## <a name="turn-on-recovery-options"></a>启用恢复选项

1. 启用[软删除](soft-delete-overview.md)。
2. 如果要防止强制删除机密/保管库，即使启用软删除后，也要启用清除保护。