---
title: Manage properties and metadata for a blob with .NET
titleSuffix: Azure Storage
description: Learn how to set and retrieve system properties and store custom metadata on blobs in your Azure Storage account using the .NET client library.
services: storage
author: pauljewellmsft
ms.author: pauljewell
ms.date: 03/28/2022
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.devlang: csharp
ms.custom: devx-track-csharp, devguide-csharp
---

# Manage blob properties and metadata with .NET

In addition to the data they contain, blobs support system properties and user-defined metadata. This article shows how to manage system properties and user-defined metadata with the [Azure Storage client library for .NET](/dotnet/api/overview/azure/storage).

## About properties and metadata

- **System properties**: System properties exist on each Blob storage resource. Some of them can be read or set, while others are read-only. Under the covers, some system properties correspond to certain standard HTTP headers. The Azure Storage client library for .NET maintains these properties for you.

- **User-defined metadata**: User-defined metadata consists of one or more name-value pairs that you specify for a Blob storage resource. You can use metadata to store additional values with the resource. Metadata values are for your own purposes only, and don't affect how the resource behaves.

    Metadata name/value pairs are valid HTTP headers and should adhere to all restrictions governing HTTP headers. For more information about metadata naming requirements, see [Metadata names](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#metadata-names).

> [!NOTE]
> Blob index tags also provide the ability to store arbitrary user-defined key/value attributes alongside an Azure Blob storage resource. While similar to metadata, only blob index tags are automatically indexed and made searchable by the native blob service. Metadata cannot be indexed and queried unless you utilize a separate service such as Azure Search.
>
> To learn more about this feature, see [Manage and find data on Azure Blob storage with blob index](storage-manage-find-blobs.md).

## Set and retrieve properties

The following code example sets the `ContentType` and `ContentLanguage` system properties on a blob.

To set properties on a blob, call [SetHttpHeaders](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.sethttpheaders) or [SetHttpHeadersAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.sethttpheadersasync). Any properties not explicitly set are cleared. The following code example first gets the existing properties on the blob, then uses them to populate the headers that aren't being updated.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_SetBlobProperties":::

The following code example gets a blob's system properties and displays some of the values.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadBlobProperties":::

## Set and retrieve metadata

You can specify metadata as one or more name-value pairs on a blob or container resource. To set metadata, add name-value pairs to the `Metadata` collection on the resource. Then, call one of the following methods to write the values:

- [SetMetadata](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.setmetadata)
- [SetMetadataAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.setmetadataasync)

The following code example sets metadata on a blob. One value is set using the collection's `Add` method. The other value is set using implicit key/value syntax.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_AddBlobMetadata":::

The following code example reads the metadata on a blob.

To retrieve metadata, call the [GetProperties](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.getproperties) or [GetPropertiesAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.getpropertiesasync) method on your blob or container to populate the [Metadata](/dotnet/api/azure.storage.blobs.models.blobproperties.metadata) collection, then read the values, as shown in the example below. The `GetProperties` method retrieves blob properties and metadata by calling both the [Get Blob Properties](/rest/api/storageservices/get-blob-properties) operation and the [Get Blob Metadata](/rest/api/storageservices/get-blob-metadata) operation.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadBlobMetadata":::

## See also

- [Set Blob Properties operation](/rest/api/storageservices/set-blob-properties)
- [Get Blob Properties operation](/rest/api/storageservices/get-blob-properties)
- [Set Blob Metadata operation](/rest/api/storageservices/set-blob-metadata)
- [Get Blob Metadata operation](/rest/api/storageservices/get-blob-metadata)
