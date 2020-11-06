---
title: Azure 应用配置 REST API-键值修订
description: 使用 Azure 应用配置处理键值修订的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 7d1990d6bc524a69de2b22b4f7c5aeec88c3ce9d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423964"
---
# <a name="key-value-revisions"></a>键值修订

api 版本：1。0

**键值修订** 定义键值资源的历史表示形式。 对于免费层商店，修订版本为7天后过期，对于标准层商店则为30天。 修订版本支持以下操作：

- 列出

对于所有操作， ``key`` 是一个可选参数。 如果省略，则表示 **任意** 键。
对于所有操作， ``label`` 是一个可选参数。 如果省略，则表示 **任意** 标签。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="list-revisions"></a>列出修订

```http
GET /revisions?label=*&api-version={api-version} HTTP/1.1
```

**响应**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvset+json; charset=utf-8"
Accept-Ranges: items
```

```json
{
    "items": [
        {
          "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
          "key": "{key}",
          "label": "{label}",
          "content_type": null,
          "value": "example value",
          "last_modified": "2017-12-05T02:41:26.4874615+00:00",
          "tags": []
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>分页

如果返回的项数超过响应限制，则会对结果进行分页。 遵循可选的 ``Link`` 响应标头并用于 ``rel="next"`` 导航。  或者，内容提供了属性的下一个链接 ``@nextLink`` 。

```http
GET /revisions?api-version={api-version} HTTP/1.1
```

**回复**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.kvs+json; charset=utf-8
Accept-Ranges: items
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

## <a name="list-subset-of-revisions"></a>列出修订部分

使用 `Range` 请求标头。 响应将包含一个 `Content-Range` 标头。 如果服务器无法满足所请求的范围，则它将通过 HTTP `416` (RangeNotSatisfiable 进行响应) 

```http
GET /revisions?api-version={api-version} HTTP/1.1
Range: items=0-2
```

**响应**

```http
HTTP/1.1 206 Partial Content
Content-Type: application/vnd.microsoft.appconfig.revs+json; charset=utf-8
Content-Range: items 0-2/80
```

## <a name="filtering"></a>Filtering

`key`支持和筛选的组合 `label` 。
使用可选的 `key` 和 `label` 查询字符串参数。

```http
GET /revisions?key={key}&label={label}&api-version={api-version}
```

### <a name="supported-filters"></a>支持的筛选器

|密钥筛选器|效果|
|--|--|
|省略 `key` 或 `key=*`|匹配 **任意** 密钥|
|`key=abc`|匹配名为 **abc** 的密钥|
|`key=abc*`|匹配以 **abc** 开头的密钥名称|
|`key=*abc`|匹配以 **abc** 结尾的密钥名称|
|`key=*abc*`|匹配包含 **abc** 的密钥名称|
|`key=abc,xyz`|匹配密钥名称 **abc** 或 **xyz** (限制为5个 CSV) |

|标签筛选器|效果|
|--|--|
|省略 `label` 或 `label=`|匹配不带标签的条目|
|`label=*`|匹配 **任意** 标签|
|`label=prod`|匹配标签 **生产**|
|`label=prod*`|匹配以 **生产** 开始的标签|
|`label=*prod`|匹配以 **生产** 结尾的标签|
|`label=*prod*`|匹配包含 **生产** 的标签|
|`label=prod,test`|匹配 (限制为5个 CSV) 的 **生产** 或 **测试** 标签|

### <a name="reserved-characters"></a>保留字符

`*`, `\`, `,`

如果保留字符是值的一部分，则必须使用对其进行转义 `\{Reserved Character}` 。 非保留字符也可以转义。

### <a name="filter-validation"></a>筛选器验证

如果发生筛选器验证错误，响应为 HTTP， `400` 并提供错误详细信息：

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid request parameter '{filter}'",
  "name": "{filter}",
  "detail": "{filter}(2): Invalid character",
  "status": 400
}
```

### <a name="examples"></a>示例

- 全部

    ```http
    GET /revisions
    ```

- 项名称以 **abc** 开头的项

    ```http
    GET /revisions?key=abc*&api-version={api-version}
    ```

- 项名称为 **abc** 或 **xyz** 并且标签包含 **生产** 的项

    ```http
    GET /revisions?key=abc,xyz&label=*prod*&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>请求特定字段

使用可选的 `$select` 查询字符串参数，并提供以逗号分隔的请求字段列表。 如果 `$select` 省略该参数，则响应将包含默认设置。

```http
GET /revisions?$select=value,label,last_modified&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Time-Based 访问

获取结果的表示形式，即过去的时间。 请参阅[2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1)部分

```http
GET /revisions?api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**回复**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.revs+json"
Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT
Link: <{relative uri}>; rel="original"
```

```json
{
    "items": [
        ....
    ]
}
```
