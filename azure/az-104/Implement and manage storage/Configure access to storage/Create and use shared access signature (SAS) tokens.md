# Delegate access by using a shared access signature

A shared access signature (SAS) is a URI that grants restricted access rights to Azure Storage resources. You can provide a shared access signature to clients who shouldn't be trusted with your storage account key but who need access to certain storage account resources. By distributing a SAS URI to these clients, you can grant them access to a resource for a specified period of time, with a specified set of permissions.

The URI query parameters that compose the SAS token incorporate all of the information necessary to grant controlled access to a storage resource. A client who has the SAS can make a request against Azure Storage by using just the SAS URI. The information in the
SAS token is used to authorize the request.

>[!IMPORTANT]
Shared access signatures are keys that grant permissions to storage resources, and you should protect them just as you would protect an account key. It's important to protect a SAS from malicious or unintended use. Use discretion in distributing a SAS, and have a plan in place for revoking a compromised SAS. Operations that use shared access signatures should be performed only over an HTTPS connection, and SAS URIs should be distributed only on a secure connection, such as HTTPS.
<!-- MD028/no-blanks-blockquote -->

## Create an **account** SAS

As of version 2015-04-05, Azure Storage supports creating a new type of shared access signature (SAS) at the level of the storage account. By creating an account SAS, you can:

- Delegate access to service-level operations that aren't currently available with a service-specific SAS, such as the Get/Set Service Properties and Get Service Stats operations.
- Delegate access to more than one service in a storage account at a time. For example, you can delegate access to resources in both Azure Blob Storage and Azure Files by using an account SAS.
- Delegate access to write and delete operations for containers, queues, tables, and file shares, which are not available with an object-specific SAS.
- Specify an IP address or a range of IP addresses from which to accept requests.
- Specify the HTTP protocol from which to accept requests (either HTTPS or HTTP/HTTPS).

### Account SAS URI example

The following example shows a Blob service URI with an account SAS token appended to it. The account SAS token provides permissions to the service, container, and objects. The table breaks down each part of the URI:

```shell
https://blobsamples.blob.core.windows.net/?sv=2022-11-02&ss=b&srt=sco&sp=rwlc&se=2023-05-24T09:51:36Z&st=2023-05-24T01:51:36Z&spr=https&sig=<signature>
```

Each part of the URI is described in the following table:

|Name|SAS portion|Description|
|--|--|--|
|Resource URI|`https://myaccount.blob.core.windows.net/?restype=service&comp=properties`|The service endpoint, with parameters for getting service properties (when called with GET) or setting service properties (when called with SET). Based on the value of the signed services field (ss), this SAS can be used with either Blob Storage or Azure Files.|
|Delimiter|`?`|The delimiter that precedes the query string. The delimiter is not part of the SAS token.|
|Storage services version|`sv=2022-11-02`|For Azure Storage services version 2012-02-12 and later, this parameter indicates which version to use.|
|Services|`ss=b`|The SAS applies to the Blob services.|
|Resource types|`srt=sco`|The SAS applies to service-level, container-level, and object-level operations.|
|Permissions|`sp=rwlc`|The permissions grant access to read, write, list, and create operations.|
|Start time|`st=2019-08-01T22%3A18%3A26Z`|Specified in UTC time. If you want the SAS to be valid immediately, omit the start time.|
|Expiry time|`se=2019-08-10T02%3A23%3A26Z`|Specified in UTC time.|
|Protocol|`spr=https`|Only requests that use HTTPS are permitted.|
|Signature|`sig=<signature>`|Used to authorize access to the blob. The signature is an HMAC that's computed over a string-to-sign and key by using the SHA256 algorithm, and then encoded by using Base64 encoding.|

## Create a **service** SAS

A service shared access signature (SAS) delegates access to a resource in just one of the storage services: Azure Blob Storage, Azure Queue Storage, Azure Table Storage, or Azure Files. The URI for a service-level SAS consists of the URI to the resource for which the SAS will delegate access, followed by the SAS token.

The SAS token is the query string that includes all the information that's required to authorize a request. The token specifies the resource that a client may access, the permissions granted, and the time period during which the signature is valid.

A SAS can also specify the supported IP address or address range from which requests can originate, the supported protocol with which a request can be made, or an optional access policy identifier that's associated with the request.

Finally, every SAS token includes a signature.

### Service SAS example

The following example shows a blob URI with a service SAS token appended to it. The service SAS token provides read and write permissions to the blob.

```shell
https://myaccount.blob.core.windows.net/sascontainer/blob1.txt?sp=rw&st=2023-05-24T01:13:55Z&se=2023-05-24T09:13:55Z&sip=168.1.5.60-168.1.5.70&spr=https&sv=2022-11-02&sr=b&sig=<signature>
```

