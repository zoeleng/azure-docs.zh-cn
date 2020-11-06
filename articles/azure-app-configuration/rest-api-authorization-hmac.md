---
title: Azure 应用配置 REST API-HMAC 授权
description: 使用 HMAC 针对使用 REST API 的 Azure 应用配置进行授权
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 8d1f8a17f51711cc5c10567797e1224f96eb7630
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423930"
---
# <a name="hmac-authorization---rest-api-reference"></a>HMAC 授权-REST API 参考

使用 HMAC 身份验证时，操作分为两种类别之一：读取或写入。 读写访问密钥授予调用所有操作的权限。 只读访问密钥授予仅调用读取操作的权限。 访问键是否为只读或读写由其 `readOnly` 属性确定。 尝试使用只读访问密钥发出写入请求将导致请求未经授权。

## <a name="obtaining-access-keys"></a>获取访问密钥

[此处](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/appconfiguration/resource-manager/Microsoft.AppConfiguration/stable/2019-10-01/appconfiguration.json)Azure 应用配置资源提供程序规范介绍了用于获取访问密钥和用于获取它们的 API 的规范。 通过 "ConfigurationStores_ListKeys" 操作获取访问密钥。

## <a name="errors"></a>错误

```http
HTTP/1.1 403 Forbidden
```

**原因：** 用于对请求进行身份验证的访问密钥不提供执行请求的操作所需的权限。
**解决方案：** 获取提供执行请求的操作并使用它对请求进行身份验证的访问密钥。
