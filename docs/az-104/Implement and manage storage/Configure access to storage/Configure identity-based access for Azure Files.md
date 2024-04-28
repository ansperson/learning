# Overview of Azure Files identity-based authentication options for SMB access

This article explains how Azure file shares can use domain services, either on-premises or in Azure, to support identity-based access to Azure file shares over SMB. Enabling identity-based access for your Azure file shares allows you to replace existing file servers with Azure file shares without replacing your existing directory service, maintaining seamless user access to shares.

## Glossary

It's helpful to understand some key terms relating to identity-based authentication for Azure file shares:

- **Kerberos authentication**
  Kerberos is an authentication protocol that's used to verify the identity of a user or host. For more information on Kerberos, see [Kerberos Authentication Overview](https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview).

- **Server Message Block (SMB) protocol**
  SMB is an industry-standard network file-sharing protocol. For more information on SMB, see [Microsoft SMB Protocol and CIFS Protocol Overview](https://learn.microsoft.com/en-us/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview).

- **Microsoft Entra ID**
  Microsoft Entra ID (formerly Azure AD) is Microsoft's multi-tenant cloud-based directory and identity management service. Microsoft Entra ID combines core directory services, application access management, and identity protection into a single solution.

- **Microsoft Entra Domain Services**
  Microsoft Entra Domain Services provides managed domain services such as domain join, group policies, LDAP, and Kerberos/NTLM authentication. These services are fully compatible with Active Directory Domain Services. For more information, see [Microsoft Entra Domain Services](https://learn.microsoft.com/en-us/azure/active-directory-domain-services/overview).

- **On-premises Active Directory Domain Services (AD DS)**
  On-premises Active Directory Domain Services (AD DS) integration with Azure Files provides the methods for storing directory data while making it available to network users and administrators. Security is integrated with AD DS through logon authentication and access control to objects in the directory. With a single network logon, administrators can manage directory data and organization throughout their network, and authorized network users can access resources anywhere on the network. AD DS is commonly adopted by enterprises in on-premises environments or on cloud-hosted VMs, and AD DS credentials are used for access control. For more information, see [Active Directory Domain Services Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

- **Azure role-based access control (Azure RBAC)**
  Azure RBAC enables fine-grained access management for Azure. Using Azure RBAC, you can manage access to resources by granting users the fewest permissions needed to perform their jobs. For more information, see [What is Azure role-based access control?](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview)

- **Hybrid identities**
  Hybrid user identities are identities in AD DS that are synced to Microsoft Entra ID using either the on-premises Microsoft Entra Connect Sync application or Microsoft Entra Connect cloud sync, a lightweight agent that can be installed from the Microsoft Entra Admin Center.

## Supported authentication scenarios

Azure Files supports identity-based authentication over SMB through the following methods. **You can only use one method per storage account.**

- **On-premises AD DS authentication:** On-premises AD DS-joined or Microsoft Entra Domain Services-joined Windows machines can access Azure file shares with on-premises Active Directory credentials that are synched to Microsoft Entra ID over SMB. Your client must have unimpeded network connectivity to your AD DS. If you already have AD DS set up on-premises or on a VM in Azure where your devices are domain-joined to your AD, you should use AD DS for Azure file shares authentication.
- **Microsoft Entra Domain Services authentication:** Cloud-based, Microsoft Entra Domain Services-joined Windows VMs can access Azure file shares with Microsoft Entra credentials. In this solution, Microsoft Entra ID runs a traditional Windows Server AD domain on behalf of the customer, which is a child of the customer’s Microsoft Entra tenant.
- **Microsoft Entra Kerberos for hybrid identities:** Using Microsoft Entra ID for authenticating hybrid user identities allows Microsoft Entra users to access Azure file shares using Kerberos authentication. This means your end users can access Azure file shares over the internet without requiring network connectivity to domain controllers from Microsoft Entra hybrid joined and Microsoft Entra joined VMs. Cloud-only identities aren't currently supported.
- **AD Kerberos authentication for Linux clients:** Linux clients can use Kerberos authentication over SMB for Azure Files using on-premises AD DS or Microsoft Entra Domain Services.

## How it works

Azure file shares use the Kerberos protocol to authenticate with an AD source. When an identity associated with a user or application running on a client attempts to access data in Azure file shares, the request is sent to the AD source to authenticate the identity. If authentication is successful, it returns a Kerberos token. The client sends a request that includes the Kerberos token, and Azure file shares use that token to authorize the request. Azure file shares only receive the Kerberos token, not the user's access credentials.

You can enable identity-based authentication on your new and existing storage accounts using one of three AD sources: AD DS, Microsoft Entra Domain Services, or Microsoft Entra Kerberos (hybrid identities only). Only one AD source can be used for file access authentication on the storage account, which applies to all file shares in the account. Before you can enable identity-based authentication on your storage account, you must first set up your domain environment.

## Configure share-level permissions for Azure Files

Once you've enabled an AD source on your storage account, you must do one of the following to access the file share:

- Set a default share-level permission that applies to all authenticated users and groups
- Assign built-in Azure RBAC roles to users and groups, or
- Configure custom roles for Microsoft Entra identities and assign access rights to file shares in your storage account.

The assigned share-level permission allows the granted identity to get access to the share only, nothing else, not even the root directory. You still need to separately configure directory and file-level permissions.

## Additional Information

- In my humble opinion CIFs protocol is almost obsolete.
- On Linux cifs-utils package can handle both CIFS and SMB protocol implementation.
- The Server Message Block (SMB) Protocol is a network file sharing protocol, and as implemented in Microsoft Windows is known as Microsoft SMB Protocol. The set of message packets that defines a particular version of the protocol is called a dialect. The Common Internet File System (CIFS) Protocol is a dialect of SMB. Both SMB and CIFS are also available on VMS, several versions of Unix, and other operating systems.

>[!NOTE]
>[Overview of Azure Files identity-based authentication options for SMB access](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-active-directory-overview)
