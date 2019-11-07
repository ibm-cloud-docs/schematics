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

# Identity & Access Data Sources
{: #identity-&-access-data-sources}


## ibm_iam_service_id

Import the details of an IAM (Identity and Access Management) servicID  on IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_iam_service_id" "ds_serviceID" {
  name = "sample"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the serviceID.

### Output parameters

The following attributes are exported:

* `service_ids` - A nested block list of IAM ServiceIDs. Nested `service_ids` blocks have the following structure:
  * `id` - The unique identifier of the serviceID.
  * `bound_to` -  bound to of the serviceID.
  * `crn` -  crn of the serviceID.
  * `description` -  description of the serviceID.
  * `version` -  version of the serviceID.
  * `locked` -  lock state of the serviceID.

  



## ibm_iam_service_policy

Import the details of an IAM (Identity and Access Management) service policy on IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "ServiceId-d7bec597-4726-451f-8a63-e62e6f19c32c"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    region               = "us-south"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

data "ibm_iam_service_policy" "testacc_ds_service_policy" {
  iam_service_id = "${ibm_iam_service_policy.policy.iam_service_id}"
}

```

### Input parameters

The following arguments are supported:

* `iam_service_id` - (Required, string) The UUID of the serviceID.

### Output parameters

The following attributes are exported:

* `policies` - A nested block describing IAM Policies assigned to serviceID. Nested `policies` blocks have the following structure:
  * `id` - The unique identifier of the IAM service policy.The id is composed of \<iam_service_id\>/\<service_policy_id\>
  * `roles` -  Roles assigned to the policy.
	* `resources` -  A nested block describing the resources in the policy.
		* `service` - Service name of the policy definition. 
		* `resource_instance_id` - ID of resource instance of the policy definition.
		* `region` - Region of the policy definition.
		* `resource_type` - Resource type of the policy definition.
		* `resource` - Resource of the policy definition.
		* `resource_group_id` - The ID of the resource group.


## ibm_iam_user_policy

Import the details of an IAM (Identity and Access Management) user policy on IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service = "kms"
    region  = "us-south"
  }]
}

data "ibm_iam_user_policy" "testacc_ds_user_policy" {
  ibm_id = "${ibm_iam_user_policy.policy.ibm_id}"
}

```

### Input parameters

The following arguments are supported:

* `ibm_id` - (Required, string) The ibm id or email of user.

### Output parameters

The following attributes are exported:

* `policies` - A nested block describing IAM Policies assigned to user. Nested `policies` blocks have the following structure:
  * `id` - The unique identifier of the IAM user policy.The id is composed of \<ibm_id\>/\<user_policy_id\>
  * `roles` -  Roles assigned to the policy.
	* `resources` -  A nested block describing the resources in the policy.
		* `service` - Service name of the policy definition. 
		* `resource_instance_id` - ID of resource instance of the policy definition.
		* `region` - Region of the policy definition.
		* `resource_type` - Resource type of the policy definition.
		* `resource` - Resource of the policy definition.
		* `resource_group_id` - The ID of the resource group. 


  