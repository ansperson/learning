# Authorize access to data in Azure Storage

Each time you access data in your storage account, your client application makes a request over HTTP/HTTPS to Azure Storage. By default, every resource in Azure Storage is secured, and every request to a secure resource must be authorized. Authorization ensures that the client application has the appropriate permissions to access a particular resource in your storage account.

## Understand authorization for data operations

The following table describes the options that Azure Storage offers for authorizing access to data:

|Azure artifact|Shared Key (storage account key)|Shared access signature (SAS)|Microsoft Entra ID|On-premises Active Directory Domain Services|anonymous read access|Storage Local Users|
|---|---|---|---|---|---|---|
|Azure Blobs|Supported|Supported|Supported|Not supported|Supported but not recommended|Supported, only for SFTP|
|Azure Files (SMB)|Supported|Not supported|Supported, only with Microsoft Entra Domain Services for cloud-only or Microsoft Entra Kerberos for hybrid identities|Supported, credentials must be synced to Microsoft Entra ID|Not supported|Not supported|
|Azure Files (REST)|Supported|Supported|Supported|Not supported|Not supported|Not supported|
|Azure Queues|Supported|Supported|Supported|Not Supported|Not supported|Not supported|
|Azure Tables|Supported|Supported|Supported|Not supported|Not supported|Not supported|

Each authorization option is briefly described below:

**Shared Key authorization** for blobs, files, queues, and tables. A client using Shared Key passes a header with every request that is signed using the storage account access key. For more information, see [Authorize with Shared Key](https://learn.microsoft.com/en-us/rest/api/storageservices/authorize-with-shared-key/).
Microsoft recommends that you disallow Shared Key authorization for your storage account. When Shared Key authorization is disallowed, clients must use Microsoft Entra ID or a user delegation SAS to authorize requests for data in that storage account. For more information, see [Prevent Shared Key authorization for an Azure Storage account](https://learn.microsoft.com/en-us/azure/storage/common/authorize-data-access#:~:text=account.%20For%20more%20information%2C%20see-,Prevent%20Shared%20Key%20authorization%20for%20an%20Azure%20Storage%20account,-.).

**Shared access signatures** for blobs, files, queues, and tables. Shared access signatures (SAS) provide limited delegated access to resources in a storage account via a signed URL. The signed URL specifies the permissions granted to the resource and the interval over which the signature is valid. A service SAS or account SAS is signed with the account key, while the user delegation SAS is signed with Microsoft Entra credentials and applies to blobs only. For more information, see [Using shared access signatures (SAS)](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview).

**Microsoft Entra integration** for authorizing requests to blob, queue, and table resources. Microsoft recommends using Microsoft Entra credentials to authorize requests to data when possible for optimal security and ease of use...

You can use Azure role-based access control (Azure RBAC) to manage a security principal's permissions to blob, queue, and table resources in a storage account. You can also use Azure attribute-based access control (ABAC) to add conditions to Azure role assignments for blob resources.

For more information about RBAC, see [What is Azure role-based access control (Azure RBAC)?](https://learn.microsoft.com/en-us/azure/storage/common/authorize-data-access#:~:text=What%20is%20Azure%20role%2Dbased%20access%20control%20(Azure%20RBAC)%3F).

For more information about ABAC and its feature status, see:

- [What is Azure attribute-based access control (Azure ABAC)?](https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-overview)
- [The status of ABAC condition features](https://learn.microsoft.com/en-us/azure/role-based-access-control/conditions-overview#status-of-condition-features)
- [The status of ABAC condition features in Azure Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-auth-abac#status-of-condition-features-in-azure-storage)

**Microsoft Entra Domain Services authentication** for Azure Files. Azure Files supports identity-based authorization over Server Message Block (SMB) through Microsoft Entra Domain Services. You can use Azure RBAC for granular control over a client's access to Azure Files resources in a storage account. For more information about Azure Files authentication using domain services, see the [overview](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-active-directory-overview).

**On-premises Active Directory Domain Services (AD DS, or on-premises AD DS) authentication** for Azure Files. Azure Files supports identity-based authorization over SMB through AD DS. Your AD DS environment can be hosted in on-premises machines or in Azure VMs. SMB access to Files is supported using AD DS credentials from domain joined machines, either on-premises or in Azure. You can use a combination of Azure RBAC for share level access control and NTFS DACLs for directory/file level permission enforcement...

**Anonymous read access** for blob data is supported, but not recommended. When anonymous access is configured, clients can read blob data without authorization. We recommend that you disable anonymous access for all of your storage accounts. For more information, see [Overview: Remediating anonymous read access for blob data](https://learn.microsoft.com/en-us/azure/storage/common/authorize-data-access#:~:text=Overview%3A%20Remediating%20anonymous%20read%20access%20for%20blob%20data).

**Storage Local Users** can be used to access blobs with SFTP or files with SMB. Storage Local Users support container level permissions for authorization...

>[!NOTE]
>[Authorize access to data in Azure Storage](https://learn.microsoft.com/en-us/azure/storage/common/authorize-data-access)
<!-- MD028/no-blanks-blockquote -->
