---
title: Azure Active Directory 使用 Fiddler REST API 测试
description: 使用 Fiddler 测试 Azure 应用配置 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 3766567fe58e8d2eb86556d3defa7a85efd9b2fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423895"
---
# <a name="test-the-azure-app-configuration-rest-api-using-fiddler"></a>使用 Fiddler 测试 Azure 应用配置 REST API

若要使用 [Fiddler](https://www.telerik.com/fiddler)测试 REST API，需要在请求中包含 [身份验证](./rest-api-authentication-hmac.md) 所需的 HTTP 标头。 下面介绍了如何配置 Fiddler 以测试 REST API，并自动生成身份验证标头：

1. 确保 TLS 1.2 是允许的协议：
    1. 请参阅 " **工具** " "选项" "  >  **Options**  >  **HTTPS** ) "。
    1. 确保已选中 " **解密 HTTPS 流量** "。
    1. 在协议列表中，添加 **tls 1.2** （如果不存在）。
1. 打开 **Fiddler 脚本编辑器** ，或在 Fiddler 内按 **Ctrl + R**
1. 在类中将以下代码添加到 `Handlers` `OnBeforeRequest` 函数之前

    ```js
    static function SignRequest(oSession: Session, credential: String, secret: String) {
        var utcNow = DateTimeOffset.UtcNow.ToString("r", System.Globalization.DateTimeFormatInfo.InvariantInfo);
        var contentHash = ComputeSHA256Hash(oSession.RequestBody);
        var stringToSign = oSession.RequestMethod.ToUpperInvariant() + "\n" + oSession.PathAndQuery + "\n" + utcNow +";" + oSession.hostname + ";" + contentHash;
        var signature = ComputeHMACHash(secret, stringToSign);

        oSession.oRequest.headers["x-ms-date"] = utcNow;
        oSession.oRequest.headers["x-ms-content-sha256"] = contentHash;
        oSession.oRequest.headers["Authorization"] = "HMAC-SHA256 Credential=" + credential + "&SignedHeaders=x-ms-date;host;x-ms-content-sha256&Signature=" + signature;
    }

    static function ComputeSHA256Hash(content: Byte[]) {
        var sha256 = System.Security.Cryptography.SHA256.Create();
        try {
            return Convert.ToBase64String(sha256.ComputeHash(content));
        }
        finally {
            sha256.Dispose();
        }
    }

    static function ComputeHMACHash(secret: String, content: String) {
        var hmac = new System.Security.Cryptography.HMACSHA256(Convert.FromBase64String(secret));
        try {
            return Convert.ToBase64String(hmac.ComputeHash(System.Text.Encoding.ASCII.GetBytes(content)));
        }
        finally {
            hmac.Dispose();
        }
    }
    ```

1. 在函数的末尾添加以下代码 `OnBeforeRequest` 。 按 TODO 注释所示更新访问密钥。

    ```js
    if (oSession.isFlagSet(SessionFlags.RequestGeneratedByFiddler) &&
        oSession.hostname.EndsWith(".azconfig.io", StringComparison.OrdinalIgnoreCase)) {

        // TODO: Replace the following placeholders with your access key
        var credential = "<Credential>"; // Id
        var secret = "<Secret>"; // Value

        SignRequest(oSession, credential, secret);
    }
    ```
1. 使用 [Fiddler 的编辑器](https://docs.telerik.com/fiddler/Generate-Traffic/Tasks/CreateNewRequest) 生成并发送请求
