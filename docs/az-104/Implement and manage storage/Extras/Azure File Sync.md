# Planning for an Azure File Sync deployment

Azure File Sync is a service that allows you to cache several Azure file shares on an on-premises Windows Server or cloud VM.

This article introduces you to Azure File Sync concepts and features. Once you're familiar with Azure File Sync, consider following the [Azure File Sync deployment guide](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-deployment-guide) to try out this service.

The files will be stored in the cloud in Azure file shares. Azure file shares can be used in two ways: by directly mounting these serverless Azure file shares (SMB) or by caching Azure file shares on-premises using Azure File Sync. Which deployment option you choose changes the aspects you need to consider as you plan for your deployment.

- Direct mount of an Azure file share: Because Azure Files provides SMB access, you can mount Azure file shares on-premises or in the cloud using the standard SMB client available in Windows, macOS, and Linux. Because Azure file shares are serverless, deploying for production scenarios doesn't require managing a file server or NAS device. This means you don't have to apply software patches or swap out physical disks.

- Cache Azure file share on-premises with Azure File Sync: Azure File Sync enables you to centralize your organization's file shares in Azure Files, while keeping the flexibility, performance, and compatibility of an on-premises file server. Azure File Sync transforms an on-premises (or cloud) Windows Server into a quick cache of your Azure file share.

## Management concepts

An Azure File Sync deployment has three fundamental management objects:

- **Azure file share:** An Azure file share is a serverless cloud file share, which provides the cloud endpoint of an Azure File Sync sync relationship. Files in an Azure file share can be accessed directly with SMB or the FileREST protocol, although we encourage you to primarily access the files through the Windows Server cache when the Azure file share is being used with.Azure File Sync. This is because Azure Files today lacks an efficient change detection mechanism like Windows Server has, so changes to the Azure file share directly will take time to propagate back to the server endpoints.
- **Server endpoint:** The path on the Windows Server that is being synced to an Azure file share. This can be a specific folder on a volume or the root of the volume. Multiple server endpoints can exist on the same volume if their namespaces don't overlap.
- **Sync group:** The object that defines the sync relationship between a cloud endpoint, or Azure file share, and a server endpoint. Endpoints within a sync group are kept in sync with each other. If for example, you have two distinct sets of files that you want to manage with Azure File Sync, you would create two sync groups and add different endpoints to each sync group.

## Azure file share management concepts

Azure file shares are deployed into storage accounts, which are top-level objects that represent a shared pool of storage. This pool of storage can be used to deploy multiple file shares, as well as other storage resources such as blob containers, queues, or tables. All storage resources that are deployed into a storage account share the limits that apply to that storage account...

There are two main types of storage accounts you will use for Azure Files deployments:

- **General purpose version 2 (GPv2) storage accounts:** GPv2 storage accounts allow you to deploy Azure file shares on standard/hard disk-based (HDD-based) hardware. In addition to storing Azure file shares, GPv2 storage accounts can store other storage resources such as blob containers, queues, or tables.
- **FileStorage storage accounts:** FileStorage storage accounts allow you to deploy Azure file shares on premium/solid-state disk-based (SSD-based) hardware. FileStorage accounts can only be used to store Azure file shares; no other storage resources (blob containers, queues, tables, etc.) can be deployed in a FileStorage account. Only FileStorage accounts can deploy both SMB and NFS file shares.

There are several other storage account types you may come across in the Azure portal, PowerShell, or CLI. Two storage account types, BlockBlobStorage and BlobStorage storage accounts, cannot contain Azure file shares. The other two storage account types you may see are general purpose version 1 (GPv1) and classic storage accounts, both of which can contain Azure file shares. Although GPv1 and classic storage accounts may contain Azure file shares, most new features of Azure Files are available only in GPv2 and FileStorage storage accounts. We therefore recommend to only use GPv2 and FileStorage storage accounts for new deployments, and to upgrade GPv1 and classic storage accounts if they already exist in your environment.

<!-- MD028/no-blanks-blockquote -->
>[!NOTE]
>[Planning for an Azure File Sync deployment](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-planning)
