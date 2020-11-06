---
title: Azure 应用配置 REST API-标头
description: 用于 Azure 应用配置的标头的参考页面 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 5e1f92c68004d0197391ab72df775913c0940fec
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423974"
---
# <a name="headers"></a>标头

本文提供了指向用于 Azure 应用配置 REST API 的标头的参考页的链接。

## <a name="request-headers"></a>请求标头

下表描述 Azure 应用配置中使用的常见请求标头。

| 标头 | 说明 | 示例 |
|--|--|--|
| **授权** | 用于对服务的请求 [进行身份验证](./rest-api-authentication-index.md) 。 请参阅 [14.8 节](https://tools.ietf.org/html/rfc2616#section-14.8) | `Authorization: HMAC-SHA256 Credential=<Credential>&SignedHeaders=Host;x-ms-date;x-ms-content-sha256&Signature=<Signature>` |
| **接受** | 通知服务器客户端将在 HTTP 响应中接受的媒体类型。 请参阅 [14.1 节](https://tools.ietf.org/html/rfc2616#section-14.1) | `Accept: application/vnd.microsoft.appconfig.kv+json;` |
| **Accept-Datetime** | 请求服务器将其内容作为之前状态的表示形式返回。 此标头的值是该状态的请求日期时间。 请参阅 [RFC 7089](https://tools.ietf.org/html/rfc7089#section-2.1.1) | `Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT` |
| **Content-Type** | 包含 HTTP 请求正文中内容的媒体类型。 请参阅 [14.17 节](https://tools.ietf.org/html/rfc2616#section-14.17) | `Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8;` |
| **日期** | 发出 HTTP 请求的日期时间。 此标头用于 [HMAC 身份验证](./rest-api-authentication-hmac.md)。 请参阅 [14.18 节](https://tools.ietf.org/html/rfc2616#section-14.18) | `Date: Fri, 11 May 2018 18:48:36 GMT` |
| **主机** | 指定为其发出请求的租户。 此标头用于 [HMAC 身份验证](./rest-api-authentication-hmac.md)。 请参阅 [14.23 节](https://tools.ietf.org/html/rfc2616#section-14.23) | `Host: contoso.azconfig.io` |
| **If-Match** | 用于发出 HTTP 请求条件。 仅当目标资源的 ETag 与此标头的值匹配时，此请求才会成功。 "*" 值匹配任何 ETag。 请参阅 [14.24 节](https://tools.ietf.org/html/rfc2616#section-14.24) | `If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **If-None-Match** | 用于发出 HTTP 请求条件。 仅当目标资源的 ETag 与此标头的值不匹配时，此请求才应成功。 "*" 值匹配任何 ETag。 请参阅 [14.26 节](https://tools.ietf.org/html/rfc2616#section-14.26) | `If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **同步-令牌** | 用于在请求序列期间启用实时一致性。 | `Sync-Token: jtqGc1I4=MDoyOA==;sn=28` |
| **x-ms-client-request-id** | 客户端提供的唯一 ID，用于跟踪请求的往返。 | `x-ms-client-request-id: 00000000-0000-0000-0000-000000000000` |
| **x-ms-内容-sha256** | HTTP 请求正文的 sha256 摘要。 此标头用于 [HMAC 身份验证](./rest-api-authentication-hmac.md)。 | `x-ms-content-sha256: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=` |
| **x-ms-date** | 如果无法访问日期标头，则可以设置此标头，并将其用于替代 `Date` 标头。 此标头用于 [HMAC 身份验证](./rest-api-authentication-hmac.md)。 | `x-ms-date: Fri, 11 May 2018 18:48:36 GMT` |
| **x-ms-return-client-request-id** | 与标头结合使用 `x-ms-client-request-id` 。 如果此标头的值为 "true"，则将指示服务器返回 `x-ms-client-request-id` 请求标头的值。 | `x-ms-return-client-request-id: true` |

## <a name="response-headers"></a>响应标头

服务器的响应中可能包含以下 HTTP 标头。

| 标头 | 说明 | 示例 |
|--|--|--|
| **Content-Type** | 包含 HTTP 响应正文中内容的媒体类型。 请参阅 [14.17 节](https://tools.ietf.org/html/rfc2616#section-14.17) | `Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8;` |
| **ETag** | 一个不透明的标记，表示给定资源的状态。 可用于条件运算。 请参阅 [14.19 节](https://tools.ietf.org/html/rfc2616#section-14.19) | `ETag: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **上次修改时间** | 描述上次修改请求的资源的时间。 设置为 [HTTP 日期](https://tools.ietf.org/html/rfc2616#section-3.3.1)格式。 请参阅 [14.29 节](https://tools.ietf.org/html/rfc2616#section-14.29) | `Last-Modified: Tue, 05 Dec 2017 02:41:26 GMT` |
| **链接** | 提供指向与响应相关的资源的链接。 此标头用于通过使用 " _下一步_ " 链接进行分页。 请参阅 [RFC 5988](https://tools.ietf.org/html/rfc5988) | `Link: </kv?after={token}>; rel="next"` |
| **名册-Datetime** | 指示响应中包含的内容表示以前的状态。 此标头的值是该状态的日期时间。 请参阅 [RFC 7089](https://tools.ietf.org/html/rfc7089#section-2.1.1) | `Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT` |
| **稍后重试** | 提供客户端在重试失败请求之前要等待的建议时间段（以毫秒为单位） () 。 | `retry-after-ms: 10` |
| **x-ms-request-id** | 服务器生成的唯一 ID，用于跟踪服务中的请求。 | `x-ms-request-id: 00000000-0000-0000-0000-000000000000` |
| **WWW-Authenticate** | 用于质询客户端进行身份验证，并提供有关身份验证尝试失败的原因。 请参阅 [14.47 节](https://tools.ietf.org/html/rfc2616#section-14.47) | `WWW-Authenticate: HMAC-SHA256 error="invalid_token" error_description="Invalid Signature"` |
