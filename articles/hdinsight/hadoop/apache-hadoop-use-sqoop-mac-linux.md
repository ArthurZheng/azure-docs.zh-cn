---
title: 将 Apache Sqoop 与 Apache Hadoop 配合使用 - Azure HDInsight
description: 了解如何使用 Apache Sqoop 在 HDInsight 和 Azure SQL 数据库 Apache Hadoop 之间导入和导出。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: 0761ea059350369a363ee1022b21c9da2702b396
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "86076107"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-azure-sql-database"></a>使用 Apache Sqoop 在 HDInsight 和 Azure SQL 数据库 Apache Hadoop 之间导入和导出数据

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用 Apache Sqoop 在 Azure HDInsight 中的 Apache Hadoop 群集与 Azure SQL 数据库或 Microsoft SQL Server 之间进行导入和导出。 本文档中的步骤直接从 Hadoop 群集的头节点使用 `sqoop` 命令。 可以使用 SSH 连接到头节点并运行本文档中的命令。 本文是[在 HDInsight 中将 Apache Sqoop 与 Hadoop 配合使用](./hdinsight-use-sqoop.md)一文的延续。

## <a name="prerequisites"></a>先决条件

* 从[在 HDInsight 中将 Apache Sqoop 与 Hadoop 配合使用](./hdinsight-use-sqoop.md)中完成[设置测试环境](./hdinsight-use-sqoop.md#create-cluster-and-sql-database)。

* SSH 客户端。 有关详细信息，请参阅[使用 SSH 连接到 HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md)。

* 熟悉 Sqoop。 有关详细信息，请参阅 [Sqoop 用户指南](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html)。

## <a name="set-up"></a>设置

1. 使用 [ssh 命令](../hdinsight-hadoop-linux-use-ssh-unix.md)连接到群集。 编辑以下命令（将 CLUSTERNAME 替换为群集的名称），然后输入该命令：

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. 为了便于使用，请设置变量。 将 `PASSWORD`、`MYSQLSERVER` 和 `MYDATABASE` 替换为相关值，然后输入以下命令：

    ```bash
    export password='PASSWORD'
    export sqlserver="MYSQLSERVER"
    export database="MYDATABASE"


    export serverConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password"
    export serverDbConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password;database=$database"
    ```

## <a name="sqoop-export"></a>Sqoop 导出

从 Hive 到 SQL。

1. 若要验证 Sqoop 是否可以看到数据库，请在打开的 SSH 连接中输入以下命令。 此命令会返回数据库列表。

    ```bash
    sqoop list-databases --connect $serverConnect
    ```

1. 输入以下命令以查看指定数据库的表列表：

    ```bash
    sqoop list-tables --connect $serverDbConnect
    ```

1. 若要将 Hive 表中的数据导出 `hivesampletable` 到 `mobiledata` 数据库中的表，请在打开的 SSH 连接中输入以下命令：

    ```bash
    sqoop export --connect $serverDbConnect \
    -table mobiledata \
    --hcatalog-table hivesampletable
    ```

1. 若要验证数据是否已导出，请通过 SSH 连接使用以下查询来查看导出的数据：

    ```bash
    sqoop eval --connect $serverDbConnect \
    --query "SELECT COUNT(*) from dbo.mobiledata WITH (NOLOCK)"


    sqoop eval --connect $serverDbConnect \
    --query "SELECT TOP(10) * from dbo.mobiledata WITH (NOLOCK)"
    ```

## <a name="sqoop-import"></a>Sqoop 导入

从 SQL 到 Azure 存储。

1. 在打开的 SSH 连接中输入以下命令，将 SQL 中的表中的数据导入 `mobiledata` 到 `wasbs:///tutorials/usesqoop/importeddata` HDInsight 上的目录。 数据中的字段将通过制表符分隔，并且相关行由换行符终止。

    ```bash
    sqoop import --connect $serverDbConnect \
    --table mobiledata \
    --target-dir 'wasb:///tutorials/usesqoop/importeddata' \
    --fields-terminated-by '\t' \
    --lines-terminated-by '\n' -m 1
    ```

1. 此外，还可以指定 Hive 表：

    ```bash
    sqoop import --connect $serverDbConnect \
    --table mobiledata \
    --target-dir 'wasb:///tutorials/usesqoop/importeddata2' \
    --fields-terminated-by '\t' \
    --lines-terminated-by '\n' \
    --create-hive-table \
    --hive-table mobiledata_imported2 \
    --hive-import -m 1
    ```

1. 在导入完成后，请在打开的 SSH 连接中输入以下命令，以列出新目录中的数据：

    ```bash
    hadoop fs -tail /tutorials/usesqoop/importeddata/part-m-00000
    ```

1. 使用 [beeline](./apache-hadoop-use-hive-beeline.md) 验证该表是否已在 Hive 中创建。

    1. 连接

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    1. 每次执行下面的一个查询并查看输出：

        ```hql
        show tables;
        describe mobiledata_imported2;
        SELECT COUNT(*) FROM mobiledata_imported2;
        SELECT * FROM mobiledata_imported2 LIMIT 10;
        ```

    1. 使用 `!exit` 退出 beeline。

## <a name="limitations"></a>限制

* 大容量导出-使用基于 Linux 的 HDInsight 时，用于将数据导出到 SQL 的 Sqoop 连接器不支持大容量插入。

* 批处理 - 在基于 Linux 的 HDInsight 上，如果在执行插入时使用 `-batch` 开关，Sqoop 将进行多次插入而不是批处理插入操作。

## <a name="important-considerations"></a>重要注意事项

* HDInsight 和 SQL Server 必须位于同一 Azure 虚拟网络上。

    有关示例，请参阅[将 HDInsight 连接到本地网络](./../connect-on-premises-network.md)文档。

    有关通过 Azure 虚拟网络使用 HDInsight 的详细信息，请参阅[使用 Azure 虚拟网络扩展 HDInsight](../hdinsight-plan-virtual-network-deployment.md) 文档。 有关 Azure 虚拟网络的详细信息，请参阅[虚拟网络概述](../../virtual-network/virtual-networks-overview.md)文档。

* 必须将 SQL Server 配置为允许 SQL 身份验证。 有关详细信息，请参阅[选择身份验证模式](https://msdn.microsoft.com/ms144284.aspx)文档。

* 可能需要将 SQL Server 配置为接受远程连接。 有关详细信息，请参阅[如何解决 SQL Server 数据库引擎的连接问题](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)文档。

## <a name="next-steps"></a>后续步骤

现在，你已了解了如何使用 Sqoop。 若要了解更多信息，请参阅以下文章：

* [将 Apache Oozie 和 HDInsight 配合使用](../hdinsight-use-oozie-linux-mac.md)：在 Oozie 工作流中使用 Sqoop 操作。
* [使用 HDInsight 分析航班延误数据](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)：使用交互式查询分析航班延误数据，然后使用 Sqoop 将数据导出到 Azure 中的数据库。
* [将数据上传到 HDInsight](../hdinsight-upload-data.md)：了解将数据上传到 HDInsight/Azure Blob 存储的其他方法。
