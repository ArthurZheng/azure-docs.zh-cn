---
title: Azure Cosmos DB 查询语言中的 TAN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TAN。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d7187ba116067445e835769fc33aa70677ef80b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "78301975"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 返回在指定表达式中以弧度表示的指定角度的正切。  
  
## <a name="syntax"></a>语法
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>参数
  
*numeric_expr*  
   是一个数值表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回一个数值表达式。  
  
## <a name="examples"></a>示例
  
  以下示例计算 PI()/2 的正切。  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 下面是结果集。  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)
