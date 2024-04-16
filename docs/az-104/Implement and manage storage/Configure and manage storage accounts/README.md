# Configure and manage storage accounts

## Azure Storage

A storage account is a container that groups a set of Azure Storage services together. Only data services from Azure Storage can be included in a storage account (**Azure Blobs**, **Azure Files**, **Azure Queues**, and **Azure Tables**).

An Azure storage account contains all of your Azure Storage data objects: blobs, files, queues, and tables. The storage account provides a unique namespace for your Azure Storage data that's accessible from anywhere in the world over HTTP or HTTPS. Data in your storage account is durable and highly available, secure, and massively scalable.

## Things to know about Azure Storage

You can think of Azure Storage as supporting three categories of data: structured data, unstructured data, and virtual machine data.

|Category|Description|Storage examples|
|--|--|--|
|Virtual machine data|Virtual machine data storage includes disks and files. Disks are persistent block storage for Azure IaaS virtual machines. Files are fully managed file shares in the cloud.|Storage for virtual machine data is provided through Azure managed disks. Data disks are used by virtual machines to store data like database files, website static content, or custom application code. The number of data disks you can add depends on the virtual machine size. Each data disk has a maximum capacity of 32,767 GB.|
|Unstructured data|Unstructured data is the least organized. Unstructured data may not have a clear relationship. The format of unstructured data is referred to as nonrelational.|Unstructured data can be stored by using Azure Blob Storage and Azure Data Lake Storage. Blob Storage is a highly scalable, REST-based cloud object store. Azure Data Lake Storage is the Hadoop Distributed File System (HDFS) as a service.|
|Structured data|Structured data is stored in a relational format that has a shared schema. Structured data is often contained in a database table with rows, columns, and keys. Tables are an autoscaling NoSQL store.|Structured data can be stored by using Azure Table Storage, Azure Cosmos DB, and Azure SQL Database. Azure Cosmos DB is a globally distributed database service. Azure SQL Database is a fully managed database-as-a-service built on SQL.|

## Explore Azure Storage services

Azure Storage offers four data services that can be accessed by using an Azure storage account:

- **Azure Blob Storage (containers):** A massively scalable object store for text and binary data.
- **Azure Files:** Managed file shares for cloud or on-premises deployments.
- **Azure Queue Storage:** A messaging store for reliable messaging between application components.
- **Azure Table Storage:** A service that stores nonrelational structured data (also known as structured NoSQL data).

### Azure Blob Storage (containers)

Azure Blob Storage is Microsoft's object storage solution for the cloud. Blob Storage is optimized for storing massive amounts of unstructured or nonrelational data, such as text or binary data. Blob Storage is ideal for:

- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

Objects in Blob Storage can be accessed from anywhere in the world via HTTP or HTTPS. Users or client applications can access blobs via URLs, the Azure Storage REST API, Azure PowerShell, the Azure CLI, or an Azure Storage client library. The storage client libraries are available for multiple languages, including .NET, Java, Node.js, Python, PHP, and Ruby.

>[!TIP]
>You can access data from Azure Blob Storage by using the NFS protocol.
<!-- MD028/no-blanks-blockquote -->

### Azure Files

Azure Files enables you to set up highly available network file shares. Shares can be accessed by using the Server Message Block (SMB) protocol and the Network File System (NFS) protocol. Multiple virtual machines can share the same files with both read and write access. You can also read the files by using the REST interface or the storage client libraries.

File shares can be used for many common scenarios:

- Many on-premises applications use file shares. This feature makes it easier to migrate those applications that share data to Azure. If you mount the file share to the same drive letter that the on-premises application uses, the part of your application that accesses the file share should work with minimal, if any, changes.
- Configuration files can be stored on a file share and accessed from multiple virtual machines. Tools and utilities used by multiple developers in a group can be stored on a file share, ensuring that everybody can find them, and that they use the same version.
- Diagnostic logs, metrics, and crash dumps are just three examples of data that can be written to a file share and processed or analyzed later.
The storage account credentials are used to provide authentication for access to the file share. All users who have the share mounted should have full read/write access to the share.

### Azure Queue Storage

Azure Queue Storage is used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.

Consider a scenario where you want your customers to be able to upload pictures, and you want to create thumbnails for each picture. You could have your customer wait for you to create the thumbnails while uploading the pictures. An alternative is to use a queue. When the customer finishes the upload, you can write a message to the queue. Then you can use an Azure Function to retrieve the message from the queue and create the thumbnails. Each of the processing parts can be scaled separately, which gives you more control when tuning the configuration.

### Azure Table Storage

Azure Table storage is a service that stores non-relational structured data (also known as structured NoSQL data) in the cloud, providing a key/attribute store with a schemaless design. Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve. Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data. In addition to the existing Azure Table Storage service, there's a new Azure Cosmos DB Table API offering that provides throughput-optimized tables, global distribution, and automatic secondary indexes.

## Types of storage accounts

Azure Storage offers several types of storage accounts. Each type supports different features and has its own pricing model.

|Type of storage account|Supported storage services|Usage|
|--|--|--|
|Standard general-purpose v2|Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files|Standard storage account type for blobs, file shares, queues, and tables. Recommended for most scenarios using Azure Storage. If you want support for network file system (NFS) in Azure Files, use the premium file shares account type.|
|Premium block blobs|Blob Storage (including Data Lake Storage)|Premium storage account type for block blobs and append blobs. Recommended for scenarios with high transaction rates or that use smaller objects or require consistently low storage latency. Learn more about example workloads.|
|Premium file shares|Azure File|Premium storage account type for file shares only. Recommended for enterprise or high-performance scale applications. Use this account type if you want a storage account that supports both Server Message Block (SMB) and NFS file shares.|
|Premium page blobs|Page blobs only|Premium storage account type for page blobs only.|

## Storage account tiers

General purpose Azure storage accounts have two tiers: Standard and Premium.

- Standard storage accounts are backed by magnetic hard disk drives (HDD). A standard storage account provides the lowest cost per GB. You can use Standard tier storage for applications that require bulk storage or where data is infrequently accessed.
- Premium storage accounts are backed by solid-state drives (SSD) and offer consistent low-latency performance. You can use Premium tier storage for Azure virtual machine disks with I/O-intensive applications like databases.

## Additional Information

- When creating a storage account you can only choose between hot or cool tiers (access tier).
- You can't change a storage account to a different type after it's created. To move your data to a storage account of a different type, you must create a new account and copy the data to the new account.
- All data in your storage account is automatically encrypted on the service side. For more information about encryption and key management, see [Azure Storage encryption for data at rest](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption).
- Your storage account name must be unique within Azure. No two storage accounts can have the same name.
- You can't convert a Standard tier storage account to a Premium tier storage account or vice versa. You must create a new storage account with the desired type and copy data, if applicable, to a new storage account.

>[!IMPORTANT]
>[Storage Account Create](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create)
<!-- MD028/no-blanks-blockquote -->
>[!NOTE]
>[Introduction](https://learn.microsoft.com/en-us/training/modules/configure-storage-accounts/2-implement-azure-storage)
>
>[Storage Services](https://learn.microsoft.com/en-us/training/modules/configure-storage-accounts/3-explore-azure-storage-services)
>
>[Overview](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview)
