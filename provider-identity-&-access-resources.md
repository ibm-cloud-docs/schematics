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

# Identity & Access Resources
{: #identity-&-access-resources}


## ibm_iam_access_group

Provides a resource for IAM access group. This allows access group to be created, updated and deleted.

### Sample Terraform code

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name        = "test"
  description = "New access group"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the access group.
* `description` - (Optional, string) Description of the access group.
* `tags` - (Optional, array of strings) Tags associated with the IAM access group.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the access group.
* `version` - Version of the access group.



## ibm_iam_access_group_members


~> **WARNING:** Multiple ibm_iam_access_group_members resources with the same group name will produce inconsistent behavior!

Provides a resource for IAM access group members. This allows access group members to be created, updated and deleted.

### Sample Terraform code

```hcl
resource "ibm_iam_access_group" "accgroup" {
  name = "testgroup"
}

resource "ibm_iam_service_id" "serviceID" {
  name = "testserviceid"
}

resource "ibm_iam_access_group_members" "accgroupmem" {
  access_group_id = "${ibm_iam_access_group.accgroup.id}"
  ibm_ids         = ["test@in.ibm.com"]
  iam_service_ids = ["${ibm_iam_service_id.serviceID.id}"]
}

```

### Input parameters

The following arguments are supported:

* `access_group_id` - (Required, string) ID of the access group.
* `ibm_ids` - (Optional, array of strings) List of IBMid's.
* `iam_service_ids` - (Optional, array of strings) List of serviceID's.  
  

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the access group members. The id is composed of \<iam_access_group_id\>/\<randomid\>
* `members` - List of members attached to the access group.
Nested `members` blocks have the following structure:
  * `iam_id` - The IBMid or Service Id of the member
  * `type` - The type of the member, either "user" or "service".

### Import

ibm_iam_access_group_members can be imported using access group ID and random id, eg

```
$ terraform import ibm_iam_access_group_members.example AccessGroupId-5391772e-1207-45e8-b032-2a21941c11ab/2018-10-04 06:27:40.041599641 +0000 UTC
```


## ibm_access_group_id

Provides a resource for IAM Access Group Policy. This allows access group policy to be created, updated and deleted.

### Sample Terraform code

#### Access Group Policy for All Identity and Access enabled services 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]
}

```

#### Access Group Policy for All Identity and Access enabled services within a resource group

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Operator", "Writer"]

  resources = [{
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}
```

#### Access Group Policy using service with region

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]

  resources = [{
    service = "cloud-object-storage"
  }]
}

```
#### Access Group Policy using resource instance 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### Access Group Policy using resource group 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Access Group Policy using resource and resource type 

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles        = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Access Group Policy using attributes

```hcl
resource "ibm_iam_access_group" "accgrp" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_access_group_policy" "policy" {
  access_group_id = "${ibm_iam_access_group.accgrp.id}"
  roles           = ["Viewer"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }

    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

### Input parameters

The following arguments are supported:

* `access_group_id` - (Required, string) ID of the access group.
* `roles` - (Required, list) comma separated list of roles. Valid roles are Writer, Reader, Manager, Administrator, Operator, Viewer, Editor.
* `resources` - (Optional, list) A nested block describing the resource of this policy.
Nested `resources` blocks have the following structure:
  * `service` - (Optional, string) Service name of the policy definition.  You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
  * `resource_instance_id` - (Optional, string) ID of resource instance of the policy definition.
  * `region` - (Optional, string) Region of the policy definition.
  * `resource_type` - (Optional, string) Resource type of the policy definition.
  * `resource` - (Optional, string) Resource of the policy definition.
  * `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. 
  * `attributes` - (Optional, map) Set resource attributes in the form of `'name=value,name=value...`.
 **NOTE**: Conflicts with `account_management`.
* `account_management` - (Optional, bool) Gives access to all account management services if set to `true`. Default value `false`. 
 **NOTE**: Conflicts with `resources`.
* `tags` - (Optional, array of strings) Tags associated with the access group Policy instance.
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the access group policy. The id is composed of \<access_group_id\>/\<access_group_policy_id\>

* `version` - Version of the access group policy.

### Import

ibm_iam_access_group_policy can be imported using access group ID and access group policy ID, eg

```
$ terraform import ibm_iam_access_group_policy.example AccessGroupId-1148204e-6ef2-4ce1-9fd2-05e82a390fcf/bf5d6807-371e-4755-a282-64ebf575b80a
```


## ibm_iam_service_id

Provides a resource for IAM ServiceID. This allows serviceID  to be created, updated and deleted.

### Sample Terraform code

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name        = "test"
  description = "New ServiceID"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the serviceID.
* `description` - (Optional, string) Description of the serviceID.
* `tags` - (Optional, array of strings) Tags associated with the IAM ServiceID.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the serviceID.
* `version` - Version of the serviceID.
* `crn` - crn of the serviceID.



## ibm_iam_service_id

