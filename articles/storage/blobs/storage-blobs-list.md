---
title: 使用 .NET 列出 Blob - Azure 存储
description: 了解如何使用 .NET 客户端库列出 Azure 存储帐户的容器中的 blob。 代码示例演示如何在平面列表中列出 blob，或者如何分层列出 blob，就像它们被组织到目录或文件夹中一样。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/22/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: e8a35902c198412f6e41c0cf39162836deb5e443
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91280090"
---
# <a name="list-blobs-with-net"></a>使用 .NET 列出 blob

通过代码列出 Blob 时，可以指定多个选项来管理如何从 Azure 存储返回结果。 可以指定要在每个结果集中返回的结果数，然后检索后续结果集。 可以指定前缀以返回名称以该字符或字符串开头的 blob。 而且，可以在平面列表结构中列出 blob，也可以分层列出 blob。 分层列表返回 blob，就像它们被组织到文件夹中一样。

本文介绍如何使用[适用于 .NET 的 Azure 存储客户端库](/dotnet/api/overview/azure/storage?view=azure-dotnet)列出 blob。  

## <a name="understand-blob-listing-options"></a>了解 Blob 列出选项

若要列出存储帐户中的 Blob，请调用以下方法之一：

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

- [BlobContainerClient.GetBlobs](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobs?view=azure-dotnet)
- [BlobContainerClient.GetBlobsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync?view=azure-dotnet)
- [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy?view=azure-dotnet)
- [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync?view=azure-dotnet)

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

- [CloudBlobClient.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobs)
- [CloudBlobClient.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmented)
- [CloudBlobClient.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmentedasync)

若要列出容器中的 Blob，请调用以下方法之一：

- [CloudBlobContainer.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs)
- [CloudBlobContainer.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmented)
- [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)

这些方法的重载提供更多选项用于管理列出操作返回 Blob 的方式。 后续部分将介绍这些选项。

---

### <a name="manage-how-many-results-are-returned"></a>管理要返回的结果数

默认情况下，列表操作一次最多返回 5000 个结果，但你可以指定你所希望的每个列表操作返回的结果数。 本文演示的示例说明了如何执行此操作。

如果列表操作返回的 blob 超过 5000 个，或者可用的 blob 数超过指定的数量，Azure 存储会返回继续标记，并显示 blob 列表。 继续标记是一个不透明值，可用于从 Azure 存储中检索下一组结果。

在代码中检查继续标记的值，以确定它是否为 null。 如果继续标记为 null，则表示结果集是完整的。 如果继续标记不为 null，则再次调用列出操作，并传入继续标记以检索下一组结果，直到继续标记为 null。

### <a name="filter-results-with-a-prefix"></a>使用前缀筛选结果

若要筛选 blob 列表，请指定参数的字符串 `prefix` 。 前缀字符串可以包含一个或多个字符。 然后，Azure 存储只返回其名称以该前缀开头的 Blob。

### <a name="return-metadata"></a>返回元数据

可以返回包含结果的 blob 元数据。 

- 如果使用的是 .NET v12 SDK，请为 [BlobTraits](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.models.blobtraits?view=azure-dotnet) 枚举指定 Metadata  值。

- 如果使用的是 .NET v11 SDK，请为 [BlobListingDetails](/dotnet/api/microsoft.azure.storage.blob.bloblistingdetails) 枚举指定 Metadata  值。 Azure 存储包含每个返回的 Blob 的元数据，因此在此上下文中，无需同时调用 **FetchAttributes** 方法之一即可检索 Blob 元数据。

### <a name="flat-listing-versus-hierarchical-listing"></a>平面列表与分层列表

Azure 存储中的 Blob 以平面范式进行组织，而不是以分层范式（类似于经典文件系统）进行组织。 但是，可以将 Blob 组织到虚拟目录中，以便模拟文件夹结构。 虚拟目录构成 blob 名称的一部分，并由分隔符表示。

若要将 blob 组织为虚拟目录，请在 blob 名称中使用分隔符。 默认分隔符是正斜杠 (/)，但你可以指定任何字符作为分隔符。