Each part of the URI is described in the following table:

| Name | SAS portion | Description |
|---|---|---|
|Resource URI|`https://myaccount.blob.core.windows.net/sascontainer/blob1.txt`|The address of the blob. We highly recommend that you use HTTPS.|
|Delimiter|`?`| The delimiter that precedes the query string. The delimiter is not part of the SAS token.|
|Permissions|`sp=rw`| The permissions granted by the SAS include Read (r) and Write (w).|
|Start time|`st=2023-05-24T01:13:55Z`| Specified in UTC time. If you want the SAS to be valid immediately, omit the start time.|
|Expiration time|`se=2023-05-24T09:13:55Z`| Specified in UTC time.|
|IP range|`sip=168.1.5.60-168.1.5.70`| The range of IP addresses from which a request will be accepted.|
|Protocol|`spr=https`| Only requests that use HTTPS are permitted.|
|Azure Storage version|`sv=2023-05-24`| For Azure Storage version 2012-02-12 and later, this parameter indicates the version to use.|
|Resource|`sr=b`| The resource is a blob.|
|Signature|`sig=<signature>`|Used to authorize access to the blob. The signature is an HMAC that's computed over a string-to-sign and key by using the SHA256 algorithm, and then encoded by using Base64 encoding.|

## Lifetime and revocation of a shared access signature

Shared access signatures grant users access rights to storage account resources. When you're planning to use a SAS, think about the lifetime of the SAS and whether your application might need to revoke access rights under certain circumstances.

### Ad hoc SAS versus stored access policy

A service SAS can take one of two forms:

- **Ad hoc SAS:** When you create an ad hoc SAS, the start time, expiration time, and permissions for the SAS are all specified in the SAS URI (or implied, if the start time is omitted). Any type of SAS can be an ad hoc SAS.

You can manage the lifetime of an ad hoc SAS by using the signedExpiry field. If you want to continue to grant a client access to the resource after the expiration time, you must issue a new signature. We recommend that you keep the lifetime of a shared access signature short. Prior to version 2012-02-12, a shared access signature not associated with a stored access policy could not have an active period that exceeded one hour.

- **SAS with stored access policy:** A stored access policy is defined on a resource container, which can be a blob container, table, queue, or file share. You can use the stored access policy to manage constraints for one or more shared access signatures. When you associate a SAS with a stored access policy, the SAS inherits the constraints (that is, the start time, expiration time, and permissions) that are defined for the stored access policy.

The stored access policy is represented by the signedIdentifier field on the URI. A stored access policy provides an additional measure of control over one or more shared access signatures, including the ability to revoke the signature if needed.

### Revoke a SAS

Because a SAS URI is a URL, anyone who obtains the SAS can use it, regardless of who originally created it. If a SAS is published publicly, it can be used by anyone in the world. A SAS grants access to resources to anyone who possesses it until one of four things happens:

- The expiration time that's specified on an ad hoc SAS is reached.
- The expiration time that's specified on the stored access policy referenced by the SAS is reached, if a stored access policy is referenced and the access policy specifies an expiration time.
- The expiration time can be reached either because the interval elapses or because you've modified the stored access policy to have an expiration time in the past, which is one way to revoke the SAS.
- The stored access policy that's referenced by the SAS is deleted, which revokes the SAS. If Azure Storage can't locate the stored access policy that's specified in the shared access signature, the client can't access the resource that's indicated by the URI.
- **If you re-create the stored access policy with exactly the same name as the deleted policy, all existing SAS tokens will again be valid, according to the permissions associated with that stored access policy. This assumes that the expiration time on the SAS has not passed. If you intend to revoke the SAS, be sure to use a different name when you re-create the access policy with an expiration time in the future.**
- The account key that was used to create the SAS is regenerated. Regenerating an account key causes all application components that use that key to fail to authorize until they're updated to use either the other valid account key or the newly regenerated account key. Regenerating the account key is the only way to immediately revoke an ad hoc SAS.

>[!TIP]
A shared access signature URI is associated with the account key that's used to create the signature and the associated stored access policy, if applicable. If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.
<!-- MD028/no-blanks-blockquote -->

