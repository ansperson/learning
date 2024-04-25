# Storage Account

An Azure storage account contains all of your Azure Storage data objects: blobs, files, queues, and tables. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS. For more information about Azure storage accounts, see Storage account overview.

## Prerequisites

```powershell
Connect-AzAccount
```

## Create a storage account

To create a general-purpose v2 storage account with PowerShell, first create a new resource group by calling the New-AzResourceGroup command:

```shell
$resourceGroup = "<resource-group>"
$location = "<location>"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

If you're not sure which region to specify for the -Location parameter, you can retrieve a list of supported regions for your subscription with the Get-AzLocation command:

```shell
Get-AzLocation | select Location
```

Next, create a standard general-purpose v2 storage account with read-access geo-redundant storage (RA-GRS) by using the New-AzStorageAccount command. Remember that the name of your storage account must be unique across Azure, so replace the placeholder value in brackets with your own unique value:

```shell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 `
  -AllowBlobPublicAccess $false
```

### Delete a storage account

```powershell
Remove-AzStorageAccount -Name <storage-account> -ResourceGroupName <resource-group>
```

## Recover a deleted storage account

A deleted storage account may be recovered in some cases from within the Azure portal. To recover a storage account, the following conditions must be true:

- The storage account was deleted within the past 14 days.
- The storage account was created with the Azure Resource Manager deployment model.
- A new storage account with the same name has not been created since the original account was deleted.
- The user who is recovering the storage account must be assigned an Azure RBAC role that provides the Microsoft.Storage/storageAccounts/write permission. For information about built-in Azure RBAC roles that provide this permission, see Azure built-in roles.

Before you attempt to recover a deleted storage account, make sure that the resource group for that account exists. If the resource group was deleted, you must recreate it. Recovering a resource group is not possible.

If the deleted storage account used customer-managed keys with Azure Key Vault and the key vault has also been deleted, then you must restore the key vault before you restore the storage account. For more information, see Azure Key Vault recovery overview.

### Recover a deleted account from the Azure portal

1. Navigate to the list of your storage accounts in the Azure portal.

2. Select the **Restore** button to open the **Restore deleted account** pane.

3. Select the subscription for the account that you want to recover from the Subscription drop-down.

4. From the dropdown, select the account to recover, as shown in the following image. If the storage account that you want to recover is not in the dropdown, then it cannot be recovered.

5. Select the Restore button to recover the account. The portal displays a notification that the recovery is in progress.

## Apply an Azure Resource Manager lock to a storage account

Microsoft recommends locking all of your storage accounts with an Azure Resource Manager lock to prevent accidental or malicious deletion of the storage account. There are two types of Azure Resource Manager resource locks:

- A CannotDelete lock prevents users from deleting a storage account, but permits reading and modifying its configuration.
- A ReadOnly lock prevents users from deleting a storage account or modifying its configuration, but permits reading the configuration.

>[!IMPORTANT]
>Locking a storage account does not protect containers or blobs within that account from being deleted or overwritten. For more information about how to protect blob data, see [Data protection overview](https://learn.microsoft.com/en-us/azure/storage/blobs/data-protection-overview).
<!-- MD028/no-blanks-blockquote -->

### Configure an Azure Resource Manager lock

```powershell
New-AzResourceLock -LockLevel CanNotDelete `
    -LockName <lock> `
    -ResourceName <storage-account> `
    -ResourceType Microsoft.Storage/storageAccounts `
    -ResourceGroupName <resource-group>
```

>[!NOTE]
>[Create a storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create)
