# Manage data by using Azure Storage Explorer and AzCopy

## Get started with Storage Explorer

Microsoft Azure Storage Explorer is a standalone app that makes it easy to work with Azure Storage data on Windows, macOS, and Linux.

### Connect Azure Storage Explorer to a storage account

Storage accounts provide a flexible solution that keeps data as files, tables, and messages. With Azure Storage Explorer, it's easy to read and manipulate this data.

You want to enable your engineers to manage the data stored in Azure Storage so they can maintain the data that your CRM application uses. You want to assess whether they can use Storage Explorer for this purpose.

Here, you'll learn about Storage Explorer and how you can use it to manage data from multiple storage accounts and subscriptions. You'll learn different ways of using Storage Explorer to connect to your data, Azure Stack, and data held in Azure Data Lake Storage.

### What is Storage Explorer?

Storage Explorer is a GUI application developed by Microsoft to simplify accessing and managing data stored in Azure Storage accounts. Storage Explorer is available on Windows, macOS, and Linux.

Some of the benefits of using Storage Explorer are:

- It's easy to connect to and manage multiple storage accounts.
- The interface lets you connect to Data Lake Storage.
- You can also use the interface to update and view entities in your storage accounts.
- Storage Explorer is free to download and use.

With Storage Explorer, you can use a range of storage and data operation tasks on any of your Azure Storage accounts. These tasks include edit, download, copy, and delete.

### Azure Storage types

Azure Storage Explorer can access many different data types from services like these:

- Azure Blob Storage: Blob storage is used to store unstructured data as a binary large object (blob).
- Azure Table Storage: Table storage is used to store NoSQL, semi-structured data.
- Azure Queue Storage: Queue storage is used to store messages in a queue, which can then be accessed and processed by applications through HTTP(S) calls.
- Azure Files: Azure Files is a file-sharing service that enables access through the Server Message Block protocol, similar to traditional file servers.
- Azure Data Lake Storage: Azure Data Lake, based on Apache Hadoop, is designed for large data volumes and can store unstructured and structured data. Azure Data Lake Storage Gen1 is a dedicated service. Azure Data Lake Storage Gen2 is Azure Blob Storage with the hierarchical namespace feature enabled on the account.

## Get started with AzCopy

AzCopy is a command-line utility that you can use to copy blobs or files to or from a storage account.

### Run AzCopy

For convenience, consider adding the directory location of the AzCopy executable to your system path for ease of use. That way you can type azcopy from any directory on your system.

If you choose not to add the AzCopy directory to your path, you'll have to change directories to the location of your AzCopy executable and type `azcopy` or `.\azcopy` in Windows PowerShell command prompts.

As an owner of your Azure Storage account, you aren't automatically assigned permissions to access data. Before you can do anything meaningful with AzCopy, you need to decide how you'll provide authorization credentials to the storage service.

### Authorize AzCopy

You can provide authorization credentials by using Microsoft Entra ID, or by using a Shared Access Signature (SAS) token.

#### Option 1: Use Microsoft Entra ID

By using Microsoft Entra ID, you can provide credentials once instead of having to append a SAS token to each command.

#### Option 2: Use a SAS token

You can append a SAS token to each source or destination URL that use in your AzCopy commands.

This example command recursively copies data from a local directory to a blob container. A fictitious SAS token is appended to the end of the container URL.

```shell
azcopy copy "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

>[!IMPORTANT]
The Secure transfer required setting of a storage account determines whether the connection to a storage account is secured with Transport Layer Security (TLS). This setting is enabled by default.
<!-- MD028/no-blanks-blockquote -->

### List of commands

|Command|Description|
|---|---|
|`azcopy bench`|Runs a performance benchmark by uploading or downloading test data to or from a specified location.|
|`azcopy copy`|Copies source data to a destination location|
|`azcopy doc`|Generates documentation for the tool in Markdown format.|
|`azcopy env`|Shows the environment variables that can configure AzCopy's behavior.|
|`azcopy jobs`|Subcommands related to managing jobs.|
|`azcopy jobs clean`| Remove all log and plan files for all jobs.|
|`azcopy jobs list`| Displays information on all jobs.|
|`azcopy jobs remove`| Remove all files associated with the given job ID.|
|`azcopy jobs resume`| Resumes the existing job with the given job ID.|
|`azcopy jobs show`| Shows detailed information for the given job ID.|
|`azcopy list`| Lists the entities in a given resource.|
|`azcopy login`| Logs in to Microsoft Entra ID to access Azure Storage resources.|
|`azcopy login status`| Lists the entities in a given resource.|
|`azcopy logout`| Logs the user out and terminates access to Azure Storage resources.|
|`azcopy make`| Creates a container or file share.|
|`azcopy remove`| Delete blobs or files from an Azure storage account.|
|`azcopy sync`| Replicates the source location to the destination location.|
|`azcopy set-properties`|Change the access tier of one or more blobs and replace (overwrite) the metadata, and index tags of one or more blobs.|

>[!NOTE]
>[Get started with Storage Explorer](https://learn.microsoft.com/en-us/azure/storage/storage-explorer/vs-azure-tools-storage-manage-with-storage-explorer)
>
>[Connect Azure Storage Explorer to a storage account](https://learn.microsoft.com/en-us/training/modules/upload-download-and-manage-data-with-azure-storage-explorer/2-connect-storage-account)
>
>[Get started with AzCopy](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10)
