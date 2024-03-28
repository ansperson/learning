# What is Azure Policy?

**Azure Policy helps to enforce organizational standards and to assess compliance at-scale.** Through its compliance dashboard, it provides an aggregated view to evaluate the overall state of the environment, with the ability to drill down to the per-resource, per-policy granularity. It also helps to bring your resources to compliance through bulk remediation for existing resources and automatic remediation for new resources.

Common use cases for Azure Policy include implementing governance for resource consistency, regulatory compliance, security, cost, and management. Policy definitions for these common use cases are already available in your Azure environment as built-ins to help you get started.

Specifically, some useful governance actions you can enforce with Azure Policy include:

- Ensuring your team deploys Azure resources only to allowed regions
- Enforcing the consistent application of taxonomic tags
- Requiring resources to send diagnostic logs to a Log Analytics workspace

**It's important to recognize that with the introduction of Azure Arc, you can extend your policy-based governance across different cloud providers and even to your local datacenters.**

All Azure Policy data and objects are encrypted at rest. For more information.

## Overview

Azure Policy evaluates resources and actions in Azure by comparing the properties of those resources to business rules. These business rules, described in JSON format, are known as policy definitions. To simplify management, several business rules can be grouped together to form a policy initiative (sometimes called a policySet). Once your business rules have been formed, **the policy definition or initiative is assigned to any scope of resources that Azure supports, such as management groups, subscriptions, resource groups, or individual resources.** The assignment applies to all resources within the Resource Manager scope of that assignment. Subscopes can be excluded, if necessary. For more information, see [Scope in Azure Policy](../Extras/Understand%20management%20scope.md).

Azure Policy uses a JSON format to form the logic the evaluation uses to determine whether a resource is compliant or not. Definitions include metadata and the policy rule. The defined rule can use functions, parameters, logical operators, conditions, and property aliases to match exactly the scenario you want. The policy rule determines which resources in the scope of the assignment get evaluated.

## Create a custom policy

You can create your own [custom policy](https://learn.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage#implement-a-new-custom-policy) definition, you can duplicate or export a preexiting policy definition and make your changes

- You can copy the resource template creation of a ARM execution and make the changes you need into the policy file in order to ease the policy creation.

## Example

```json
{
    "properties": {
        "displayName": "Deny unencrypted HTTPS storage accts",
        "description": "Deny unencrypted HTTPS storage accts",
        "mode": "all",
         "parameters": {
            "effectType": {
                "type": "string",
                "defaultValue": "Deny",
                "allowedValues": [
                    "Deny",
                    "Disabled"
                ],
                "metadata": {
                    "displayName": "Effect",
                    "description": "Enable or disable the execution of the policy"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                    },
                    {
                        "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
                        "notEquals": "true"
                    }
                ]
            },
            "then": {
                "effect": "[parameters('effectType')]"
            }
        }
    }
}
```

## Additional Information

- On `Definitions` you can read all policies definitions.
- A scope is a conjunction of subscription plus resource group.
- You can select resources to be excluded from the policy itself.
- Policy enforcement if enable it will prevent you to create a resource that goes against a policy, if disable you can create the resource but it will become noncompliant.
- By default, this assignment will only take effect on newly created resources.
- You can see the polices applied to a resource group also on the resource group page under policies page. You can check the policy compliance also there.
- The policy can have two effect: audit do not prevent the creation only audit it can make it noncompliant or deny which deny the resource creation.
- Policies with audit options are not evaluated in real time meaning it can take some time to see the noncompliant status on the dashboard.
- You can create a policy in of "Allowed locations for resource groups" specifying which region is allowed to create resoruce groups

>[!IMPORTANT]
>[Create a policy assignment](https://learn.microsoft.com/en-gb/azure/governance/policy/assign-policy-portal)
<!-- MD028/no-blanks-blockquote -->
>[!NOTE]
>[Policy Overview](https://learn.microsoft.com/en-us/azure/governance/policy/overview)
