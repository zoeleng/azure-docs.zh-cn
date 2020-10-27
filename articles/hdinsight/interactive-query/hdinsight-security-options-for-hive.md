---
title: Azure HDInsight 中 Hive 的安全选项
description: 标准和 ESP 群集中 Hive 的安全性选项。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: 8573ba99b7aef13025b4f175640ac9583ad5a679
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545950"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Azure HDInsight 中 Hive 的安全选项

本文档介绍了 HDInsight 中 Hive 的建议安全选项。 可以通过 Ambari 配置这些选项。

!["Hive 的安全选项"](./media/hdinsight-security-options-for-hive/security-options-hive.png "Hive 的安全选项")

## <a name="hiveserver2-authentication"></a>HiveServer2 authentication

对于标准群集，推荐的 HiveServer2 authentication 设置为 none。 若要启用身份验证，我们建议企业安全性套餐) 群集升级到 [ESP](../domain-joined/hdinsight-security-overview.md) (。 

对于 ESP 群集，默认情况下会启用 [Kerberos](https://web.mit.edu/Kerberos/) 身份验证。 不支持 (PAM) 和自定义身份验证方案的可插入身份验证模块。

## <a name="hiveserver2-authorization"></a>HiveServer2 授权

对于标准群集，默认设置为 "无"。 [SqlStdAuth 可以启用基于 SQL 标准的授权)  (](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) 。 标准群集不支持通过 [Apache Ranger](https://ranger.apache.org/) 进行的授权。 建议升级到 ESP 群集以进行 Ranger 授权。 

对于 ESP 群集，默认情况下启用通过 Ranger 的授权。 


## <a name="ssl-encryption-for-hiveserver2"></a>HiveServer2 的 SSL 加密

不建议为标准群集或 ESP 群集启用 Hiveserver2 SSL。 已在网关上启用 SSL。 可以使用[Internet 协议安全 (IPSec) ](https://en.wikipedia.org/wiki/IPsec)启用[传输中的加密](../domain-joined/encryption-in-transit.md)，以加密群集节点之间的通信。


## <a name="next-steps"></a>后续步骤
* [HiveServer2 Authentication 概述](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [HiveServer2 授权概述](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization#:~:text=%20Overview%20of%20Authorization%20Modes%20%201%201,and%20Apache%20Sentry%20are%20apache%20projects...%20More%20)
* [启用基于 SQL 标准的 Hive 授权](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [具有 Hive 的 Apache Ranger](../domain-joined/apache-domain-joined-run-hive.md)
