---
title: Azure Cosmos DB 查询语言中的 StringToNumber
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 StringToNumber。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5ca8d0c4a6d244823dda6f0f79a3cf5c743a12a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "78296416"
---
# <a name="stringtonumber-azure-cosmos-db"></a>StringToNumber (Azure Cosmos DB)
 返回已转换为数字值的表达式。 如果表达式无法转换，则返回未定义的表达式。  
  
## <a name="syntax"></a>语法
  
```sql
StringToNumber(<str_expr>)  
```  
  
## <a name="arguments"></a>参数
  
*str_expr*  
   是要解析为 JSON Number 表达式的字符串表达式。 JSON 中的数字必须是整数或浮点数。 有关 JSON 格式的详细信息，请参阅 [json.org](https://json.org/)  
  
## <a name="return-types"></a>返回类型
  
  返回一个数字表达式或未定义的表达式。  
  
## <a name="examples"></a>示例
  
  以下示例演示 `StringToNumber` 在不同类型中的行为方式。 

只能在 Number 之前或之后使用空格。

```sql
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 下面是结果集。  
  
```json
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

在 JSON 中，有效的 Number 必须是整数或浮点数。

```sql
SELECT   
    StringToNumber("0xF")
```  
  
 下面是结果集。  
  
```json
{{}}
```  

传递的表达式将会解析为 Number 表达式；以下输入不会计算为 Number 类型，因此会返回未定义。 

```sql
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 下面是结果集。  
  
```json
{{}}
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)
