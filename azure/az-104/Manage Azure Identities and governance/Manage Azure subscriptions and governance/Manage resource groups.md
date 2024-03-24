# Azure resource groups

>[!IMPORTANT]
>[ARM](../Extras/Azure%20resoruce%20manager.md) (Azure Resource Manager).

A resource group is a container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization. Generally, add resources that share the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.

The resource group stores metadata about the resources. Therefore, when you specify a location for the resource group, you are specifying where that metadata is stored. For compliance reasons, you may need to ensure that your data is stored in a particular region.

## Additional Information

- You can move one resource group into another subscription.
- You can apply [locks](./Configure%20resource%20locks.md) to a resource group.
- **Each resource can exist in only one resource group.**
- A resource can connect to resources in other resource groups. This scenario is common when the two resources are related but don't share the same lifecycle. For example, you can have a web app that connects to a database in a different resource group.
- When you delete a resource group, all resources in the resource group are also deleted.
- You can deploy up to 800 instances of a resource type in each resource group.
- Some resources can exist outside of a resource group. These resources are deployed to the subscription, management group, or tenant. Only specific resource types are supported at these scopes.
- To reduce the impact of regional outages, we recommend that you locate resources in the same region as the resource group.
- **Why does a resource group need a location:** The resource group stores metadata about the resources. When you specify a location for the resource group, you're specifying where that metadata is stored. For compliance reasons, you may need to ensure that your data is stored in a particular region.
- You can move resources created in one resource group to another resource group, subscription or region without recreating them, **beware this will create a new resource ID.**

>[!IMPORTANT]
>[Create resource groups](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
<!-- MD028/no-blanks-blockquote -->
>[!NOTE]
>[Overview](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal)
