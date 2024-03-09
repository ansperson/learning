Tags are metadata elements that you apply to your Azure resources. They're key-value pairs that help you identify resources based on settings that are relevant to your organization. If you want to track the deployment environment for your resources, add a key named Environment. To identify the resources deployed to production, give them a value of Production. The full key-value pair is Environment = Production.

- You need to have `Microsoft.Resources/tags` permissions to manage tags
- Tag names are case-insensitive for operations; Tag values are case-sensitive.
- Resources don't inherit the tags you apply to a resource group or a subscription. To apply tags from a subscription or resource group to the resources, see [Azure Policies - tags](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-policies).
- You can use tags to group your billing data. 
- Not all resource types support tags.
- Each resource, resource group, and subscription can have a maximum of 50 tag name-value pairs. 
- Tag names can't contain these characters: `<`, `>`, `%`, `&`, `\`, `?`, `/`
- It is possible to [add tags in bulk](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-portal#add-tags-to-multiple-resources).

>[!TIP]
>[Tag Resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources)