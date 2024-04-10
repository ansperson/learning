# Create Users and Groups

## Create user accounts

Every user who wants access to Azure resources needs an Azure user account. A user account has all the information required to authenticate the user during the sign-in process. Microsoft Entra ID supports three types of user accounts. The types indicate where the user is defined (in the cloud or on-premises), and whether the user is internal or external to your Microsoft Entra organization.

|User account|Description|
|--|--|
|Cloud identity|A user account with a cloud identity is defined only in Microsoft Entra ID. This type of user account includes administrator accounts and users who are managed as part of your organization. A cloud identity can be for user accounts defined in your Microsoft Entra organization, and also for user accounts defined in an external Microsoft Entra instance. When a cloud identity is removed from the primary directory, the user account is deleted.|
|Directory-synchronized identity|User accounts that have a directory-synchronized identity are defined in an on-premises Active Directory. A synchronization activity occurs via Microsoft Entra Connect to bring these user accounts in to Azure. The source for these accounts is Windows Server Active Directory.|
|Guest user|Guest user accounts are defined outside Azure. Examples include user accounts from other cloud providers, and Microsoft accounts like an Xbox LIVE account. The source for guest user accounts is Invited user. Guest user accounts are useful when external vendors or contractors need access to your Azure resources.|

## Create group accounts

A Microsoft Entra group helps organize users, which makes it easier to manage permissions. Using groups lets the resource owner (or Microsoft Entra directory owner), assign a set of access permissions to all the group members instead of having to provide the rights one by one. Groups allow you to define a security boundary, then add and remove specific users to grant or deny access with a minimum amount of effort. Even better, Microsoft Entra ID supports the ability to define membership based on rules, such as what department a user works in or the job title they have.

Microsoft Entra ID allows you to define two different types of groups.

- Security groups: These are the most common, and are used to manage member and computer access to shared resources for a group of users. For example, you can create a security group for a specific security policy. By doing it this way, you can give a set of permissions to all the members at once instead of having to add permissions to each member individually. This option requires a Microsoft Entra administrator.

- Microsoft 365 groups: These groups provide collaboration opportunities by giving members access to a shared mailbox, calendar, files, SharePoint site, and more. This option also lets you give people outside of your organization access to the group. This option is available to users as well as admins.

The membership type can be one of three values:

- Assigned (static). The group will contain specific users or groups that you select.

- Dynamic user. You can create rules based on characteristics to enable attribute-based dynamic memberships for groups. For example, if a user’s department is Sales, that user will be dynamically assigned to the Sales group. You can use security groups for either devices or users, but you can only use Microsoft 365 Groups for user groups. If the user's department changes in the future, they're automatically removed from the group. This feature requires a Microsoft Entra ID P1 license.

- Dynamic device. You can create rules based on characteristics to enable attribute-based dynamic memberships for groups. For example, if a user’s device is associated with the Service department, that device will be dynamically assigned to the Service group. You can use security groups for either devices or users, but you can only use Microsoft 365 Groups for user groups. If the device's association with a particular department changes in the future, it's automatically removed from the group. This feature requires a Microsoft Entra ID P1 license.

## Additional Information

- You can create and delete users and groups in bulk.
- Only Global administrators or User administrators have privileges to create and delete user accounts in the Azure portal.
- To complete bulk create or delete operations, the admin fills out a comma-separated values (CSV) template of the data for the user accounts.

>[!NOTE]
>[Create user accounts](https://learn.microsoft.com/en-gb/training/modules/configure-user-group-accounts/2-create-user-accounts)
>
>[Create group accounts](https://learn.microsoft.com/en-gb/training/modules/configure-user-group-accounts/5-create)
>
>[Create and manage users](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/3-users)
>
>[Create and manage groups](https://learn.microsoft.com/en-us/training/modules/manage-users-and-groups-in-aad/4-groups)
