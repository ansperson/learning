As an administrator, you can lock an Azure subscription, resource group, or resource to protect them from accidental user deletions and modifications. __The lock overrides any user permissions.__

You can set locks that prevent either deletions or modifications. In the portal, these locks are called Delete and Read-only. In the command line, these locks are called CanNotDelete and ReadOnly.

- **CanNotDelete** means authorized users can read and modify a resource, but they can't delete it.
- **ReadOnly** means authorized users can read a resource, but they can't delete or update it. Applying this lock is similar to restricting all authorized users to the permissions that the Reader role provides.
- Unlike role-based access control (RBAC), **you use management locks to apply a restriction across all users and roles.**
- When you cancel an Azure subscription:
  - A resource lock doesn't block the subscription cancellation.
  - Azure preserves your resources by deactivating them instead of immediately deleting them.
  - Azure only deletes your resources permanently after a waiting period.

>[!TIP]
>[Lock Resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/lock-resources?tabs=json)