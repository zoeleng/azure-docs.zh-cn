---
title: Azure 应用配置 REST API-密钥
description: 使用 Azure 应用配置处理密钥的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: f44ecdf571791d54a78d25dde514d57053b59160
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423973"
---
# <a name="keys"></a>键

api 版本：1。0

以下语法表示密钥资源：

```http
{
    "name": [string]             // Name of the key
}
```

## <a name="operations"></a>操作

关键资源支持以下操作：

- 列出

所有操作 `name` 都是一个可选的筛选器参数。 如果省略，则表示 *任意* 键。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="list-keys"></a>列出密钥

```http
GET /keys?api-version={api-version} HTTP/1.1
```

**响应**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json; charset=utf-8"
```

```json
{
    "items": [
        {
          "name": "{key-name}"
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>分页

如果返回的项数超过响应限制，则会对结果进行分页。 跟随可选的 `Link` 响应标头并用于 `rel="next"` 导航。 或者，内容以属性的形式提供下一个链接 `@nextLink` 。 下一个链接包含 `api-version` 参数。

```http
GET /keys?api-version={api-version} HTTP/1.1
```

**回复**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json; charset=utf-8
Link: <{relative uri}>; rel="next"
```

```json
{
    "items": [
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="filtering"></a>Filtering

支持按筛选 ```name``` 。

```http
GET /keys?name={key-name}&api-version={api-version}
```

支持以下筛选器：

|密钥筛选器|效果|
|--|--|
|省略 `name` 或 `name=*`|匹配 **任意** 密钥|
|`name=abc`|匹配名为 **abc** 的密钥|
|`name=abc*`|匹配以 abc 开头的密钥名称|
|`name=abc,xyz`|匹配密钥名称 **abc** 或 **xyz** (限制为5个 CSV) |

保留以下字符： `*` 、 `\` 、 `,`

如果保留字符是值的一部分，则必须使用对其进行转义 `\{Reserved Character}` 。 非保留字符也可以转义。

## <a name="filter-validation"></a>筛选器验证

在出现筛选器验证错误的情况下，响应为 HTTP `400` 并带有错误详细信息：

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid request parameter 'name'",
  "name": "name",
  "detail": "name(2): Invalid character",
  "status": 400
}
```

## <a name="examples"></a>示例

- 全部

    ```http
    GET /keys?api-version={api-version}
    ```

- 项名称以 **abc** 开头

    ```http
    GET  /keys?name=abc*&api-version={api-version}
    ```

- 项名称为 **abc** 或 **xyz**

    ```http
    GET /keys?name=abc,xyz&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>请求特定字段

使用可选的 `$select` 查询字符串参数，并提供所请求字段的逗号分隔列表。 如果 `$select` 省略该参数，则响应将包含默认设置。

```http
GET /keys?$select=name&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Time-Based 访问

获取结果的表示形式，即过去的时间。 请参阅[2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1)部分

```http
GET /keys&api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**回复**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json"
Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT
Link: <relative uri>; rel="original"
```

```json
{
    "items": [
        ....
    ]
}
```
