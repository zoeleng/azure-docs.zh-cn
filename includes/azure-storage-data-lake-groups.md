---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 73a8f20122e21c4557289995c15b008401285011
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131643"
---
始终使用 [Azure AD 安全组](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups) 作为 ACL 条目中分配的主体。 拒绝直接分配各个用户或服务主体。 使用此结构，你可以添加和删除用户或服务主体，不需要向整个目录结构重新应用 ACL。 相反，只需在适当的 Azure AD 安全组中添加或删除用户和服务主体。 

有多种不同的方法来设置组。 例如，假设你有一个名为 **/LogData** 的目录，其中包含你的服务器生成的日志数据。 Azure 数据工厂 (ADF) 将数据引入到该文件夹。 服务工程团队的特定用户将上传日志并管理此文件夹的其他用户，而各个 Databricks 群集将从该文件夹分析日志。 

若要启用这些活动，你可以创建一个 `LogsWriter` 组和一个 `LogsReader` 组。 然后，可以按如下所示分配权限：

- 将 `LogsWriter` 组添加到具有权限的 **/LogData** 目录的 ACL `rwx` 。
- 将 `LogsReader` 组添加到具有权限的 **/LogData** 目录的 ACL `r-x` 。
- 向组中添加服务主体对象或 (MSI) 托管服务标识 `LogsWriters` 。
- 将服务工程团队中的用户添加到 `LogsWriter` 组。
- 将 Databricks 的服务主体对象或 MSI 添加到 `LogsReader` 该组。

如果服务工程团队中的用户离开了公司，则可以将其从组中删除 `LogsWriter` 。 如果未将该用户添加到组，而是为该用户添加了专用 ACL 条目，则必须从 **/LogData** 目录中删除该 acl 条目。 还需要删除 **/LogData** 目录的整个目录层次结构中所有子目录和文件中的条目。 

若要创建组并添加成员，请参阅 [使用 Azure Active Directory 创建基本组并添加成员](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal)。