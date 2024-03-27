# Use tags to organize your Azure resources

Tags are metadata elements that you apply to your Azure resources. They're key-value pairs that help you identify resources based on settings that are relevant to your organization. If you want to track the deployment environment for your resources, add a key named Environment. To identify the resources deployed to production, give them a value of Production. The full key-value pair is Environment = Production.

## Additional Information

- You need to have `Microsoft.Resources/tags` permissions to manage tags
- Tag names are case-insensitive for operations; Tag values are case-sensitive.
- Resources don't inherit the tags you apply to a resource group or a subscription. To apply tags from a subscription or resource group to the resources, see [Azure Policies - tags](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-policies).
- You can use tags to group your billing data.
- Not all resource types support tags.
- Each resource, resource group, and subscription can have a maximum of 50 tag name-value pairs.
- Tag names can't contain these characters: `<`, `>`, `%`, `&`, `\`, `?`, `/`
- It is possible to [add tags in bulk](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-portal#add-tags-to-multiple-resources).
- Tags are stored as plain text. Never add sensitive values to tags.

## Apply tags

Azure CLI offers two commands to apply tags: `az tag create` and `az tag update`. You need to have the Azure CLI 2.10.0 version or later. You can check your version with az version. To update or install it, see Install the Azure CLI.

The az tag create replaces all tags on the resource, resource group, or subscription. When you call the command, pass the resource ID of the entity you want to tag.

The following example applies a set of tags to a storage account:

```shell
resource=$(az resource show -g demoGroup -n demostorage --resource-type Microsoft.Storage/storageAccounts --query "id" --output tsv)
az tag create --resource-id $resource --tags Dept=Finance Status=Normal
````

If you run the command again, but this time with different tags, **notice that the earlier tags disappear**.

To add tags to a resource that already has tags, use az tag update. Set the --operation parameter to Merge.

```shell
az tag update --resource-id $resource --operation Merge --tags Dept=Finance Status=Normal
```

>[!NOTE]
>[Tag Resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources)
>
>[Apply Tags](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-cli)