如果使用分隔符来命名 Blob，可以选择以分层方式列出 Blob。 对于分层列出操作，Azure 存储将返回父对象下的所有虚拟目录和 Blob。 可以递归方式调用列出操作来遍历层次结构，类似于以编程方式遍历经典文件系统。

## <a name="use-a-flat-listing"></a>使用平面列出

默认情况下，列出操作在平面列表中返回 Blob。 在平面列表中，Blob 不会按虚拟目录进行组织。

以下示例使用平面列表列出指定容器中的 Blob（其中指定了可选的段大小），并将 Blob 名称写入控制台窗口。

如果在帐户上启用了分层命名空间功能，则目录不是虚拟目录， 而是具体的独立对象。 因此，目录在列表中显示为长度为零的 blob。

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsFlatListing":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

```csharp
private static async Task ListBlobsFlatListingAsync(CloudBlobContainer container, int? segmentSize)
{
    BlobContinuationToken continuationToken = null;
    CloudBlob blob;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned
        // and execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(string.Empty,
                true, BlobListingDetails.Metadata, segmentSize, continuationToken, null, null);

            foreach (var blobItem in resultSegment.Results)
            {
                blob = (CloudBlob)blobItem;

                // Write out some blob properties.
                Console.WriteLine("Blob name: {0}", blob.Name);
            }

            Console.WriteLine();

           // Get the continuation token and loop until it is null.
           continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

示例输出类似于：

```
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## <a name="use-a-hierarchical-listing"></a>使用分层列表

以分层方式调用列出操作时，Azure 存储将返回位于层次结构第一级别的虚拟目录和 Blob。 将设置每个虚拟目录的 [Prefix](/dotnet/api/microsoft.azure.storage.blob.cloudblobdirectory.prefix) 属性，以便可以在递归调用中传递前缀来检索下一个目录。

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

若要以分层方式列出 blob，请调用 [BlobContainerClient.GetBlobsByHierarchy](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchy?view=azure-dotnet) 或 [BlobContainerClient.GetBlobsByHierarchyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsbyhierarchyasync?view=azure-dotnet) 方法。

以下示例使用分层列表列出指定容器中的 Blob（其中指定了可选的段大小），并将 Blob 名称写入控制台窗口。

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CRUD.cs" id="Snippet_ListBlobsHierarchicalListing":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

若要以分层方式列出 Blob，请将列出方法的 `useFlatBlobListing` 参数设置为 **false**。

以下示例使用平面列表列出指定容器中的 Blob（其中指定了可选的段大小），并将 Blob 名称写入控制台窗口。

```csharp
private static async Task ListBlobsHierarchicalListingAsync(CloudBlobContainer container, string prefix)
{
    CloudBlobDirectory dir;
    CloudBlob blob;
    BlobContinuationToken continuationToken = null;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned and
        // execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(prefix,
                false, BlobListingDetails.Metadata, null, continuationToken, null, null);
            foreach (var blobItem in resultSegment.Results)
            {
                // A hierarchical listing may return both virtual directories and blobs.
                if (blobItem is CloudBlobDirectory)
                {
                    dir = (CloudBlobDirectory)blobItem;

                    // Write out the prefix of the virtual directory.
                    Console.WriteLine("Virtual directory prefix: {0}", dir.Prefix);

                    // Call recursively with the prefix to traverse the virtual directory.
                    await ListBlobsHierarchicalListingAsync(container, dir.Prefix);
                }
                else
                {
                    // Write out the name of the blob.
                    blob = (CloudBlob)blobItem;

                    Console.WriteLine("Blob name: {0}", blob.Name);
                }
                Console.WriteLine();
            }

            // Get the continuation token and loop until it is null.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

示例输出类似于：

```
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> 在分层列出操作中无法列出 Blob 快照。

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>后续步骤

- [列出 Blob](/rest/api/storageservices/list-blobs)
- [枚举 Blob 资源](/rest/api/storageservices/enumerating-blob-resources)