Provides a resource for IAM Service Policy. This allows service policy  to be created, updated and deleted.

### Sample Terraform code

#### Service Policy for All Identity and Access enabled services 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]
}

```

#### Service Policy using service with region

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]

  resources = [{
    service = "cloud-object-storage"
  }]
}

```
#### Service Policy using resource instance 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### Service Policy using resource group 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Service Policy using resource and resource type 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### Service Policy using attributes 

```hcl
resource "ibm_iam_service_id" "serviceID" {
  name = "test"
}

data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_service_policy" "policy" {
  iam_service_id = "${ibm_iam_service_id.serviceID.id}"
  roles        = ["Administrator"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }
    
  }]
}

```

### Input parameters

The following arguments are supported:

* `iam_service_id` - (Required, string) UUID of the serviceID.
* `roles` - (Required, list) comma separated list of roles. Valid roles are Writer, Reader, Manager, Administrator, Operator, Viewer, Editor.
* `resources` - (Optional, list) A nested block describing the resource of this policy.
Nested `resources` blocks have the following structure:
  * `service` - (Optional, string) Service name of the policy definition.  You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
  * `resource_instance_id` - (Optional, string) ID of resource instance of the policy definition.
  * `region` - (Optional, string) Region of the policy definition.
  * `resource_type` - (Optional, string) Resource type of the policy definition.
  * `resource` - (Optional, string) Resource of the policy definition.
  * `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. 
  * `attributes` - (Optional, map) Set resource attributes in the form of `'name=value,name=value...`.
 **NOTE**: Conflicts with `account_management`.
* `account_management` - (Optional, bool) Gives access to all account management services if set to `true`. Default value `false`. 
 **NOTE**: Conflicts with `resources`.
* `tags` - (Optional, array of strings) Tags associated with the service policy instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the service policy. The id is composed of \<iam_service_id\>/\<service_policy_id\>

* `version` - Version of the service policy.

### Import

ibm_iam_service_policy can be imported using serviceID and service policy id, eg

```
$ terraform import ibm_iam_service_policy.example ServiceId-d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```




## ibm_iam_user_policy

Provides a resource for IAM User Policy. This allows user policy to be created, updated and deleted. To assign a policy to one user, the user must exist in the account to which you assign the policy. 

### Sample Terraform code

#### User Policy for All Identity and Access enabled services 

```hcl
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]
}

```

#### User Policy using service with region

```hcl
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service = "kms"
  }]
}

```
#### User Policy using resource instance 

```hcl
resource "ibm_resource_instance" "instance" {
  name     = "test"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Manager", "Viewer", "Administrator"]

  resources = [{
    service              = "kms"
    resource_instance_id = "${element(split(":",ibm_resource_instance.instance.id),7)}"
  }]
}

```

#### User Policy using resource group 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Viewer"]

  resources = [{
    service           = "containers-kubernetes"
    resource_group_id = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### User Policy using resource and resource type 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Administrator"]

  resources = [{
    resource_type = "resource-group"
    resource      = "${data.ibm_resource_group.group.id}"
  }]
}

```

#### User Policy using attributes 

```hcl
data "ibm_resource_group" "group" {
  name = "default"
}

resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@in.ibm.com"
  roles  = ["Administrator"]

  resources = [{
    service = "is"

    attributes = {
      "vpcId" = "*"
    }
    
  }]
}

```


### Input parameters

The following arguments are supported:

* `ibm_id` - (Required, string) The ibm id or email of user.
* `roles` - (Required, list) comma separated list of roles. Valid roles are Writer, Reader, Manager, Administrator, Operator, Viewer, Editor.
* `resources` - (Optional, list) A nested block describing the resource of this policy.
Nested `resources` blocks have the following structure:
  * `service` - (Optional, string) Service name of the policy definition.  You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
  * `resource_instance_id` - (Optional, string) ID of resource instance of the policy definition.
  * `region` - (Optional, string) Region of the policy definition.
  * `resource_type` - (Optional, string) Resource type of the policy definition.
  * `resource` - (Optional, string) Resource of the policy definition.
  * `resource_group_id` - (Optional, string) The ID of the resource group. You can retrieve the value from data source `ibm_resource_group`. 
  * `attributes` - (Optional, map) Set resource attributes in the form of `'name=value,name=value...`.
 **NOTE**: Conflicts with `account_management`.
* `account_management` - (Optional, bool) Gives access to all account management services if set to `true`. Default value `false`. 
 **NOTE**: Conflicts with `resources`.
* `tags` - (Optional, array of strings) Tags associated with the user policy instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the User Policy. The id is composed of \<ibm_id\>/\<user_policy_id\>

* `version` - Version of the User Policy.

### Import

ibm_iam_user_policy can be imported using IBMID and User Policy id, eg

```
$ terraform import ibm_iam_user_policy.example test@in.ibm.com/9ebf7018-3d0c-4965-9976-ef8e0c38a7e2
```