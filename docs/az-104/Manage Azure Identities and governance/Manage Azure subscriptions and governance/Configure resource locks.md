# Lock your resources to protect your infrastructure

As an administrator, you can lock an **Azure subscription**, **resource group**, or **resource** to protect them from accidental user deletions and modifications. **The lock overrides any user permissions.**

You can set locks that prevent either deletions or modifications. In the portal, these locks are called Delete and Read-only. In the command line, these locks are called CanNotDelete and ReadOnly.

- **CanNotDelete** means authorized users can read and modify a resource, but they can't delete it.
- **ReadOnly** means authorized users can read a resource, but they can't delete or update it. Applying this lock is similar to restricting all authorized users to the permissions that the Reader role provides.

Unlike role-based access control (RBAC), **you use management locks to apply a restriction across all users and roles.**

## Configure locks

### PowerShell

```powershell
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

### Azure CLI

```shell
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

## Additional Information

- If you have a Delete lock on a resource and attempt to delete its resource group, the feature blocks the whole delete operation. Even if the resource group or other resources in the resource group are unlocked, the deletion doesn't happen. You never have a partial deletion.
- When you cancel an Azure subscription:
  - A resource lock doesn't block the subscription cancellation.
  - Azure preserves your resources by deactivating them instead of immediately deleting them.
  - Azure only deletes your resources permanently after a waiting period.

>[!IMPORTANT]
>[Configure Locks](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/lock-resources?tabs=json#configure-locks)
<!-- MD028/no-blanks-blockquote -->
>[!NOTE]
>[Overview](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/lock-resources)
