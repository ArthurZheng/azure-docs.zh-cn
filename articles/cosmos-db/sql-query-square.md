---
title: Azure Cosmos DB 查询语言中的 SQUARE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 SQUARE。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a22c4daaf9df889f2256bc78f2175c966d4841f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "78303437"
---
# <a name="square-azure-cosmos-db"></a>SQUARE (Azure Cosmos DB)
 返回指定数字值的平方。  
  
## <a name="syntax"></a>语法
  
```sql
SQUARE(<numeric_expr>)  
```  
  
## <a name="arguments"></a>参数
  
*numeric_expr*  
   是一个数值表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回一个数值表达式。  
  
## <a name="examples"></a>示例
  
  以下示例返回数字 1-3 的平方。  
  
```sql
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 下面是结果集。  
  
```json
[{s1: 1, s2: 4, s3: 9}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)
