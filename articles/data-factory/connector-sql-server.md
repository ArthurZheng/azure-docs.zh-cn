---
title: 向/从 SQL Server 复制数据
description: 了解如何使用 Azure 数据工厂将数据移入和移出本地或 Azure VM 中的 SQL Server 数据库。
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/21/2020
ms.openlocfilehash: 255c89a0944abb17ba18cbc5c651d3a3be67892d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91331993"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>使用 Azure 数据工厂向/从 SQL Server 复制数据

> [!div class="op_single_selector" title1="选择要使用的 Azure 数据工厂的版本："]
> * [版本 1](v1/data-factory-sqlserver-connector.md)
> * [当前版本](connector-sql-server.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文概述了如何使用 Azure 数据工厂中的复制活动从/向 SQL Server 数据库复制数据。 本文基于总体概述复制活动的[复制活动概述](copy-activity-overview.md)一文。

## <a name="supported-capabilities"></a>支持的功能

以下活动支持此 SQL Server 连接器：

- 带有[支持的源或接收器矩阵](copy-activity-overview.md)的[复制活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [GetMetadata 活动](control-flow-get-metadata-activity.md)

可以将数据从 SQL Server 数据库复制到任何支持的接收器数据存储。 或者，可将数据从任何支持的源数据存储复制到 SQL Server 数据库。 有关复制活动支持作为源或接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

具体而言，此 SQL Server 连接器支持：

- SQL Server 版本 2005 及更高版本。
- 使用 SQL 或 Windows 身份验证复制数据。
- 作为源，使用 SQL 查询或存储过程检索数据。 还可以选择从 SQL Server 源进行并行复制，有关详细信息，请参阅 [从 SQL 数据库并行复制](#parallel-copy-from-sql-database) 部分。
- 作为接收器，根据源架构自动创建目标表（如果不存在）；在复制过程中，将数据追加到表或使用自定义逻辑调用存储过程。 

[SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb) 不受支持。

>[!NOTE]
>此连接器目前不支持 SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)。 为了解决此问题，可以使用[通用 ODBC 连接器](connector-odbc.md)和 SQL Server ODBC 驱动程序。 按照[此指南](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver)完成 ODBC 驱动程序下载和连接字符串配置。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 SQL Server 数据库连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

SQL Server 链接服务支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 **SqlServer**。 | 是 |
| connectionString |指定使用 SQL 身份验证或 Windows 身份验证连接到 SQL Server 数据库时所需的 **connectionString** 信息。 请参阅以下示例。<br/>还可以在 Azure Key Vault 中输入密码。 如果使用 SQL 身份验证，请从连接字符串中提取 `password` 配置。 有关详细信息，请参阅表格后面的 JSON 示例，以及[在 Azure Key Vault 中存储凭据](store-credentials-in-key-vault.md)。 |是 |
| userName |如果使用 Windows 身份验证，请指定用户名。 例如，**domainname\\username**。 |否 |
| password |指定为用户名指定的用户帐户的密码。 将此字段标记为 **SecureString**，以便安全地将其存储在 Azure 数据工厂中。 或者，可以[引用 Azure Key Vault 中存储的机密](store-credentials-in-key-vault.md)。 |否 |
| connectVia | 此[集成运行时](concepts-integration-runtime.md)用于连接到数据存储。 从[先决条件](#prerequisites)部分了解更多信息。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

>[!TIP]
>如果遇到错误（错误代码为“UserErrorFailedToConnectToSqlServer”，且消息如“数据库的会话限制为 XXX 且已达到。”），请将 `Pooling=false` 添加到连接字符串中，然后重试。

**示例 1：使用 SQL 身份验证**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**示例 2：将 SQL 身份验证与 Azure Key Vault 中的密码结合使用**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**示例 3：使用 Windows 身份验证**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;",
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
     }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供 SQL Server 数据集支持的属性列表。

从/向 SQL Server 数据库复制数据时支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为 SqlServerTable。 | 是 |
| 架构 | 架构的名称。 |对于源为“No”，对于接收器为“Yes”  |
| 表 | 表/视图的名称。 |对于源为“No”，对于接收器为“Yes”  |
| tableName | 具有架构的表/视图的名称。 此属性支持后向兼容性。 对于新的工作负荷，请使用 `schema` 和 `table`。 | 对于源为“No”，对于接收器为“Yes” |

**示例**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的节和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 SQL Server 源和接收器支持的属性列表。

### <a name="sql-server-as-a-source"></a>SQL Server 作为源

>[!TIP]
>若要使用数据分区有效地从 SQL Server 中加载数据，请从 [SQL 数据库中的并行复制](#parallel-copy-from-sql-database)中了解详细信息。

要从 SQL Server 复制数据，请将复制活动中的源类型设置为 SqlSource。 复制活动的 source 节支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 复制活动 source 节的 type 属性必须设置为 SqlSource。 | 是 |
| sqlReaderQuery |使用自定义 SQL 查询读取数据。 例如 `select * from MyTable`。 |否 |
| sqlReaderStoredProcedureName |此属性是从源表读取数据的存储过程的名称。 最后一个 SQL 语句必须是存储过程中的 SELECT 语句。 |否 |
| storedProcedureParameters |这些参数用于存储过程。<br/>允许的值为名称或值对。 参数的名称和大小写必须与存储过程参数的名称和大小写匹配。 |否 |
| isolationLevel | 指定 SQL 源的事务锁定行为。 允许的值为：ReadCommitted、ReadUncommitted、RepeatableRead、Serializable、Snapshot    。 如果未指定，则使用数据库的默认隔离级别。 请参阅[此文档](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel)了解更多详细信息。 | 否 |
| partitionOptions | 指定用于从 SQL Server 加载数据的数据分区选项。 <br>允许的值为： **None** (默认值) 、 **PhysicalPartitionsOfTable**和 **DynamicRange**。<br>启用分区选项后 (即不 `None`) ，从 SQL Server 中并发加载数据的并行度由 [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) 复制活动的设置控制。 | 否 |
| partitionSettings | 指定数据分区的设置组。 <br>当分区选项不是 `None` 时适用。 | 否 |
| 在 `partitionSettings` 下： | | |
| partitionColumnName | 指定源列的名称，此名称 **以整数或日期/日期/时间类型** ，用于并行复制的范围分区。 如果未指定，系统会自动检测表的索引或主键并将其用作分区列。<br>当分区选项是 `DynamicRange` 时适用。 如果使用查询来检索源数据，请在 WHERE 子句中挂接 `?AdfDynamicRangePartitionCondition `。 有关示例，请参阅 [从 SQL 数据库并行复制](#parallel-copy-from-sql-database) 部分。 | 否 |
| partitionUpperBound | 分区范围拆分的分区列的最大值。 此值用于决定分区跨距，而不是用于筛选表中的行。 将对表或查询结果中的所有行进行分区和复制。 如果未指定，则复制活动会自动检测值。  <br>当分区选项是 `DynamicRange` 时适用。 有关示例，请参阅 [从 SQL 数据库并行复制](#parallel-copy-from-sql-database) 部分。 | 否 |
| partitionLowerBound | 分区范围拆分的分区列的最小值。 此值用于决定分区跨距，而不是用于筛选表中的行。 将对表或查询结果中的所有行进行分区和复制。 如果未指定，则复制活动会自动检测值。<br>当分区选项是 `DynamicRange` 时适用。 有关示例，请参阅 [从 SQL 数据库并行复制](#parallel-copy-from-sql-database) 部分。 | 否 |

**需要注意的要点：**

- 如果为 **SqlSource** 指定 **sqlReaderQuery**，则复制活动针对 SQL Server 源运行此查询以获取数据。 也可通过指定 sqlReaderStoredProcedureName 和 storedProcedureParameters 来指定存储过程，前提是存储过程使用参数 。
- 如果不指定 **sqlReaderQuery** 或 **sqlReaderStoredProcedureName**，则数据集 JSON 的“structure”节中定义的列用于构建查询。 查询 `select column1, column2 from mytable` 针对 SQL Server 运行。 如果数据集定义没有“structure”，则会从表中选择所有列。

**示例：使用 SQL 查询**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**示例：使用存储过程**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**存储过程定义**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sql-server-as-a-sink"></a>SQL Server 作为接收器

> [!TIP]
> 若要详细了解支持的写入行为、配置和最佳做法，请参阅[将数据加载到 SQL Server 中的最佳做法](#best-practice-for-loading-data-into-sql-server)。

要向 SQL Server 复制数据，请将复制活动中的接收器类型设置为 SqlSink。 复制活动的 sink 节支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | 复制活动的 sink 的 type 属性必须设置为 SqlSink。 | 是 |
| preCopyScript |此属性指定将数据写入到 SQL Server 中之前复制活动要运行的 SQL 查询。 每次运行复制仅调用该查询一次。 可以使用此属性清除预加载的数据。 |否 |
| tableOption | 指定是否根据源架构[自动创建接收器表](copy-activity-overview.md#auto-create-sink-tables)（如果不存在）。 接收器指定存储过程时不支持自动创建表。 允许的值为：`none`（默认值）、`autoCreate`。 |否 |
| sqlWriterStoredProcedureName | 定义如何将源数据应用于目标表的存储过程的名称。 <br/>此存储过程由每个批处理调用。 若要执行仅运行一次且与源数据无关的操作（例如删除或截断），请使用 `preCopyScript` 属性。<br>请参阅[调用 SQL 接收器的存储过程](#invoke-a-stored-procedure-from-a-sql-sink)中的示例。 | 否 |
| storedProcedureTableTypeParameterName |存储过程中指定的表类型的参数名称。  |否 |
| sqlWriterTableType |要在存储过程中使用的表类型名称。 通过复制活动，使移动数据在具备此表类型的临时表中可用。 然后，存储过程代码可合并复制数据和现有数据。 |否 |
| storedProcedureParameters |存储过程的参数。<br/>允许的值为名称和值对。 参数的名称和大小写必须与存储过程参数的名称和大小写匹配。 | 否 |
| writeBatchSize |每批要插入到 SQL 表中的行数。<br/>允许的值为表示行数的整数。 默认情况下，Azure 数据工厂会根据行大小动态确定适当的批大小。 |否 |
| writeBatchTimeout |此属性指定超时前等待批插入操作完成的时间。<br/>允许的值是指时间跨度。 例如，“00:30:00”表示 30 分钟。 如果未指定值，则超时默认为“02:00:00”。 |否 |

**示例 1：追加数据**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**示例 2：在复制过程中调用存储过程**

请参阅[调用 SQL 接收器的存储过程](#invoke-a-stored-procedure-from-a-sql-sink)，了解更多详细信息。

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="parallel-copy-from-sql-database"></a>从 SQL 数据库并行复制

"复制" 活动中的 "SQL Server" 连接器提供内置数据分区以并行复制数据。 可以在复制活动的“源”表中找到数据分区选项。 

![分区选项的屏幕截图](./media/connector-sql-server/connector-sql-partition-options.png)

启用分区复制时，复制活动对 SQL Server 源运行并行查询以按分区加载数据。 可通过复制活动中的 [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) 设置控制并行度。 例如，如果将设置 `parallelCopies` 为4，则数据工厂会同时生成并运行基于指定分区选项和设置的四个查询，每个查询将从您的 SQL Server 检索部分数据。

建议你在使用数据分区时启用并行复制，尤其是从 SQL Server 加载大量数据时。 下面是适用于不同方案的建议配置。 将数据复制到基于文件的数据存储中时，建议将数据作为多个文件写入文件夹（仅指定文件夹名称），在这种情况下，性能优于写入单个文件。

| 方案                                                     | 建议的设置                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 从包含物理分区的大型表进行完整加载。        | **分区选项**：表的物理分区。 <br><br/>在执行期间，数据工厂将自动检测物理分区并按分区复制数据。 <br><br/>若要检查表是否具有物理分区，可以引用 [此查询](#sample-query-to-check-physical-partition)。 |
| 完全加载大型表，没有物理分区，而使用整数或日期时间列进行数据分区。 | **分区选项**：动态范围分区。<br>**分区列** (可选) ：指定用于对数据进行分区的列。 如果未指定，则使用索引或主键列。<br/>**分区上限** 和 **分区下限** (可选) ：指定是否要确定分区跨距。 这不适用于筛选表中的行，表中的所有行都将进行分区和复制。 如果未指定，则复制活动会自动检测值。<br><br>例如，如果分区列 "ID" 的值的范围是从1到100，并将下限设置为20，将上限设置为80，并将 "并行复制" 设置为 "4"，则数据工厂会分别在范围 <= 20、[21、50]、[51、80] 和 >= 81 中通过4个分区 Id 检索数据。 |
| 使用自定义查询，无需物理分区即可加载大量数据，而使用整数或日期/日期时间列进行数据分区。 | **分区选项**：动态范围分区。<br>**查询**：`SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`。<br>**分区列**：指定用于对数据进行分区的列。<br>**分区上限** 和 **分区下限** (可选) ：指定是否要确定分区跨距。 这不适用于筛选表中的行，查询结果中的所有行都将进行分区和复制。 如果未指定，则复制活动会自动检测值。<br><br>在执行期间，数据工厂将替换为 `?AdfRangePartitionColumnName` 每个分区的实际列名称和值范围，并将发送到 SQL Server。 <br>例如，如果分区列 "ID" 的值的范围是从1到100，并将下限设置为20，将上限设置为80，并将 "并行复制" 设置为 "4"，则数据工厂会分别在范围 <= 20、[21、50]、[51、80] 和 >= 81 中通过4个分区 Id 检索数据。 <br><br>下面是针对不同方案的更多示例查询：<br> 1. 查询整个表： <br>`SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition`<br> 2. 从具有列选择和其他 where 子句筛选器的表中查询： <br>`SELECT <column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 3. 查询与子查询： <br>`SELECT <column_list> FROM (<your_sub_query>) AS T WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 4. 在子查询中具有分区的查询： <br>`SELECT <column_list> FROM (SELECT <your_sub_query_column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition) AS T`
|

用 partition 选项加载数据的最佳做法：

1. 选择 "特殊" 列作为分区列 (如 primary key 或 unique key) ，以避免数据歪斜。 
2. 如果表具有内置分区，请使用分区选项 "表的物理分区" 以获得更好的性能。  
3. 如果使用 Azure Integration Runtime 来复制数据，则可以将更大的 "[数据集成单位 (DIU) ](copy-activity-performance-features.md#data-integration-units)" ( # B0 4) 来利用更多计算资源。 检查此处适用的情况。
4. "[复制并行度](copy-activity-performance-features.md#parallel-copy)" 控制分区号，将此数值设置得太大有时会影响性能，建议将此数字设置为 (DIU 或自承载 IR 节点的数量) * (2 到 4) 。

**示例：包含物理分区的大型表中的完全加载**

```json
"source": {
    "type": "SqlSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**示例：使用动态范围分区进行查询**

```json
"source": {
    "type": "SqlSource",
    "query": "SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column (optional) to decide the partition stride, not as data filter>",
        "partitionLowerBound": "<lower_value_of_partition_column (optional) to decide the partition stride, not as data filter>"
    }
}
```

### <a name="sample-query-to-check-physical-partition"></a>用于检查物理分区的示例查询

```sql
SELECT DISTINCT s.name AS SchemaName, t.name AS TableName, pf.name AS PartitionFunctionName, c.name AS ColumnName, iif(pf.name is null, 'no', 'yes') AS HasPartition
FROM sys.tables AS t
LEFT JOIN sys.objects AS o ON t.object_id = o.object_id
LEFT JOIN sys.schemas AS s ON o.schema_id = s.schema_id
LEFT JOIN sys.indexes AS i ON t.object_id = i.object_id 
LEFT JOIN sys.index_columns AS ic ON ic.partition_ordinal > 0 AND ic.index_id = i.index_id AND ic.object_id = t.object_id 
LEFT JOIN sys.columns AS c ON c.object_id = ic.object_id AND c.column_id = ic.column_id 
LEFT JOIN sys.partition_schemes ps ON i.data_space_id = ps.data_space_id 
LEFT JOIN sys.partition_functions pf ON pf.function_id = ps.function_id 
WHERE s.name='[your schema]' AND t.name = '[your table name]'
```

如果表具有物理分区，则会看到如下所示的 "HasPartition"。

![Sql 查询结果](./media/connector-azure-sql-database/sql-query-result.png)

## <a name="best-practice-for-loading-data-into-sql-server"></a>将数据加载到 SQL Server 中的最佳做法

将数据复制到 SQL Server 中时，可能需要不同的写入行为：

- [追加](#append-data)：我的源数据只包含新记录。
- [更新插入](#upsert-data)：我的源数据包含插入和更新内容。
- [覆盖](#overwrite-the-entire-table)：我需要每次都重新加载整个维度表。
- [使用自定义逻辑进行写入](#write-data-with-custom-logic)：在将数据最终插入目标表之前，我需要额外的处理。

有关如何在 Azure 数据工厂中进行配置和最佳做法，请参阅相应的部分。

### <a name="append-data"></a>追加数据

追加数据是此 SQL Server 接收器连接器的默认行为。 Azure 数据工厂执行批量插入，以有效地在表中写入数据。 可以相应地在复制活动中配置源和接收器。

### <a name="upsert-data"></a>更新插入数据

选项 1：当需要复制大量数据时，可以使用复制活动将所有记录大容量加载到一个临时表中，然后运行存储过程活动来一次性应用 [MERGE](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql) 或 INSERT/UPDATE 语句。 

复制活动当前并非原生支持将数据加载到数据库临时表中。 有一种结合多种活动进行设置的高级方法，请参阅[优化 SQL 数据库批量更新插入方案](https://github.com/scoriani/azuresqlbulkupsert)。 下面显示了使用永久表作为暂存的示例。

例如，在 Azure 数据工厂中，可以使用**复制活动**创建一个管道，并将其与**存储过程活动**相链接。 前者将数据从源存储复制到数据集中的 SQL Server 临时表（例如，表名为“UpsertStagingTable”的表）。 然后，后者调用一个存储过程，以将临时表中的源数据合并到目标表中，并清理临时表。

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

在数据库中使用 MERGE 逻辑定义一个存储过程（如以下示例所示），以便从上述存储过程活动指向该过程。 假设目标是包含三个列的 **Marketing** 表：**ProfileID**、**State** 和 **Category**。 根据 **ProfileID** 列执行更新插入。

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING UpsertStagingTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE UpsertStagingTable
END
```

选项 2：可选择[在复制活动中调用存储过程](#invoke-a-stored-procedure-from-a-sql-sink)。 这种方法运行源表中的每个批（由 `writeBatchSize` 属性控制），而不是在复制活动中使用批量插入作为默认方法。

### <a name="overwrite-the-entire-table"></a>覆盖整个表

可以在复制活动接收器中配置 **preCopyScript** 属性。 在此情况下，对于运行的每个复制活动，Azure 数据工厂会先运行脚本。 然后，运行复制来插入数据。 例如，若要使用最新数据覆盖整个表，请指定一个脚本，以先删除所有记录，然后从源批量加载新数据。

### <a name="write-data-with-custom-logic"></a>使用自定义逻辑写入数据

使用自定义逻辑写入数据的步骤与[更新插入数据](#upsert-data)部分中的描述类似。 如果在将源数据最终插入目标表之前需要应用额外的处理，则可先将数据加载到临时表，然后再调用存储过程活动，或者在复制活动接收器中调用存储过程来应用数据。

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a> 调用 SQL 接收器的存储过程

将数据复制到 SQL Server 数据库中时，还可以通过对每批源表使用附加参数来配置和调用用户指定的存储过程。 存储过程功能利用[表值参数](https://msdn.microsoft.com/library/bb675163.aspx)。

当内置复制机制无法使用时，还可使用存储过程。 例如，在将源数据最终插入目标表之前应用额外的处理。 额外处理的示例包括合并列、查找其他值以及将数据插入多个表。

以下示例演示如何使用存储过程，在 SQL Server 数据库中的表内执行 upsert。 假设输入数据和接收器 **Marketing** 表各有三列：**ProfileID**、**State** 和 **Category**。 基于 ProfileID 列执行更新插入，并仅将其应用于名为“ProductA”的特定类别。

1. 在数据库中，使用与 **sqlWriterTableType** 相同的名称定义表类型。 表类型的架构与输入数据返回的架构相同。

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. 在数据库中，使用与 **sqlWriterStoredProcedureName** 相同的名称定义存储过程。 它可处理来自指定源的输入数据，并将其合并到输出表中。 存储过程中的表类型的参数名称与数据集中定义的 **tableName** 相同。

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. 在 Azure 数据工厂中，在复制活动中定义 **SQL sink** 节，如下所示：

    ```json
    "sink": {
        "type": "SqlSink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="data-type-mapping-for-sql-server"></a>SQL Server 的数据类型映射

从/向 SQL Server 复制数据时，以下映射用于从 SQL Server 数据类型映射到 Azure 数据工厂临时数据类型。 若要了解复制活动如何将源架构和数据类型映射到接收器，请参阅[架构和数据类型映射](copy-activity-schema-and-type-mapping.md)。

| SQL Server 数据类型 | Azure 数据工厂临时数据类型 |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |布尔 |
| char |String, Char[] |
| date |DateTime |
| datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| 小数 |小数 |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |小数 |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |小数 |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |小数 |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |String |

>[!NOTE]
> 对于映射到十进制临时类型的数据类型，目前复制活动支持的最大精度为 28。 如果数据需要的精度大于 28，请考虑在 SQL 查询中将其转换为字符串。

## <a name="lookup-activity-properties"></a>Lookup 活动属性

若要了解有关属性的详细信息，请查看 [Lookup 活动](control-flow-lookup-activity.md)。

## <a name="getmetadata-activity-properties"></a>GetMetadata 活动属性

若要了解有关属性的详细信息，请查看 [GetMetadata 活动](control-flow-get-metadata-activity.md) 

## <a name="using-always-encrypted"></a>使用 Always Encrypted

使用 [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) 从/向 SQL Server 复制数据时，请通过自承载 Integration Runtime 使用[通用 ODBC 连接器](connector-odbc.md)和 SQL Server ODBC 驱动程序。 此 SQL Server 连接器目前不支持 Always Encrypted。 

更具体地说：

1. 安装自承载 Integration Runtime（如果没有）。 有关详细信息，请参阅[自承载集成运行时](create-self-hosted-integration-runtime.md)一文。

2. 从[此处](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)下载适用于 SQL Server 的 64 位 ODBC 驱动程序，并将其安装在 Integration Runtime 计算机上。 若要详细了解此驱动程序的工作原理，请参阅[在适用于 SQL Server 的 ODBC 驱动程序中使用 Always Encrypted](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver#using-the-azure-key-vault-provider)。

3. 创建 ODBC 类型的链接服务以连接到 SQL 数据库。 若要使用 SQL 身份验证，请按如下所示指定 ODBC 连接字符串，并选择“基本”身份验证以设置用户名和密码。

    ```
    Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>
    ```

4. 相应地使用 ODBC 类型创建数据集和复制活动。 若要了解详细信息，请参阅 [ODBC 连接器](connector-odbc.md)一文。

## <a name="troubleshoot-connection-issues"></a>排查连接问题

1. 将 SQL Server 实例配置为接受远程连接。 启动“SQL Server Management Studio”，右键单击“服务器”并选择“属性”  。 从列表中选择“连接”，并选中“允许远程连接到此服务器”复选框 。

    ![启用远程连接](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    有关详细步骤，请参阅[配置远程访问服务器配置选项](https://msdn.microsoft.com/library/ms191464.aspx)。

2. 启动“SQL Server 配置管理器”。 针对所需实例展开“SQL Server 网络配置”，并选择“MSSQLSERVER 的协议” 。 协议将显示在右窗格中。 右键单击“TCP/IP”并选择“启用”以启用 TCP/IP 。

    ![启用 TCP/IP](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    有关详细信息和启用 TCP/IP 协议的其他方法，请参阅[启用或禁用服务器网络协议](https://msdn.microsoft.com/library/ms191294.aspx)。

3. 在同一窗口中，双击“TCP/IP”以启动“TCP/IP 属性”窗口 。
4. 切换到“IP 地址”选项卡。向下滚动到“IPAll”部分。 记下“TCP 端口”的值。 默认值为 **1433**。
5. 在计算机上创建 Windows 防火墙规则，以便允许通过此端口传入流量。 
6. **验证连接**：若要使用完全限定名称连接到 SQL Server，请从另一台计算机使用 SQL Server Management Studio。 例如 `"<machine>.<domain>.corp.<company>.com,1433"`。

## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
