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

# Resource Management Data Sources
{: #resource-management-data-sources}


## ibm_resource_group

Import the details of an existing IBM resource Group as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

Import the resource group by name

```hcl
data "ibm_resource_group" "group" {
  name = "test"
}
```

Import the default resource group

```hcl
data "ibm_resource_group" "group" {
  is_default = "true"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Optional, string) The name of the IBM Cloud resource group. You can retrieve the value by running the `ibmcloud resource groups` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).  
  **NOTE**: Conflicts with `is_default`.

* `is_default` - (Optional, boolean) Specifies whether you want to import default resource group. The default value is `false`.  
  **NOTE**: Conflicts with `name`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the resource group.  



## ibm_resource_instance

Import the details of an existing IBM resource instance from IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

data "ibm_resource_instance" "testacc_ds_resource_instance" {
  name              = "myobjectstore"
  location          = "global"
  resource_group_id = "${data.ibm_resource_group.group.id}"
  service           = "cloud-object-storage"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the resource instance.

* `resource_group_id` - (Optional, string) The id of the resource group where the resource instance exists. If not provided it takes the default resource group.

* `location` - (Optional, string) The location or the environment in which instance exists.

* `service` - (Optional, string) The service type of the instance. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the resource instance.
* `status` - The status of resource instance.
* `plan` - The plan for the service offering used by this resource instance.



## ibm_resource_key

Import the details of an existing IBM resource key from IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_resource_key" "resourceKeydata" {
  name                  = "myobjectKey"
  resource_instance_id  = "${ibm_resource_instance.resource.id}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the resource key. You can retrieve the value by running the `ibmcloud resource service-keys` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `resource_instance_id` - (Optional, string) The id of the resource instance that the resource key is associated with. You can retrieve the value by running the `ibmcloud resource service-instances` command in the IBM Cloud CLI.  
  **NOTE**: Conflicts with `resource_alias_id`.
* `resource_alias_id` - (Optional, string) The id of the resource alias that the resource key is associated with. You can retrieve the value by running the `ibmcloud resource service-alias` command in the IBM Cloud CLI.  
  **NOTE**: Conflicts with `resource_instance_id`.
* `most_recent` - (Optional, boolean) If there are multiple resource keys, you can set this argument to `true` to import only the most recently created key.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the resource key.
* `credentials` - The credentials associated with the key.
* `role` - The user role.
* `status` - Status of resource key.  



## ibm_resource_quota

Import the details of an existing quota for an IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_resource_quota" "rsquotadata" {
  name = "Trial Quota"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the quota for the IBM Cloud resource. You can retrieve the value by running the `ibmcloud resource quotas` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the quota.
* `type` - Type of the quota.
* `max_apps` - Defines the total app limit.
* `max_instances_per_app` - Defines the total instances limit per app.
* `max_app_instance_memory` - Defines the total memory of app instance.
* `total_app_memory` - Defines the total memory for app.
* `max_service_instances` - Defines the total service instances limit.
* `vsi_limit` - Defines the VSI limit.