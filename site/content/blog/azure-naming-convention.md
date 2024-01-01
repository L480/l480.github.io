+++
title = "Azure Naming Convention: The Kubernetes Approach"
date = "2020-09-08"
tags = [
    "azure",
]
+++

Almost all of my customers spend days and weeks thinking about and creating naming conventions for their Azure resources. However, it usually takes a very short time until these naming conventions become inconsistent, because some names are not suitable for certain resource types or some resource types are not considered in the naming convention at all. It often takes a while until they notice that a user-friendly naming convention does not make any sense in a cattle environment.

Instead of using user-friendly resource names I want to show you how to use random strings with very less user-friendly information for your resource names, and resource tags for a user-friendly display name and other kind of information. The idea is based on how Kubernetes creates pod names.

Let me show you the pros and cons of random names compared to user-friendly names.

## User-Friendly Names

### Pros

- Resource names (mostly) contain human-readable information

### Cons

- Unable to rename resources
- Must be compatible with all resource types
- Time consuming to create and maintain
- Each resource type has it's own naming convention
- Naming convention must provide unused global unique names
- All users and departments must stick to the latest version of the naming convention
- Difficult blue-green deployments, due to conflicting names

## Random Names

### Pros

- Easier to rename resources through resource tags
- Compatible with all resource types
- Easy to apply and maintain
- One naming convention for all resource types
- Names are always globally unique
- Easy to spread across all users and departments
- Always one or more user-friendly names as resource tags
- Easier blue-green deployments

### Cons

- Resource names does not contain much human-readable information

## How does it look like?

Instead of thinking about fancy naming constructs, just think about the most suitable term which describes your service the best and concatenate that term with a random string, until you have reached 16 characters in total (or more, depending on the resource type limitations). The rest of the information should be added as resource tags, such as a user-friendly display name, a cost center etc.

The name now looks a bit like a Kubernetes pod name.

So why don't we just use random strings only? The only reason for a bit of human-readable information is, to better identity resources in dropdown menus and other parts of UIs, such as in the Azure Portal, Azure DevOps, Datadog etc.

![Random String Naming Convention on Azure](/azure-naming-convention1.png)

The above resources are entirely deployed through Terraform, which makes the generation of random strings even easier, thanks to the built-in Terraform [uuidv5](https://www.terraform.io/docs/configuration/functions/uuidv5.html), [sha256](https://www.terraform.io/docs/configuration/functions/sha256.html), and [substr](https://www.terraform.io/docs/configuration/functions/substr.html) functions.

```hcl
locals {
  name = "myapp"
  uuid = "9ee29d93-35c8-4447-84f7-7d929e81fbd0"
}

resource "azurerm_resource_group" "rg" {
  name     = substr("${local.name}-${sha256(uuidv5(local.uuid, "rg"))}", 0, 15)
  location = "West Europe"

  tags = {
    displayName = "My Demo App"
    costCenter  = "1000"
    tfState     = "demo-app.json"
  }
}
```

## FAQ

### Why don't you just use the naming convention from the [Microsoft Cloud Adoption Framework (CAF)](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)?

The naming best practices from the cloud adoption framework are guidelines (like how to abbreviate a certain resource type etc.), rather than a ready to use naming convention. Several resource types are not even considered in the cloud adoption framework (e.g. subnet names, NSG rule names, VNet peering names and many more). From my experience it's quiet hard to come up with a meaningful and (more importantly) consistent naming convention which also fits your organizational structures if you stick entirely to the cloud adoption framework.
