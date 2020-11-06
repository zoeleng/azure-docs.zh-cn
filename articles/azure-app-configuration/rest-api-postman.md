---
title: 使用 Postman Azure Active Directory REST API 测试
description: 使用 Postman 测试 Azure 应用配置 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 9690678fc7b794c694e588a7993cb131d8264a72
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423965"
---
# <a name="test-the-azure-app-configuration-rest-api-using-postman"></a>使用 Postman 测试 Azure 应用配置 REST API

若要使用 [Postman](https://www.getpostman.com/)测试 REST API，需要在请求中包含 [身份验证](./rest-api-authentication-hmac.md) 所需的 HTTP 标头。 下面介绍了如何配置 Postman 以测试 REST API，并自动生成身份验证标头：

1. 创建新 [请求](https://learning.getpostman.com/docs/postman/sending_api_requests/requests/)
1. 将 `signRequest` [JavaScript 身份验证示例](./rest-api-authentication-hmac.md#javascript)中的函数添加到请求的[预请求脚本](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/)
1. 将以下代码添加到预请求脚本的末尾。 按 TODO 注释所示更新访问密钥

    ```js
    // TODO: Replace the following placeholders with your access key
    var credential = "<Credential>"; // Id
    var secret = "<Secret>"; // Value

    var isBodyEmpty = pm.request.body === null || pm.request.body === undefined || pm.request.body.isEmpty();

    var headers = signRequest(
        pm.request.url.getHost(),
        pm.request.method,
        pm.request.url.getPathWithQuery(),
        isBodyEmpty ? undefined : pm.request.body.toString(),
        credential,
        secret);

    // Add headers to the request
    headers.forEach(header => {
        pm.request.headers.upsert({key: header.name, value: header.value});
    })
    ```

1. 发送请求
