---
title: Azure Cosmos DB 查询语言中的 PI
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 PI。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5f2aa78b866b148f59ee3dcfccd351eb51e46526
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93089145"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 返回 PI 的常量值。  
  
## <a name="syntax"></a>语法
  
```sql
PI ()  
```  
   
## <a name="return-types"></a>返回类型
  
  返回数值表达式。  
  
## <a name="examples"></a>示例
  
  下面的示例返回 `PI` 的值。  
  
```sql
SELECT PI() AS pi 
```  
  
 下面是结果集：  
  
```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)
