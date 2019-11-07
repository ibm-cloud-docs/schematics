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

# Cloud Foundry Resources
{: #cloud-foundry-resources}


## ibm_app

Provides an application resource. This allows applications to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_space" "space" {
  org   = "example.com"
  space = "dev"
}

resource "ibm_app" "app" {
  name                 = "my-app"
  space_guid           = "${data.ibm_space.space.id}"
  app_path             = "hello.zip"
  wait_timeout_minutes = 90
  buildpack            = "sdk-for-nodejs"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the application. You can retrieve the value by running the `ibmcloud app list` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `memory` - (Optional, integer) The amount of memory, specified in megabytes, that each instance has. If you don't specify a value, the system assigns pre-defined values based on the quota allocated to the application. You can check the default values by running `ibmcloud cf org <org-name>`. The command lists the quotas that are defined in your organization and space. If space quotas are defined, you can get them by running `ibmcloud cf space-quota <space-quota-name>`, where <quota-name> is the name of the quota. Otherwise you can check the organization quotas by running `ibmcloud cf quota <quota-name>`.
* `instances` - (Optional, integer) The number of instances of the application.
* `disk_quota` - (Optional, integer) The maximum amount of disk, specified in megabytes, available to an instance of an application. The default value is [1024 MB](http://bosh.io/jobs/cloud_controller_ng?source=github.com/cloudfoundry/cf-release&version=234#p=cc.default_app_disk_in_mb). Check with your cloud provider if the value has been set differently.
* `space_guid` - (Required, string) The GUID of the space where the application is deployed. You can retrieve the value from data source `ibm_space` or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `buildpack` - (Optional, string) The buildpack to compile or prepare the application. You can provide its values in the following ways:
  * Leave the value blank for auto-detection.
  * Point to the Git URL for a buildpack. For example, https://github.com/cloudfoundry/nodejs-buildpack.git.
  * List the name of an installed buildpack. For example, `go_buildpack`.
* `environment_json` - (Optional, map) The key/value pairs of all the environment variables to run in your application. Do not provide any key/value pairs for system or service variables.
* `command` - (Optional, string) The initial command for the app.
* `route_guid` - (Optional, set) The route GUIDs that bind you want to the application. The route must be in the same space as the application.
* `service_instance_guid` - (Optional, set) The service instance GUIDs that you want to bind to the application.
* `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the application to restage or start. The default value is `20`. A value of `0` means that there is no wait period.
* `app_path` - (Required, string) The path to the compressed file of the application. The compressed file must contain all the application files directly within it instead of within a top-level folder. To create the compressed file, go to the directory where your application files are and run `zip -r myapplication.zip *`.
* `app_version`	 - (Optional, string) The version of the application. If you make changes to the content in the application compressed file specified by _app_path_, Terraform can't detect the changes. You can let Terraform know that your file content has changed by either changing the application compressed file name or by using this argument to indicate the version of the file.
* `health_check_http_endpoint` - (Optional, string) Endpoint called to determine if the app is healthy.
* `health_check_type` - (Optional, string) Type of health check to perform. Default `port`. Valid types are `port` and `process`.
* `health_check_timeout` - (Optional, integer) Timeout in seconds for health checking of an staged app when starting up.
* `tags` - (Optional, array of strings) Tags associated with the application instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the application.



## ibm_app_domain_private

Provides a private domain resource. This allows private domains to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_org" "orgdata" {
  org = "someexample.com"
}

resource "ibm_app_domain_private" "domain" {
  name     = "foo.com"
  org_guid = "${data.ibm_org.orgdata.id}"
  tags     = ["tag1", "tag2"]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the domain.
* `org_guid` - (Required, string) The GUID of the organization that owns the domain. You can retrieve the value from data source `ibm_org` or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `tags` - (Optional, array of strings) Tags associated with the application private domain instance.  
  **NOTE**: `Tags` are managed locally and are currently not stored on the IBM Cloud service endpoint.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the private domain.



## ibm_app_domain_shared

Provides a shared domain resource. This allows shared domains to be created, updated, and deleted.

### Sample Terraform code

```hcl
resource "ibm_app_domain_shared" "domain" {
  name              = "foo.com"
  router_group_guid = "3hG5jkjk4k34JH5666"
  tags              = ["tag1", "tag2"]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the domain.
* `router_group_guid` - (Optional, string) The GUID of the router group.
* `tags` - (Optional, array of strings) Tags associated with the application shared domain instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the shared domain.



## ibm_app_route

Provides a route resource. This allows routes to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_space" "spacedata" {
  space = "space"
  org   = "someexample.com"
}

data "ibm_app_domain_shared" "domain" {
  name = "mybluemix.net"
}

resource "ibm_app_route" "route" {
  domain_guid = "${data.ibm_app_domain_shared.domain.id}"
  space_guid  = "${data.ibm_space.spacedata.id}"
  host        = "somehost172"
  path        = "/app"
}
```

### Input parameters

The following arguments are supported:

* `domain_guid` - (Required, string) The GUID of the associated domain. You can retrieve the value from data source `ibm_app_domain_shared` or `ibm_app_domain_private`.
* `space_guid` - (Required, string) The GUID of the space where you want to create the route. You can retrieve the value from data source `ibm_space` or by running the `ibmcloud iam space <space_name> --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `host` - (Optional, string) The host portion of the route. Host is required for shared-domains.
* `port` - (Optional, integer) The port of the route. Port is supported for domains of TCP router groups only.
* `path` - (Optional, string) The path for a route as raw text. Paths must be 2 - 128 characters. Paths must start with a forward slash (/) and cannot contain a question mark (?).
* `tags` - (Optional, array of strings) Tags associated with the route instance.  
    **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the route.



## ibm_org

Provides an organization resource. This allows organizations to be created, updated, and deleted.

### Sample Terraform code

```hcl
resource "ibm_org" "testacc_org" {
    name = "myorg"
    org_quota_definition_guid = "myorgquotaguid"
    auditors = ["auditor@in.ibm.com"]
    managers = ["manager@in.ibm.com"]
    users = ["user@in.ibm.com"]
    billing_managers = ["billingmanager@in.ibm.com"]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The descriptive name used to identify a org. The org name must be unique in IBM Cloud and cannot be in use by another IBM Cloud user. 
* `org_quota_definition_guid` - (Optional, string) The GUID for the quota associated with the org. The quota sets memory, service, and instance limits for the org.
* `managers` - (Optional, set) The email addresses for users that you want to assign manager access to. The email address needs to be associated with an IBMid. Managers have the following permissions within the org:
  * Can create, view, edit, or delete spaces.
  * Can view usage and quota information.
  * Can invite users and manage user access.
  * Can assign roles to users.
  * Can manage custom domains.
* `users` - (Optional, set) The email addresses for the users that you want to grant org-level access to. The email address needs to be associated with an IBMid. 
* `auditors` - (Optional, set) The email addresses for the users that you want to assign auditor access to. The email address needs to be associated with an IBMid. Auditors have the following permissions within the org:
  * Can view users and their assigned roles.
  * Can view quota information.
* `billing_managers` - (Optional, set) The email addresses for the users that you want to assign billing manager access to. The email address needs to be associated with an IBMid. Billing managers have the following permissions within the org:
  * Can view runtime and service usage information on the usage dashboard.  

**NOTE**: By default the user creating this resource will have the manager role as per the Cloud Foundry API behavior. Terraform will throw error if you add yourself to the manager role or as a user. This information is not persisted in the state file to avoid any spurious diffs.
* `tags` - (Optional, array of strings) Tags associated with the org.  
  **NOTE**: Tags are managed locally and not stored on the IBM Cloud service endpoint.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the org.  


### Import

Org can be imported using the `id`, e.g.

```
$ terraform import ibm_org.myorg abde-12345
```
Once you bring your current org under terraform management, you can perform others operations like adding user or assigning them required roles.



## ibm_service_instance

Provides a service instance resource. This allows service instances to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_space" "spacedata" {
  space = "prod"
  org   = "somexample.com"
}

resource "ibm_service_instance" "service_instance" {
  name       = "test"
  space_guid = "${data.ibm_space.spacedata.id}"
  service    = "speech_to_text"
  plan       = "lite"
  tags       = ["cluster-service", "cluster-bind"]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify the service instance.
* `space_guid` - (Required, string) The GUID of the space where you want to create the service. You can retrieve the value from data source `ibm_space`.
* `service` - (Required, string) The name of the service offering. You can retrieve the value by running the `ibmcloud service offerings` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `plan` - (Required, string) The name of the plan type supported by service. You can retrieve the value by running the `ibmcloud service offerings` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `tags` - (Optional, array of strings) Tags associated with the public IP instance.
* `parameters` - (Optional, map) Arbitrary parameters to pass to the service broker. The value must be a JSON object.
* `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the service instance to become available before declaring it as created. It is also the same amount of time waited for deletion to finish. The default value is `10`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new service instance.
* `credentials` - The credentials provided by the service broker to use the service.
* `service_keys` - The service keys associated with the service.
* `service_plan_guid` - The plan of the service offering used by this service instance.



## ibm_service_key

Provides a service key resource. This allows service keys to be created, updated, and deleted.

### Sample Terraform code

```hcl
data "ibm_service_instance" "service_instance" {
  name = "mycloudant"
}

resource "ibm_service_key" "serviceKey" {
  name                  = "mycloudantkey"
  service_instance_guid = "${data.ibm_service_instance.service_instance.id}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify a service key.
* `parameters` - (Optional, map) Arbitrary parameters to pass along to the service broker. Must be a JSON object.
* `service_instance_guid` - (Required, string) The GUID of the service instance associated with the service key.
* `tags` - (Optional, array of strings) Tags associated with the service key instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new service key.
* `credentials` - The credentials associated with the key.



## ibm_space

Provides a space resource. This allows spaces to be created, updated, and deleted.

### Sample Terraform code

```hcl
resource "ibm_space" "space" {
  name        = "myspace"
  org         = "myorg"
  space_quota = "myspacequota"
  managers    = ["manager@example.com"]
  auditors    = ["auditor@example.com"]
  developers  = ["developer@example.com"]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The descriptive name used to identify a space.
* `org` - (Required, string) The name of the organization to which this space belongs.
* `space_quota` - (Optional, string) The name of the Space Quota Definition associated with the space.
* `managers` - (Optional, set) The email addresses (associated with IBMids) of the users to whom you want to give a manager role in this space. Users with the manager role can invite users, manage users, and enable features for the given space.
* `developers` - (Optional, set) The email addresses (associated with IBMids) of the users to whom you want to give a developer role in this space. Users with the developer role can create apps and services, manage apps and services, and see logs and reports in the given space.
* `auditors` - (Optional, set) The email addresses (associated with IBMids) of the users to whom you want to give an auditor role in this space. Users with the auditor role can view logs, reports, and settings in the given space.  

**NOTE**: By default the newly created space has no user associated with it. Add your own email address to the `managers` or `developers` field in order to be able to use the space correctly for the first time.

* `tags` - (Optional, array of strings) Tags associated with the space instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new space.
