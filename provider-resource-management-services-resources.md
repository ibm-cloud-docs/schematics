---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-07"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Resource Management Services Resources
{: #resource-management-services-resources}


## ibm_resource_group

Provides a resource group resource. This allows resource groups to be created, and updated. Resource group cannot be deleted by a user. When user perform `terraform destroy` it removes the terraform state information.

### Sample Terraform code

```hcl

resource "ibm_resource_group" "resourceGroup" {
  name     = "prod"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string)The name of the resource group.
* `quota_id` - (Removed, string) The id of the quota.You can [refer to a quota by name using a data source](../d/resource_quota.html).
* `tags` - (Optional, array of strings) Tags associated with the resource group instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new resource group.
* `default` - Specifies whether its default resource group or not.
* `state` - State of the resource group.


### Import

ibm_resource_group can be imported using resource group id, eg

```
$ terraform import ibm_resource_group.example 5ffda12064634723b079acdb018ef308
```


## ibm_resource_instance

Provides a Resource Instance resource. This allows Resource Instances to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_resource_group" "group" {
  name = "test"
}

resource "ibm_resource_instance" "resource_instance" {
  name              = "test"
  service           = "cloud-object-storage"
  plan              = "lite"
  location          = "global"
  resource_group_id = "${data.ibm_resource_group.group.id}"
  tags              = ["tag1", "tag2"]

  parameters = {
    "HMAC" = true
  }
  //User can increase timeouts 
  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
}
```



The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify the resource instance.
* `service` - (Required, string) The name of the service offering. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `plan` - (Required, string) The name of the plan type supported by service. You can retrieve the value by running the `ibmcloud catalog service <servicename>` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `location` - (Required, string) Target location or environment to create the resource instance.
* `resource_group_id` - (Optional, string) The ID of the resource group where you want to create the service. You can retrieve the value from data source `ibm_resource_group`. If not provided creates the service in default resource group.
* `tags` - (Optional, array of strings) Tags associated with the instance.
* `parameters` - (Optional, map) Arbitrary parameters to create instance. The value must be a JSON object.
* `guid`- (Optional, string) Guid of the resource instance.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new resource instance.
* `status` - Status of resource instance.

### Timeouts

ibm_resource_instance provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for Creating Instance.
* `update` - (Default 10 minutes) Used for Updating Instance.
* `delete` - (Default 10 minutes) Used for Deleting Instance.





## ibm_resource_key

Provides a resource key resource. This allows resource keys to be created, and deleted.

### Sample Terraform code

```hcl
data "ibm_resource_instance" "resource_instance" {
  name = "myobjectsotrage"
}

resource "ibm_resource_key" "resourceKey" {
  name                 = "myobjectkey"
  role                 = "Viewer"
  resource_instance_id = "${data.ibm_resource_instance.resource_instance.id}"

  //User can increase timeouts 
  timeouts {
    create = "15m"
    delete = "15m"
  }
}
```



The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify a resource key.
* `role` - (Required, string) Name of the user role. Valid roles are Writer, Reader, Manager, Administrator, Operator, Viewer, Editor.
* `parameters` - (Optional, map) Arbitrary parameters to pass. Must be a JSON object.
* `resource_instance_id` - (Optional, string) The id of the resource instance associated with the resource key.  
 **NOTE**: Conflicts with `resource_alias_id`.
* `resource_alias_id` - (Optional, string) The id of the resource alias associated with the resource key.  
 **NOTE**: Conflicts with `resource_instance_id`.
* `tags` - (Optional, array of strings) Tags associated with the resource key instance.
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new resource key.
* `credentials` - The credentials associated with the key.
* `status` - Status of resource key.

### Timeouts

ibm_resource_key provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for Creating Key.
* `delete` - (Default 10 minutes) Used for Deleting Key.

