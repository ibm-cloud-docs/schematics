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

# Cloud Foundry Data Sources
{: #cloud-foundry-data-sources}


## ibm_account

Import the details of an existing IBM Cloud account as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_org" "orgData" {
  org = "example.com"
}

data "ibm_account" "accountData" {
  org_guid = "${data.ibm_org.orgData.id}"
}
```

### Input parameters

The following arguments are supported:

* `org_guid` - (Required, string) The GUID of the IBM Cloud organization. You can retrieve the value from the `ibm_org` data source or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the account.  
* `account_users` - The list of account user's in the account. Nested `account_users` blocks have the following structure:
  * `id` -  The account user Id.
  * `email` - The account user email.
  * `state` -  The account user state.
  * `role` -  The account user role.


## ibm_app

Import the details of an existing IBM Cloud app as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_app" "testacc_ds_app" {
  name       = "my-app"
  space_guid = "${ibm_app.app.space_guid}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the application. You can retrieve the value by running the `ibmcloud app list` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Required, string) The GUID of the IBM Cloud space where the application is deployed. You can retrieve the value with the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the application.
* `memory` - The memory, specified in megabytes, that is allocated to the application.
* `instances` - The number of instances of the application.
* `disk_quota` - The disk quota for an instance of the application, specified in megabytes.
* `buildpack` - The buildpack used by the application. It can be any of the following:
    * Blank, to indicate auto-detection.
    * A Git URL pointing to a buildpack.
    * The name of an installed buildpack.
* `environment_json` - Key/value pairs of all the environment variables. Does not include any system or service variables.
* `route_guid` - The route GUIDs that are bound to the application.
* `service_instance_guid` - The service instance GUIDs that are bound to the application.
* `package_state` - The state of the application package, such as staged or pending.
* `state` - The state of the application.
* `health_check_http_endpoint` - Endpoint called to determine if the app is healthy.
* `health_check_type` - Type of health check.
* `health_check_timeout` - Health check timeout in seconds.



## ibm_app_domain_private

Import the details of an existing IBM Cloud private domain as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_app_domain_private" "private_domain" {
  name = "foo.com"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the private domain.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the private domain.  



## ibm_app_domain_shared

Import the details of an existing IBM Cloud shared domain as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_app_domain_shared" "shared_domain" {
  name = "foo.com"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the shared domain.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the shared domain.  



## ibm_app_route

Import the details of an existing IBM Cloud route as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_app_route" "route" {
  domain_guid = "${data.ibm_app_domain_shared.domain.id}"
  space_guid  = "${data.ibm_space.spacedata.id}"
  host        = "somehost"
  path        = "/app"
}
```

### Input parameters

The following arguments are supported:

* `domain_guid` - (Required, string) The GUID of the associated domain. You can retrieve the value from the `ibm_app_domain_shared` data source.
* `space_guid` - (Required, string) The GUID of the space where you want to create the route. You can retrieve the value from the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `host` - (Optional, string) The host portion of the route. Required for shared domains.
* `port` - (Optional, integer) The port of the route. Supported for domains of TCP router groups only.
* `path` - (Optional, string) The path for a route as raw text. Paths must contain 2 - 128 characters. Paths must start with a forward slash (/). Paths must not contain a question mark (?).


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the route.  



## ibm_org

Import the details of an existing IBM Cloud org as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_org" "orgdata" {
  org = "example.com"
}
```

### Input parameters

The following arguments are supported:

* `org` - (Required, string) The name of the IBM Cloud organization. You can retrieve the value by running the `ibmcloud iam orgs` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the organization.  



## ibm_org_quota

Import the details of an existing quota for an IBM Cloud org as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_org_quota" "orgquotadata" {
  name = "quotaname"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the quota plan for the IBM Cloud org. You can retrieve the value by running the `ibmcloud cf quotas` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the organization.
* `app_instance_limit` - Defines the total number of app instances that are allowed for the organization.
* `app_tasks_limit` - Defines the total app task limit for the organization.
* `instance_memory_limit` - Defines the total memory usage that is allowed per instance for the organization.
* `memory_limit` - Defines the total memory usage that is allowed for the organization.
* `non_basic_services_allowed` - Defines if non-basic (paid) services are allowed for the organization.
* `total_private_domains` - Defines the total private domain limit for the organization.
* `total_reserved_route_ports` - Defines the number of routes with reserved ports for the organization. 
* `total_routes` - Defines the maximum total routes for the organization.
* `total_service_keys` - Defines the maximum number of service keys for the organization.
* `total_services` - Defines the maximum number of services for organization.
* `trial_db_allowed` - Defines if a trial db is allowed for the organization.



## ibm_service_instance

Import the details of an existing IBM service instance from IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_space" "space" {
  org   = "example.com"
  space = "dev"
}

data "ibm_service_instance" "serviceInstance" {
  name = "mycloudantdb"
  space_guid   = "${data.ibm_space.space.id}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the service instance. You can retrieve the value by running the `ibmcloud service list` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

* `space_guid` - (Required, string) The GUID of the space where the service instance exists. You can retrieve the value from data source `ibm_space`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the service instance.
* `credentials` - The credentials provided by the service broker to use this service.
* `service_keys` - The service keys associated with this service.
* `service_plan_guid` - The plan GUID for the service offering used by this service instance.



## ibm_service_key

Import the details of an existing IBM service key from IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_space" "space" {
  org   = "example.com"
  space = "dev"
}

data "ibm_service_key" "serviceKeydata" {
  name                  = "mycloudantdbKey"
  service_instance_name = "mycloudantdb"
  space_guid            = "${data.ibm_space.space.id}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the service key. You can retrieve the value by running the `ibmcloud service keys` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `service_instance_name` - (Required, string) The name of the service instance that the service key is associated with. You can retrieve the value by running the `ibmcloud service list` command in the IBM Cloud CLI.
* `space_guid` - (Required, string) The GUID of the space where the service instance exists. You can retrieve the value from the data source `ibm_space`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the service key.
* `credentials` - The credentials associated with the key.  



## ibm_service_plan

Import the details of an existing service plan from IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_service_plan" "service_plan" {
  service = "cloudantNoSQLDB"
  plan    = "Lite"
}
```

### Input parameters

The following arguments are supported:

* `service` - (Required, string) The name of the service offering. You can retrieve the name of the service by running the `ibmcloud service offerings` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `plan` - (Required, string) The name of the plan type supported by the service. You can retrieve the plan type by running the `ibmcloud service offerings` command in the IBM Cloud CLI.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the service plan.  



## ibm_space

Import the details of an existing IBM Cloud space as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_space" "spaceData" {
  space = "prod"
  org   = "someexample.com"
}
```

The following example shows how you can use the data source to reference the space ID in the `ibm_service_instance` resource.

```hcl
resource "ibm_service_instance" "service_instance" {
  name              = "test"
  space_guid        = "${data.ibm_space.spaceData.id}"
  service           = "speech_to_text"
  plan              = "lite"
  tags              = ["cluster-service", "cluster-bind"]
}

```

### Input parameters

The following arguments are supported:

* `org` - (Required) The name of your IBM Cloud organization. You can retrieve the value by running the `ibmcloud iam orgs` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space` - (Required) The name of your space. You can retrieve the value by running the `ibmcloud iam spaces` command in the IBM Cloud CLI.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the space.  
* `managers` - The email addresses (associated with IBMid) of the users who have a manager role in this space.
* `auditors` - The email addresses (associated with IBMid) of the users who have an auditor role in this space.
* `developers` - The email addresses (associated with IBMid) of the users who have a developer role in this space.
