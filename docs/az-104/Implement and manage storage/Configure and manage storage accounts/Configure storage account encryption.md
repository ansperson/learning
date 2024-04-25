# Azure Storage encryption for data at rest

Azure Storage uses service-side encryption (SSE) to automatically encrypt your data when it is persisted to the cloud. Azure Storage encryption protects your data and to help you to meet your organizational security and compliance commitments.

Microsoft recommends using service-side encryption to protect your data for most scenarios. However, the Azure Storage client libraries for Blob Storage and Queue Storage also provide client-side encryption for customers who need to encrypt data on the client.

## About Azure Storage service-side encryption

Data in Azure Storage is encrypted and decrypted transparently using 256-bit AES encryption, one of the strongest block ciphers available, and is FIPS 140-2 compliant. Azure Storage encryption is similar to BitLocker encryption on Windows.

Azure Storage encryption is enabled for all storage accounts, including both Resource Manager and classic storage accounts. Azure Storage encryption cannot be disabled. Because your data is secured by default, you don't need to modify your code or applications to take advantage of Azure Storage encryption.

**Data in a storage account is encrypted regardless of performance tier (standard or premium), access tier (hot or cool), or deployment model (Azure Resource Manager or classic).** All new and existing block blobs, append blobs, and page blobs are encrypted, including blobs in the archive tier. All Azure Storage redundancy options support encryption, and all data in both the primary and secondary regions is encrypted when geo-replication is enabled. All Azure Storage resources are encrypted, including blobs, disks, files, queues, and tables. All object metadata is also encrypted.

There is no additional cost for Azure Storage encryption.

## About encryption key management

Data in a new storage account is encrypted with Microsoft-managed keys by default. You can continue to rely on Microsoft-managed keys for the encryption of your data, or you can manage encryption with your own keys. If you choose to manage encryption with your own keys, you have two options. You can use either type of key management, or both:

- You can specify a customer-managed key to use for encrypting and decrypting data in Blob Storage and in Azure Files. Customer-managed keys must be stored in Azure Key Vault or Azure Key Vault Managed Hardware Security Model (HSM). For more information about customer-managed keys, see Use customer-managed keys for Azure Storage encryption.
You can specify a customer-provided key on Blob Storage operations. A client making a read or write request against Blob Storage can include an encryption key on the request for granular control over how blob data is encrypted and decrypted. For more information about customer-provided keys, see Provide an encryption key on a request to Blob Storage.
- By default, a storage account is encrypted with a key that is scoped to the entire storage account. Encryption scopes enable you to manage encryption with a key that is scoped to a container or an individual blob. You can use encryption scopes to create secure boundaries between data that resides in the same storage account but belongs to different customers. Encryption scopes can use either Microsoft-managed keys or customer-managed keys.

## Enable infrastructure encryption for double encryption of data

Azure Storage automatically encrypts all data in a storage account at the service level using 256-bit AES with GCM mode encryption, one of the strongest block ciphers available, and is FIPS 140-2 compliant. Customers who require higher levels of assurance that their data is secure can also enable 256-bit AES with CBC encryption at the Azure Storage infrastructure level for double encryption. Double encryption of Azure Storage data protects against a scenario where one of the encryption algorithms or keys might be compromised. In this scenario, the additional layer of encryption continues to protect your data.

Infrastructure encryption can be enabled for the entire storage account, or for an encryption scope within an account. When infrastructure encryption is enabled for a storage account or an encryption scope, data is encrypted twice — once at the service level and once at the infrastructure level — with two different encryption algorithms and two different keys.

Service-level encryption supports the use of either Microsoft-managed keys or customer-managed keys with Azure Key Vault or Key Vault Managed Hardware Security Model (HSM). Infrastructure-level encryption relies on Microsoft-managed keys and always uses a separate key. For more information about key management with Azure Storage encryption, see [About encryption key management](https://learn.microsoft.com/en-gb/azure/storage/common/storage-service-encryption#about-encryption-key-management).

To doubly encrypt your data, you must first create a storage account or an encryption scope that is configured for infrastructure encryption.

>[!IMPORTANT]
Infrastructure encryption is recommended for scenarios where doubly encrypting data is necessary for compliance requirements. For most other scenarios, Azure Storage encryption provides a sufficiently powerful encryption algorithm, and there is unlikely to be a benefit to using infrastructure encryption.
<!-- MD028/no-blanks-blockquote -->

### Create an account with infrastructure encryption enabled

To enable infrastructure encryption for a storage account, you must configure a storage account to use infrastructure encryption at the time that you create the account. Infrastructure encryption cannot be enabled or disabled after the account has been created. The storage account must be of type general-purpose v2 or premium block blob.

```powershell
New-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage-account> `
    -Location <location> `
    -SkuName "Standard_RAGRS" `
    -Kind StorageV2 `
    -AllowBlobPublicAccess $false `
    -RequireInfrastructureEncryption
```

## Additional Information

- Storage Service Encryption uses 256-bit AES encryption. When your data is in any tier of an Azure storage account, it's encrypted by default. Each storage account is automatically encrypted as it's created. **In fact, you can't disable encryption for storage accounts even if you want to. You can choose whether you want to use your own customer-managed keys (BYOK) or the default Microsoft-managed keys that are kept in Azure Key Vault.**

>[!NOTE]
>[Introduction](https://learn.microsoft.com/en-us/training/modules/configure-blob-storage/4-create-blob-access-tiers)
>
>[Overview](https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview)
>[Azure Storage encryption for data at rest](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption)
>
>[Secure data at rest by using Azure Storage Service Encryption](https://learn.microsoft.com/en-us/training/modules/secure-data-at-rest/3-azure-storage-service-encryption)
>[Enable infrastructure encryption for double encryption of data](https://learn.microsoft.com/en-gb/azure/storage/common/infrastructure-encryption-enable)
