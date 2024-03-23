# Blob access tiers

Azure Storage supports several access tiers for blob data, including Hot, Cool, and Archive. Each access tier is optimized to support a particular pattern of data usage.

## Things to know about blob access tiers

Characteristics of the blob access tiers.

### Online access tiers

When your data is stored in an online access tier (either hot, cool or cold), users can access it immediately. The hot tier is the best choice for data that is in active use. The cool or cold tier is ideal for data that is accessed less frequently, but that still must be available for reading and writing.

#### Hot tier

The Hot tier is optimized for frequent reads and writes of objects in the Azure storage account. A good usage case is data that is actively being processed. By default, new storage accounts are created in the Hot tier. This tier has the lowest access costs, but higher storage costs than the Cool and Archive tiers.

#### Cool tier

The Cool tier is optimized for storing large amounts of data that's infrequently accessed. This tier is intended for data that remains in the Cool tier for at least 30 days. A usage case for the Cool tier is short-term backup and disaster recovery datasets and older media content. This content shouldn't be viewed frequently, but it needs to be immediately available. Storing data in the Cool tier is more cost-effective. Accessing data in the Cool tier can be more expensive than accessing data in the Hot tier.

#### Cold tier

The Cold tier is also optimized for storing large amounts of data that's infrequently accessed. This tier is intended for data that can remain in the tier for at least 90 days.

### Archive access tier

The archive tier is an offline tier for storing data that is rarely accessed. The archive access tier has the lowest storage cost. However, this tier has higher data retrieval costs with a higher latency as compared to the hot, cool, and cold tiers.

### Archive tier

The Archive tier is an offline tier that's optimized for data that can tolerate several hours of retrieval latency. Data must remain in the Archive tier for at least 180 days or be subject to an early deletion charge. Data for the Archive tier includes secondary backups, original raw data, and legally required compliance information. This tier is the most cost-effective option for storing data. Accessing data is more expensive in the Archive tier than accessing data in the other tiers.

## Configure the blob access tier

In the Azure portal, you can select the blob access tier for your Azure storage account. You can also change the blob access tier for your account at any time. By selecting the correct access tier for your needs, you can store your blob data in the most cost-effective manner.

>[!NOTE]
>Setting the access tier is only allowed on Block Blobs. They are not supported for Append and Page Blobs.
>
>In an account that has soft delete enabled, a blob is considered deleted after it is deleted and retention period expires. Until that period expires, the blob is only soft-deleted and is not subject to the early deletion penalty.

## Default account access tier setting

Storage accounts have a default access tier setting that indicates the online tier in which a new blob is created. The default access tier setting can be set to either hot or cool. Users can override the default setting for an individual blob when uploading the blob or changing its tier.

The default access tier for a new general-purpose v2 storage account is set to the hot tier by default. You can change the default access tier setting when you create a storage account or after it's created. If you don't change this setting on the storage account or explicitly set the tier when uploading a blob, then a new blob is uploaded to the hot tier by default.

A blob that doesn't have an explicitly assigned tier infers its tier from the default account access tier setting. If a blob's access tier is inferred from the default account access tier setting, then the Azure portal displays the access tier as Hot (inferred) or Cool (inferred).

Changing the default access tier setting for a storage account applies to all blobs in the account for which an access tier hasn't been explicitly set. If you toggle the default access tier setting to a cooler tier in a general-purpose v2 account, then you're charged for write operations (per 10,000) for all blobs for which the access tier is inferred. You're charged for both read operations (per 10,000) and data retrieval (per GB) if you toggle to a warmer tier in a general-purpose v2 account.

>[!WARNING]
>**Must Read**
>
>[How to set a blob tier](https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-online-manage?tabs=azure-portal)
<!-- MD028/no-blanks-blockquote -->
>[!TIP]
>[Introduction](https://learn.microsoft.com/en-us/training/modules/configure-blob-storage/4-create-blob-access-tiers)
>
>[Overview](https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview)