As a best practice, we recommend that you use a stored access policy with a service SAS. If you choose not to use a stored access policy, be sure to keep the period during which the ad hoc SAS is valid short. For more information about associating a service SAS with a stored access policy, see [Define a stored access policy.](https://learn.microsoft.com/en-us/rest/api/storageservices/define-stored-access-policy)

## Create a user **delegation** SAS

You can secure a shared access signature (SAS) token for access to a container, directory, or blob by using either Microsoft Entra credentials or an account key. A SAS that's secured with Microsoft Entra credentials is called a user delegation SAS. As a security best practice, we recommend that you use Microsoft Entra credentials when possible, rather than the account key, which can be more easily compromised. When your application design requires shared access signatures, use Microsoft Entra credentials to create a user delegation SAS to help ensure better security.

Every SAS is signed with a key. To create a user delegation SAS, you must first request a user delegation key, which you then use to sign the SAS. The user delegation key is analogous to the account key that's used to sign a service SAS or an account SAS, except that it relies on your Microsoft Entra credentials. To request the user delegation key, call the Get User Delegation Key operation. You can then use the user delegation key to create the SAS.

A user delegation SAS is supported for Azure Blob Storage and Azure Data Lake Storage Gen2. **Stored access policies are not supported for a user delegation SAS.**

For information about using your **account key** to secure a SAS, see **Create a service SAS** and **Create an account SAS**.

### User delegation SAS example

The following example shows a blob URI with a user delegation SAS token appended to it. The user delegation SAS token provides read and write permissions to the blob.

```shell
https://myaccount.blob.core.windows.net/sascontainer/blob1.txt?sp=rw&st=2023-05-24T01:13:55Z&se=2023-05-24T09:13:55Z&skoid=<object-id>&sktid=<tenant-id>&skt=2023-05-24T01:13:55Z&ske=2023-05-24T09:13:55Z&sks=b&skv=2022-11-02&sip=168.1.5.60-168.1.5.70&spr=https&sv=2022-11-02&sr=b&sig=<signature>
```

Each part of the URI is described in the following table:

|Name|SAS portion|Description|
|---|---|---|
|Resource URI|`https://myaccount.blob.core.windows.net/sascontainer/blob1.txt`|The address of the blob. We highly recommend that you use HTTPS.|
|Delimiter|`?`|The delimiter that precedes the query string. The delimiter is not part of the SAS token.|
|Permissions|`sp=rw`|The permissions granted by the SAS include Read (r) and Write (w).|
|Start time|`st=2023-05-24T01:13:55Z`|Specified in UTC time. If you want the SAS to be valid immediately, omit the start time.|
|Expiration time|`se=2023-05-24T09:13:55Z`|Specified in UTC time.|
|Object ID|`skoid=<object-id>`|A Microsoft Entra security principal.|
|Tenant ID|`sktid=<tenant-id>`|The Microsoft Entra tenant where the security principal is registered.|
|Key start time|`skt=2023-05-24T01:13:55Z | The start of the lifetime of the user delegation key.|
|Key expiry time|`ske=2023-05-24T09:13:55Z | The end of the lifetime of the user delegation key.|
|Key service|`sks=b`|Only the Blob service is supported for the service value.|
|Key version|`skv=2022-11-02`|The storage service version that was used to get the user delegation key.|
|IP range|`sip=168.1.5.60-168.1.5.70`|The range of IP addresses from which a request will be accepted.|
|Protocol|`spr=https`|Only requests that use HTTPS are permitted.|
|Blob service version|`sv=2022-11-02`|For Azure Storage version 2012-02-12 and later, this parameter indicates the version to use.|
|Resource|`sr=b`|The resource is a blob.|
|Signature|`sig=<signature>`|Used to authorize access to the blob. The signature is an HMAC that's computed over a string-to-sign and key by using the SHA256 algorithm, and then encoded by using Base64 encoding.|

### Revoke a user delegation SAS

If you believe that a SAS has been compromised, you should revoke it. You can revoke a user delegation SAS either by revoking the user delegation key, or by changing or removing RBAC role assignments for the security principal that's used to create the SAS.

>[!TIP]
Both the user delegation key and RBAC role assignments are cached by Azure Storage, so there may be a delay between when you initiate the process of revocation and when an existing user delegation SAS becomes invalid.
<!-- MD028/no-blanks-blockquote -->

#### Revoke the user delegation key

You can revoke the user delegation key by calling the Revoke User Delegation Keys operation. When you revoke the user delegation key, any shared access signatures that rely on that key become invalid. You can then call the Get User Delegation Key operation again and use the key to create new shared access signatures. This is the quickest way to revoke a user delegation SAS.

#### Change or remove role assignments

You can change or remove the RBAC role assignment for the security principal that's used to create the SAS. When a client uses the SAS to access a resource, Azure Storage verifies that the security principal whose credentials were used to secure the SAS has the required permissions to the resource.

>[!NOTE]
>[Delegate access with shared access signature](https://learn.microsoft.com/en-us/rest/api/storageservices/delegate-access-with-shared-access-signature)
>
>[Create account sas](https://learn.microsoft.com/en-us/rest/api/storageservices/create-account-sas)
>
>[Create service sas](https://learn.microsoft.com/en-us/rest/api/storageservices/create-service-sas)
>
>[Create user delegation sas](https://learn.microsoft.com/en-us/rest/api/storageservices/create-user-delegation-sas)
