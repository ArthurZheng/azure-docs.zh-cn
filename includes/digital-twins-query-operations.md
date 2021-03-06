---
author: baanders
description: Azure 数字孪生查询操作的包含文件
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 450ea0b8024192f6f351b4893fe6e8e07db236f7
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91931966"
---
## <a name="query-language-features"></a>查询语言功能

Azure 数字孪生针对大容量图形提供丰富的查询功能。 使用类似于 SQL 的语法，使用类似于 SQL 的语法（类似于 [IoT 中心查询语言](../articles/iot-hub/iot-hub-devguide-query-language.md) ，具有许多可比较功能）对查询进行了说明。

> [!NOTE]
> 所有 Azure 数字孪生查询操作都区分大小写。

下面是 Azure 数字孪生查询语言中可用的操作。

获取数字孪生，按 .。。
* 使用 `IS_OF_MODEL` operator)  (模型
* 属性 (包括 [标记属性](../articles/digital-twins/how-to-use-tags.md)) 
* interfaces
* 关系
  - 关系的属性

可以通过以下操作进一步增强查询：
*  (查询) 的多个关系类型上获取孪生 `JOIN` 。 
  - 在预览期间，允许最多五个级别 `JOIN` 。
* 仅选择最上面的查询结果 (`Select TOP` 运算符) 
* 使用计算结果集中的项数 `Select COUNT`
* 使用标量函数： `IS_BOOL` 、 `IS_DEFINED` 、 `IS_NULL` 、 `IS_NUMBER` 、 `IS_OBJECT` 、 `IS_PRIMITIVE` `IS_STRING` `STARTSWITH` `ENDSWITH` 、、和。
* 使用查询比较运算符： `IN` / `NIN` 、 `=` 、 `!=` 、 `<` 、 `>` 、 `<=` 、 `>=` 。
* 使用任意组合 (`AND` 、 `OR` 、 `NOT` 运算符) `IS_OF_MODEL` 、标量函数和比较运算符。
* 使用延续：查询对象的实例化页面大小最多为 100)  (。 可以通过在后续对 API 的调用中提供继续标记，一次检索一页中的数字孪生。