---
title: Data Lake Storage Gen1 门户中创建 Azure HDInsight 群集
description: 在 Azure 门户中创建和使用包含 Azure Data Lake Storage Gen1 的 HDInsight 群集
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8d0ffb008258c586a5965b0741b848f1cef319f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91857051"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-by-using-the-azure-portal"></a>使用 Azure 门户创建包含 Azure Data Lake Storage Gen1 的 HDInsight 群集

> [!div class="op_single_selector"]
> * [使用 Azure 门户](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [使用 PowerShell（对于默认存储）](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [使用 PowerShell（对于附加存储）](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [使用资源管理器](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

了解如何使用 Azure 门户创建 Azure Data Lake Storage Gen1 为默认存储或附加存储的 HDInsight 群集。 尽管其他存储对于 HDInsight 群集是可选的，但建议将业务数据存储在其他存储帐户中。

## <a name="prerequisites"></a>必备条件

在开始之前，请确保满足以下要求：

* **一个 Azure 订阅**。 转到[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
* **Azure Data Lake Storage Gen1 帐户**。 请遵循[通过 Azure 门户开始使用 Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md) 中的说明进行操作。 还必须在该帐户上创建根文件夹。  本文将使用名为 __/clusters__ 的根文件夹。
* **Azure Active Directory 服务主体**。 本操作方法指南提供了有关如何在 Azure Active Directory (Azure AD) 中创建服务主体的说明。 但是，只有 Azure AD 管理员才能创建服务主体。 如果你是管理员，则可以跳过此先决条件，然后继续。

>[!NOTE]
>仅当你是 Azure AD 管理员时，才可以创建服务主体。 Azure AD 管理员必须先创建服务主体，才能创建包含 Data Lake Storage Gen1 的 HDInsight 群集。 此外，必须根据[使用证书创建服务主体](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate)中所述，使用证书创建服务主体。
>

## <a name="create-an-hdinsight-cluster"></a>创建 HDInsight 群集

在本部分中，将创建一个 HDInsight 群集，并将 Data Lake Storage Gen1 作为默认存储或附加存储。 本文仅重点介绍配置 Data Lake Storage Gen1 的部分。 有关创建群集的一般信息和过程，请参阅[在 HDInsight 中创建 Hadoop 群集](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)。

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-default-storage"></a>创建使用 Data Lake Storage Gen1 作为默认存储的群集

若要创建 Data Lake Storage Gen1 作为默认存储帐户的 HDInsight 群集，请执行以下操作：

1. 登录 [Azure 门户](https://portal.azure.com)。
2. 有关创建 HDInsight 群集的一般信息，请遵循[创建群集](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters)。
3. 在“存储”边栏选项卡的“主存储类型”下，选择“Azure Data Lake Storage Gen1”，然后输入以下信息************：

    ![HDInsight 存储帐户设置](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png)

    * 选择 Data Lake Store 帐户：选择一个现有的 Data Lake Storage Gen1 帐户****。 必须具有现有的 Data Lake Storage Gen1 帐户。  请参阅[先决条件](#prerequisites)。
    * **根路径**：输入群集特定文件的存储路径。 在屏幕截图中，它是 __/clusters/myhdiadlcluster/__，其中，__/clusters__ 文件夹是必须存在的，*myhdicluster* 文件夹则由门户创建。  *myhdicluster* 是群集名称。
    * Data Lake Store 访问：配置 Data Lake Storage Gen1 帐户和 HDInsight 群集之间的访问****。 有关说明，请参阅[配置 Data Lake Storage Gen1 访问](#configure-data-lake-storage-gen1-access)。
    * 附加存储帐户：添加 Azure 存储帐户作为群集的附加存储帐户****。 添加额外的 Data Lake Storage Gen1 帐户后，为群集提供对更多 Data Lake Storage Gen1 帐户中数据的权限，并将某个 Data Lake Storage Gen1 帐户配置为主存储类型。 请参阅[配置 Data Lake Storage Gen1 访问](#configure-data-lake-storage-gen1-access)。

4. 在“Data Lake Store 访问”中单击“选择”，并根据[在 HDInsight 中创建 Hadoop 群集](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md)中所述继续创建群集。********

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>创建使用 Data Lake Storage Gen1 作为额外存储的群集

以下说明使用 Azure Blob 存储帐户作为默认存储创建 HDInsight 群集，并使用作为附加存储 Data Lake Storage Gen1 的存储帐户。

使用 Data Lake Storage Gen1 作为附加存储帐户创建 HDInsight 群集：

1. 登录 [Azure 门户](https://portal.azure.com)。
2. 有关创建 HDInsight 群集的一般信息，请遵循[创建群集](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters)。
3. 在“存储”**** 边栏选项卡的“主存储类型”**** 下，选择“Azure 存储”****，然后输入以下信息：

    ![HDInsight 存储帐户设置附加存储](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png)

    * **选择方法** -若要指定属于 Azure 订阅的存储帐户，请选择 " **我的订阅**"，然后选择存储帐户。 要指定不属于 Azure 订阅的存储帐户，请选择“访问密钥”****，并提供该外部存储帐户的信息。

    * **默认容器** -使用默认值或指定自己的名称。
    * **其他存储帐户** -添加更多 Azure 存储帐户作为附加存储。
    * **Data Lake Store 访问** -配置 Data Lake Storage Gen1 帐户和 HDInsight 群集之间的访问权限。 有关说明，请参阅 [配置 Data Lake Storage Gen1 访问](#configure-data-lake-storage-gen1-access)。

## <a name="configure-data-lake-storage-gen1-access"></a>配置 Data Lake Storage Gen1 访问

本部分使用 Azure Active Directory 服务主体从 HDInsight 群集配置 Data Lake Storage Gen1 访问。

### <a name="specify-a-service-principal"></a>指定服务主体

在 Azure 门户中，可以使用现有的服务主体，或创建一个新的服务主体。

若要从 Azure 门户创建服务主体：

1. 从 "存储" 边栏选项卡中选择 **Data Lake Store 访问** 。
1. 在 " **Data Lake Storage Gen1 访问** " 边栏选项卡中，选择 " **新建**"。
1. 选择 " **服务主体**"，然后按照说明创建服务主体。
1. 下载证书（如果决定以后再次使用）。 如要想要在创建其他 HDInsight 群集时使用相同的服务主体，下载证书的做法将非常有用。

    ![将服务主体添加到 HDInsight 群集](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png)

1. 选择 " **访问权限** " 以配置文件夹访问权限。  请参阅[配置文件权限](#configure-file-permissions)。

使用 Azure 门户中的现有服务主体：

1. 选择 **Data Lake Store 访问**。
1. 在 " **Data Lake Storage Gen1 访问** " 边栏选项卡上，选择 " **使用现有**"。
1. 选择 " **服务主体**"，然后选择一个服务主体。
1. 上传与所选服务主体关联的证书（.pfx 文件），然后输入证书密码。

[将服务主体添加到 HDInsight 群集](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png)

1. 选择 " **访问权限** " 以配置文件夹访问权限。  请参阅[配置文件权限](#configure-file-permissions)。

### <a name="configure-file-permissions"></a><a name="configure-file-permissions"></a>配置文件权限

此配置不同，具体取决于该帐户是用作默认存储还是使用其他存储帐户：

* 用作默认存储

  * Data Lake Storage Gen1 帐户根级别的权限
  * HDInsight 群集存储根级别的权限。 例如，本教程前面使用的 __/clusters__ 文件夹。

* 用作附加存储

  * 需要文件访问权限的文件夹上的权限。

若要在根级别 Data Lake Storage Gen1 的存储帐户分配权限，请执行以下操作：

1. 在 " **Data Lake Storage Gen1 访问** " 边栏选项卡中，选择 " **访问**"。 随即打开“选择文件权限”**** 边栏选项卡。 其中列出了订阅中的所有存储帐户。
1. 悬停 (不要单击) 将鼠标悬停在该帐户的名称上，Data Lake Storage Gen1 使复选框可见，然后选中该复选框。

    ![选择文件权限](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png)

   默认情况下，将选择 " __读取__"、" __写入__" 和 " __执行__ "。

1. 单击页面底部的“选择”****。
1. 选择 " **运行** " 分配权限。
1. 选择“完成”。

在 HDInsight 群集根级别分配权限：

1. 在 " **Data Lake Storage Gen1 访问** " 边栏选项卡中，选择 " **访问**"。 随即打开“选择文件权限”**** 边栏选项卡。 其中列出了订阅中具有 Data Lake Storage Gen1 的所有存储帐户。
1. 从 " **选择文件权限** " 边栏选项卡中，选择具有 Data Lake Storage Gen1 名称的存储帐户以显示其内容。
1. 通过选中文件夹左侧的复选框，选择 HDInsight 群集存储根目录。 根据之前的屏幕截图，群集存储根目录是选择 Data Lake Storage Gen1 作为默认存储时指定的 __/clusters__ 文件夹。
1. 设置文件夹上的权限。  “读取”、“写入”和“执行”默认全部选中。
1. 单击页面底部的“选择”****。
1. 选择“运行”。
1. 选择“完成”。

如果使用 Data Lake Storage Gen1 作为额外存储，则必须仅为要从 HDInsight 群集访问的文件夹分配权限。 例如，在下面的屏幕截图中，只提供对 Data Lake Storage Gen1 的存储帐户中 **mynewfolder** 文件夹的访问权限。

![将服务主体权限分配给 HDInsight 群集](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png)

## <a name="verify-cluster-setup"></a><a name="verify-cluster-set-up"></a>验证群集设置

群集设置完成后，在群集边栏选项卡中通过执行以下一个或两个步骤来验证结果：

* 若要验证群集的关联存储是否为具有指定 Data Lake Storage Gen1 的帐户，请在左窗格中选择 " **存储帐户** "。

    ![验证关联的存储](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png)

* 若要验证服务主体是否与 HDInsight 群集正确关联，请在左窗格中选择 " **Data Lake Storage Gen1 访问** "。

    ![验证服务主体](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png)

## <a name="examples"></a>示例

使用 Data Lake Storage Gen1 作为存储设置群集后，请参阅以下示例，了解如何使用 HDInsight 群集来分析存储在 Data Lake Storage Gen1 中的数据。

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-as-primary-storage"></a>对 Data Lake Storage Gen1 中的数据运行 Hive 查询， (作为主存储) 

若要运行 Hive 查询，请在 Ambari 门户中使用 Hive 视图界面。 有关 Ambari Hive 视图用法的说明，请参阅[将 Hive 视图与 HDInsight 中的 Hadoop 配合使用](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md)。

使用 Data Lake Storage Gen1 中的数据时，有几个要更改的字符串。

例如，如使用以 Data Lake Storage Gen1 作为主存储的群集，则数据的路径是：adl://<data_lake_storage_gen1_account_name>/azuredatalakestore.net/path/to/file**。 用于从存储在 Data Lake Storage Gen1 中的示例数据创建表的 Hive 查询如下所示：

```console
CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsg1storage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'
```

说明：

* `adl://hdiadlsg1storage.azuredatalakestore.net/` Data Lake Storage Gen1 的帐户的根。
* `/clusters/myhdiadlcluster` 是在创建群集时指定的群集数据的根。
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` 是查询中所用示例文件的位置。

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-as-additional-storage"></a>针对作为附加存储的 Data Lake Storage Gen1 (中的数据运行 Hive 查询) 

如果创建的群集使用 Blob 存储作为默认存储，则不会将示例数据包含在使用 Data Lake Storage Gen1 作为附加存储的存储帐户中。 在这种情况下，请先将数据从 Blob 存储传输到具有 Data Lake Storage Gen1 的存储帐户，然后运行这些查询，如前面的示例中所示。

有关如何使用 Data Lake Storage Gen1 将数据从 Blob 存储复制到存储帐户的信息，请参阅以下文章：

* [使用 Distcp 将数据复制到 Azure Blob 存储和 Data Lake Storage Gen1](data-lake-store-copy-data-wasb-distcp.md)
* [使用 AdlCopy 将数据从 Azure Blob 存储复制到 Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-storage-gen1-with-a-spark-cluster"></a>在 Spark 群集中使用 Data Lake Storage Gen1

可以使用 Spark 群集对存储在 Data Lake Storage Gen1 中的数据运行 Spark 作业。 有关详细信息，请参阅[使用 HDInsight Spark 群集分析 Data Lake Storage Gen1 中的数据](../hdinsight/spark/apache-spark-use-with-data-lake-store.md)。

### <a name="use-data-lake-storage-gen1-in-a-storm-topology"></a>在 Storm 拓扑中使用 Data Lake Storage Gen1

您可以使用具有 Data Lake Storage Gen1 的存储帐户来写入风暴拓扑中的数据。 有关如何实现此方案的说明，请参阅[将 Azure Data Lake Storage Gen1 与 HDInsight 上的 Apache Storm 配合使用](../hdinsight/storm/apache-storm-write-data-lake-store.md)。

## <a name="see-also"></a>请参阅

* [将 Data Lake Storage Gen1 与 Azure HDInsight 群集配合使用](../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen1.md)
* [PowerShell：创建 HDInsight 群集以使用 Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx