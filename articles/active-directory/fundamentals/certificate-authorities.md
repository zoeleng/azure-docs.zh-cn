---
title: Azure Active Directory 证书颁发机构
description: Azure 中使用的受信任证书列表
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: baselden
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7cc92c8bfb7dd9cb0328820009828ed0dd22e2a1
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339455"
---
# <a name="certificate-authorities-used-by-azure-active-directory"></a>Azure Active Directory 使用的证书颁发机构

> [!IMPORTANT]
> 此页中的信息仅与显式指定 (Ca) 可接受的证书颁发机构列表的实体相关。 除非没有其他选项，否则应该避免这种做法（称为证书固定）。

尝试通过 TLS/SSL 协议访问 Azure Active Directory (Azure AD) 标识服务的任何实体都将使用下面列出的 Ca 提供的证书。 如果实体信任这些 Ca，则它可能会使用证书来验证标识服务的身份和合法性，并建立安全连接。

证书颁发机构可以分类到根 Ca 和中间 Ca。 通常，根 Ca 具有一个或多个关联的中间 Ca。 本文列出了 Azure AD 标识服务和与其中每个根关联的中间 Ca 时使用的根 Ca。 对于每个 CA，我们都包含统一资源标识符 (Uri) 下载关联的颁发机构信息访问 (AIA) ，证书吊销列表分发点 (CDP) 文件。 如果需要，还可以提供联机证书状态协议 (OCSP) 终结点的 URI。

## <a name="cas-used-in-azure-public-and-azure-us-government-clouds"></a>Azure 公共和 Azure 美国政府云中使用的 CAs

不同的服务可以使用不同的根或中间 Ca。 因此，可能需要下面列出的所有条目。

### <a name="digicert-global-root-g2"></a>DigiCert 全局根 G2


| 根 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - |- |-|-|-|-|
| DigiCert 全局根 G2| 033af1e6a711a 9a0bb2864b11d09fae5| 2013 年 8 月 1 日 <br>2038年1月15日| df3c24f9bfd666761b268 073fe06d1cc8d4f82a4| [AIA](http://cacerts.digicert.com/DigiCertGlobalRootG2.crt)<br>[连续](http://crl3.digicert.com/DigiCertGlobalRootG2.crl) |


#### <a name="associated-intermediate-cas"></a>关联的中间 Ca

| 颁发和中间 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - | 
| Microsoft Azure TLS 发证 CA 01| 0aafa6c5ca63c45141 ea3be1f7c75317| 2020 年 7 月 29 日<br>2024年6月27日| 2f2877c5d778c31e0f29c 7e371df5471bd673173| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2001%20-%20xsign.crt)<br>[连续](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TL%20Issuing%20CA%2001.crl)|
|Microsoft Azure TLS 发证 CA 02| 0c6ae97cced59983 8690a00a9ea53214| 2020 年 7 月 29 日<br>2024年6月27日| e7eea674ca718e3befd 90858e09f8372ad0ae2aa| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2002%20-%20xsign.crt)<br>[连续](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2002.crl) |
| Microsoft Azure TLS 发证 CA 05| 0d7bede97d8209967a 52631b8bdd18bd| 2020 年 7 月 29 日<br>2024年6月27日| 6c3af02e7f269aa73a fd0eff2a88a4a1f04ed1e5| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2005%20-%20xsign.crt)<br>[连续](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2005.crl) |
| Microsoft Azure TLS 发证 CA 06| 02e79171fb8021e93fe 2d983834c50c0| 2020 年 7 月 29 日<br>2024年6月27日| 30e01761ab97e59a06b 41ef20af6f2de7ef4f7b0| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20zure%20TLS%20Issuing%20CA%2006%20-%20xsign.crt)<br>[连续](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.crl) |


 ### <a name="baltimore-cybertrust-root"></a>Baltimore CyberTrust 根

| 根 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - | 
| Baltimore CyberTrust 根| 020000b9| 5月12日，2000<br>5月12日，2025| d4de20d05e66fc53fe 1a50882c78db2852cae474|<br>[连续](http://crl3.digicert.com/Omniroot2025.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>关联的中间 Ca

| 颁发和中间 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - | 
| Microsoft RSA TLS CA 01| 703d7a8f0ebf55aaa 59f98eaf4a206004eb2516a| 2020 年 7 月 21 日<br>2024年10月8日| 417e225037fbfaa4f9 5761d5ae729e1aea7e3a42| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2001.crt)<br>[连续](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2001.crl)<br>[OCSP](https://ocsp.msocsp.com/) |
| Microsoft RSA TLS CA 02| b0c2d2d13cdd56cdaa 6ab6e2c04440be4a429c75| 2020 年 7 月 21 日<br>5月20日2024| 54d9d20239080c32316ed 9ff980a48988f4adf2d| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2002.crt)<br>[连续](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2002.crl)<br>[OCSP](https://ocsp.msocsp.com/) |


 ### <a name="digicert-global-root-ca"></a>DigiCert 全局根 CA

| 根 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - | 
| DigiCert 全局根 CA| 083be056904246 b1a1756ac95991c74a| 2006年11月9日<br>2031年11月9日| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| [连续](http://crl3.digicert.com/DigiCertGlobalRootCA.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>关联的中间 Ca

| 颁发和中间 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - |
| DigiCert SHA2 安全服务器 CA| 01fda3eb6eca75c 888438b724bcfbc91| 3月8日2013，2023| 1fb86b1168ec743154062 e8c9cc5b171a4b7ccb4| [AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt)<br>[连续](http://crl3.digicert.com/ssca-sha2-g6.crl)<br>[OCSP](http://ocsp.digicert.com/) |
| DigiCert SHA2 安全服务器 CA |02742eaa17ca8e21 c717bb1ffcfd0ca0 |2020 年 9 月 22 日<br>2030年9月22日|626d44e704d1ceabe3bf 0d53397464ac8080142c|[AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA-2.crt)<br>[连续](http://crl3.digicert.com/DigiCertSHA2SecureServerCA.crl)<br>[OCSP](http://ocsp.digicert.com/)|


## <a name="cas-used-in-azure-china-21vianet-cloud"></a>Azure 中国世纪互联云中使用的 CAs

### <a name="digicert-global-root-ca"></a>DigiCert 全局根 CA


| 根 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - |
| DigiCert 全局根 CA| 083be056904246b 1a1756ac95991c74a| 11月9日，2006<br>11月9日，2031| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| [连续](http://ocsp.digicert.com/)<br>[OCSP](http://crl3.digicert.com/DigiCertGlobalRootCA.crl) |


#### <a name="associated-intermediate-ca"></a>关联的中间 CA

| 颁发和中间 CA| 序列号| 颁发日期到期日期| SHA1 指纹| URI |
| - | - | - | - | - | - |
| DigiCert 基本 RSA CN CA G2| 02f7e1f982bad 009aff47dc95741b2f6| 2020年3月4日<br>2030年3月4日| 4d1fa5d1fb1ac3917c08e 43f65015e6aea571179| [AIA](http://cacerts.digicert.cn/DigiCertBasicRSACNCAG2.crt)<br>[连续](http://crl.digicert.cn/DigiCertBasicRSACNCAG2.crl)<br>[OCSP](http://ocsp.digicert.cn/) |

## <a name="next-steps"></a>后续步骤
[了解 Microsoft 365 加密链](https://docs.microsoft.com/microsoft-365/compliance/encryption-office-365-certificate-chains?view=o365-worldwide)
