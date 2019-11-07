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

# Infrastructure Resources
{: #infrastructure-resources}


## ibm_cdn

This resource is used to order a cdn domain mapping.

### Sample Terraform code

```hcl
resource "ibm_cdn" "test_cdn1" {
  hostname = "www.default.com"
  vendor_name = "akamai"
  origin_address = "111.111.111.5"
  origin_type = "HOST_SERVER"
}
```

### Input parameters

* `host_name` - (Required,  string) Hostname associated with the cdn domain mapping.
* `cname` - (Optional,  string) enter a unique cname for your cdn.
* `path` - (Optional,  string) enter the path for the cdn .
* `vendor_name` - (Required,  string) only "akamai" is supported for now.
* `origin_type` - (Required,  string) mention the type of storage. It can be "HOST_SERVER" or "OBJECT_STORAGE".
* `origin_address` - (Required,  string) Provide the IP address for domain mapping.
* `protocol` - (Optional, string) "HTTP is taken as default".
* `http_port` - (Optional, Int) 80 is taken as default. **NOTE**: It can only be populated if protocol is set to "HTTP" or "HTTP_AND_HTTPS"
* `https_port` - (Optional, Int) 0 is taken as default. **NOTE**: It can only be populated if protocol is set to "HTTPS" or "HTTP_AND_HTTPS"
* `bucket_name` - (Required, string) required for "OBJECT_STORAGE" origin_type only.
* `certificate_type`: required for HTTPS protocol. SHARED_SAN_CERT or WILDCARD_CERT.
* `header`: (Optional, string) Provide Header for CDN.
* `respect_headers`: A boolean value that, if set to true, will cause TTL settings in the Origin to override CDN TTL settings.
* `file_extension` - (optional for Object Storage) File extensions that are allowed to be cached.
* `performance_configuration` - (Optional, string) Provide the performance configuration. Default is "General web delivery"
* `cache_key_query_rule`: The following options are available to configure Cache Key behavior:
    include-all - includes all query arguments default
    ignore-all - ignores all query arguments
    ignore: space separated query-args - ignores those specific query arguments. For example, ignore: query1 query2
    include: space separated query-args: includes those specific query arguments. For example, include: query1 query2

### Output parameters

The following attributes are exported:

* `id` - The unique internal identifier of the cdn domian mapping.
* `status` - The Status of the cdn domian mapping.


## ibm_cis

Provides a Cloud Internet Services instance resource. This allows CIS instances to be created, updated, and deleted. The ibmcloud_api_key used by Terraform must have been granted sufficient rights to create IBM Cloud Resources and have access to the Resource Group the CIS instance will be associated with. 

If no resource_group_id is specified, the CIS instance is created under the default resource group. The API_KEY must have been assigned permissions for this group.  

### Sample Terraform code

```hcl
data "ibm_resource_group" "group" {
  name = "test"
}

resource "ibm_cis" "cis_instance" {
  name              = "test"
  plan              = "standard"
  resource_group_id = "${data.ibm_resource_group.group.id}"
  tags              = ["tag1", "tag2"]
  location          = "global"

  //User can increase timeouts 
  timeouts {
    create = "15m"
    update = "15m"
    delete = "15m"
  }
}
```



The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify the CIS instance.
* `plan` - (Required, string) The name of the plan type for Cloud Internet Services. You can retrieve the value by running the `ibmcloud catalog service internet-svcs` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `location` - (Required, string) Target location or environment to create the CIS instance.
* `resource_group_id` - (Optional, string) The ID of the resource group where you want to create the service. You can retrieve the value from data source `ibm_resource_group`. If not provided creates the service in default resource group.
* `tags` - (Optional, array of strings) Tags associated with the instance.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new CIS instance.
* `status` - Status of resource instance.


### Import

The `ibm_cis` resource can be imported using the `id`. The ID is formed from the `CRN` (Cloud Resource Name) from the **Overview** page of the Internet Services instance. These will be located under the **Domain** heading. 

* CRN is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`

```
$ terraform import ibm_cis.myorg <crn>

$ terraform import ibm_cis.myorg crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::
```

### Timeouts

ibm_cis provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for Creating Instance.
* `update` - (Default 10 minutes) Used for Updating Instance.
* `delete` - (Default 10 minutes) Used for Deleting Instance.





## ibm_cis_domain

Creates a DNS Domain resource that represents a DNS domain assigned to CIS. A domain is the basic resource for working with Cloud Internet Services and is typically the first resouce that is assigned to the CIS service instance. The domain will not become `active` until the DNS Registrar is updated with the CIS name servers in the exported variable `name_servers`. Refer to the resource `dns_domain_registration_nameservers`for updating the IBM Cloud DNS Registrars name servers. 

### Sample Terraform code

```hcl
resource "ibm_cis_domain" "example" {
    domain = "example.com"
    cis_id = "${ibm_cis.instance.id}"
}

resource "ibm_cis" "instance" {
  name              = "test-domain"
  plan              = "standard"
```

### Input parameters

The following arguments are supported: 

* `domain` - (Required) The DNS domain name which will be added to CIS and managed.
* `cis_id` - (Required) The ID of the CIS service instance



### Output parameters

The following attributes are exported:

* `id` - The domain ID.
* `paused` - Boolean of whether this domain is paused (traffic bypasses CIS). Default: false.
* `status` - Status of the domain. Valid values: `active`, `pending`, `initializing`, `moved`, `deleted`, `deactivated`. After creation, the status will remain pending until the DNS Registrar is updated with the CIS name servers, exported in the 'name_servers' variable. 
* `name_servers` - The IBM CIS assigned name servers, to be passed by interpolation to the resource dns_`domain_registration_nameservers`.
* `original_name_servers` - The name servers from when the Domain was initially registered with the DNS Registrar.  

### Import

The `ibm_cis_domain` resource can be imported using the `id`. The ID is formed from the `Domain ID` of the domain concatentated using a `:` character with the `CRN` (Cloud Resource Name). 

The Domain ID and CRN will be located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `bx cis` CLI commands.

* **Domain ID** is a 32 digit character string of the form: `9caf68812ae9b3f0377fdf986751a78f` 

* **CRN** is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`


```
$ terraform import ibm_cis_domain.myorg <domain-id>:<crn>

$ terraform import ibm_cis_domain.myorg  9caf68812ae9b3f0377fdf986751a78f:crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::



## ibm_cis_domain_settings

Provides a resource which customizes IBM Cloud Internet Services domain settings. 

### Sample Terraform code

```hcl
resource "ibm_cis_domain_settings" "test" {
  cis_id          = "${ibm_cis.instance.id}"
  domain_id       = "${ibm_cis_domain.example.id}"
  waf             = "on"
  ssl             = "full"
  min_tls_version = "1.2"
}
```

### Input parameters

The following arguments are supported:

* `cis_id` - (Required) The ID of the CIS service instance.
* `domain_id` - (Required) The ID of the domain.
* `waf`. Allowed values: "off", "on"
* `min_tls_version`. Allowed values: 1.1", "1.2", "1.3".
* `ssl`. Allowed values: "off", "flexible", "full", "strict", "origin_pull".
* `automatic_https_rewrites`. Allowed values: "off", "on"
* `opportunistic_encryption`. Allowed values: "off", "on"
* `cname_flattening`. Allowed values: "flatten_at_root", "flatten_all", "flatten_none".


Additional settings not implemented in this version of the provider. 


### Output parameters

The following attributes are exported: 

* `certificate_status`. Value of: "none", "initializing", "authorizing", "active"



## ibm_cis_dns_record

Provides a IBM DNS Record resource. This resource is associated with an IBM Cloud Internet Services instance and a CIS Domain resource. 

### Sample Terraform code

```hcl

## Add a DNS record to the domain
resource "ibm_cis_dns_record" "example" {
  cis_id = "${ibm_cis.instance.id}"  
  domain_id = "${ibm_cis_domain.example.id}"
  name   = "terraform"
  content  = "192.168.0.11"
  type   = "A"
}
```

### Input parameters

The following arguments are supported:

* `cis_id` - (Required) The ID of the CIS service instance
* `domain_id` - (Required) The ID of the domain to add the DNS record to.
* `name` - (Required) The name of the record, e.g. "www".
* `type` - (Required) The type of the record. A,AAAA,CNAME,NS,MX,TXT,LOC,SRV,SPF,CAA. 
* `content` - (Optional) The (string) value of the record, e.g. "192.168.127.127". Either this or `data` must be specified
* `data` - (Optional) Map of attributes that constitute the record value. Only for LOC, CCA and SRV record types. Either this or `content` must be specified
* `priority` - (Optional) The priority of the record
* `proxied` - (Optional) Whether the record gets CIS's origin protection; defaults to `false`.

### Output parameters

The following attributes are exported:

* `id` - The record ID
* `name` - The FQDN of the record
* `proxiable` - Shows whether this record can be proxied, must be true if setting `proxied=true`
* `created_on` - The RFC3339 timestamp of when the record was created
* `modified_on` - The RFC3339 timestamp of when the record was last modified
* `data` - Map of attributes that constitute the record value.

### Import

The `ibm_cis_dns_record` resource can be imported using the `id`. The ID is formed from the `Dns Record ID`, the `Domain ID` of the domain and the `CRN` (Cloud Resource Name) concatentated using a `:` character.  

The Domain ID and CRN will be located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `bx cis` CLI commands.

* **Domain ID** is a 32 digit character string of the form: `9caf68812ae9b3f0377fdf986751a78f`

* **CRN** is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`

* **Dns Record ID** is a 32 digit character string of the form: `489d96f0da6ed76251b475971b097205c`. The id of an existing DNS record is not avaiable via the UI. It can be retrieved programatically via the CIS API or via the CLI using the CIS command to list the defined DNS recordss:  `bx cis dns-records <domain_id>` 


```
$ terraform import ibm_cis_dns_record.myorg <dns_record_id>:<domain-id>:<crn>

$ terraform import ibm_cis_dns_record.myorg  48996f0da6ed76251b475971b097205c:9caf68812ae9b3f0377fdf986751a78f:crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::


## ibm_cis_global_load_balancer

Provides a IBM CIS Global Load Balancer resource. This sits in front of a number of defined pools of origins, directs traffic to available origins and provides various options for geographically-aware load balancing. This resource is associated with an IBM Cloud Internet Services instance, a CIS Domain resource and CIS Origin pool resources.  

### Sample Terraform code

```hcl

## Define a global load balancer which directs traffic to defined origin pools

## In normal usage different pools would be set for data centers/availability zones and/or for different regions

## Within each availability zone or region we can define multiple pools in failover order

resource "ibm_cis_global_load_balancer" "example" {
  cis_id = "${ibm_cis.instance.id}"
  domain_id = "${ibm_cis_domain.example.id}"
  name = "www.example.com"
  fallback_pool_id = "${ibm_cis_origin_pool.example.id}"
  default_pool_ids = ["${ibm_cis_origin_pool.example.id}"]
  description = "example load balancer using geo-balancing"
  proxied = true
}

resource "ibm_cis_origin_pool" "example" {
  cis_id = "${ibm_cis.instance.id}"
  name = "example-lb-pool"
  origins {
    name = "example-1"
    address = "192.0.2.1"
    enabled = false
  }
}
```

### Input parameters

The following arguments are supported:

* `cis_id` - (Required) The ID of the CIS service instance
* `domain_id` - (Required) The ID of the domain to add the load balancer to.
* `name` - (Required) The DNS name to associate with the load balancer. This can be a hostname, e.g. "www" or the fully qualified name "www.example.com". "example.com" is also accepted. 
* `fallback_pool_id` - (Required) The pool ID to use when all other pools are detected as unhealthy.
* `default_pool_ids` - (Required) A list of pool IDs ordered by their failover priority. Used whenever region/pop pools are not defined.
* `description` - (Optional) Free text description.
* `proxied` - (Optional) Whether the hostname gets ibm's origin protection. Defaults to `false`.
* `session_affinity` - (Optional) Associates all requests coming from an end-user with a single origin. ibm will set a cookie on the initial response to the client, such that consequent requests with the cookie in the request will go to the same origin, so long as it is available.

Region and pop pools are not currently implemented in this version of the provider. 

### Output parameters

The following attributes are exported:

* `id` - Unique identifier for the global load balancer.
`name` - The fully qualified name of the load balancer, e.g. "www.example.com". 
* `created_on` - The RFC3339 timestamp of when the load balancer was created.
* `modified_on` - The RFC3339 timestamp of when the load balancer was last modified.

### Import

The `ibm_cis_global_load_balancer` resource can be imported using the `id`. The ID is formed from the `Global Load Balancer ID`, the `Domain ID` of the domain and the `CRN` (Cloud Resource Name) concatentated usinga `:` character.  

The Domain ID and CRN will be located on the **Overview** page of the Internet Services instance under the **Domain** heading of the UI, or via using the `bx cis` CLI commands.

* **Domain ID** is a 32 digit character string of the form: `9caf68812ae9b3f0377fdf986751a78f`

* **CRN** is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`

* **Glb ID** is a 32 digit character string of the form: `57d96f0da6ed76251b475971b097205c`. The id of an existing GLB is not avaiable via the UI. It can be retrieved programatically via the CIS API or via the CLI using the CIS command to list the defined GLBs:  `bx cis glbs <domain_id>` 


```
$ terraform import ibm_cis_global_load_balancer.myorg <glb_id>:<domain-id>:<crn>

$ terraform import ibm_cis_domain.myorg  57d96f0da6ed76251b475971b097205c:9caf68812ae9b3f0377fdf986751a78f:crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::



## ibm_cis_healthcheck

If you're using IBM's Cloud Internet Services Global Load Balancing to load-balance across multiple origin servers or data centers, you can configure a Healthcheck monitor to actively check the availability of those servers over HTTP(S). This resource is associated with an IBM Cloud Internet Services instance. 

### Sample Terraform code

```hcl
resource "ibm_cis_healthcheck" "test" {
  cis_id = "${ibm_cis.instance.id}"
  expected_body = "alive"
  expected_codes = "2xx"
  method = "GET"
  timeout = 7
  path = "/health"
  interval = 60
  retries = 5
  description = "example load balancer"
}
```

### Input parameters

The following arguments are supported:

* `cis_id` - (Required) The ID of the CIS service instance
* `expected_body` - (Required) A case-insensitive sub-string to look for in the response body. If this string is not found, the origin will be marked as unhealthy. A null value of "" is allowed to match on any content. 
* `expected_codes` - (Required) The expected HTTP response code or code range of the health check. Eg `2xx`
* `method` - (Optional) The HTTP method to use for the health check. Default: "GET".
* `timeout` - (Optional) The timeout (in seconds) before marking the health check as failed. Default: 5.
* `path` - (Optional) The endpoint path to health check against. Default: "/".
* `interval` - (Optional) The interval between each health check. Shorter intervals may improve failover time, but will increase load on the origins as we check from multiple locations. Default: 60.
* `retries` - (Optional) The number of retries to attempt in case of a timeout before marking the origin as unhealthy. Retries are attempted immediately. Default: 2.
* `type` - (Optional) The protocol to use for the healthcheck. Currently supported protocols are 'HTTP' and 'HTTPS'. Default: "http".
* `description` - (Optional) Free text description.

Header is not currently supported in this version of the provider. 

### Output parameters

The following attributes are exported:

* `id` - Load balancer monitor ID.
* `created_on` - The RFC3339 timestamp of when the load balancer monitor was created.
* `modified_on` - The RFC3339 timestamp of when the load balancer monitor was last modified.

### Import

The `ibm_cis_health_check` resource can be imported using the `id`. The ID is formed from the `Healthcheck Id` and the `CRN` (Cloud Resource Name) concatentated usinga `:` character.  

The CRN will be located on the **Overview** page of the Internet Services instance under the **Domain** heading. 

* **CRN** is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`

* **Healthcheck ID** is a 32 digit character string of the form: `1fc7c3247067ee00856729661c7d58c9`. The id of an existing Healthcheck monitor is not avaiable via the UI. It can be retrieved programmatically via the CIS API or via the CLI using the CIS command to list the defined GLBs:  `bx cis glb-monitors` 


```
$ terraform import ibm_cis_healthcheck.myorg <healthcheck_id>:<crn>

$ terraform import ibm_cis_healthcheck.myorg 1fc7c3247067ee00856729661c7d58c9:crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::



## ibm_cis_origin_pool

Provides a IBM Cloud Internet Services origin pool resource. This provides a pool of origins that can be used by a IBM CIS Global Load Balancer. This resource is associated with an IBM Cloud Internet Services instance and optionally a CIS Healthcheck monitor resource. 


### Sample Terraform code

```hcl
resource "ibm_cis_origin_pool" "example" {
  cis_id = "${ibm_cis.instance.id}"
  name = "example-pool"
  origins {
    name = "example-1"
    address = "192.0.2.1"
    enabled = false
  }
  origins {
    name = "example-2"
    address = "192.0.2.2"
    enabled = false
  }
  description = "example load balancer pool"
  enabled = false
  minimum_origins = 1
  notification_email = "someone@example.com"
  check_regions      = ["WEU"]
}
```

### Input parameters

The following arguments are supported:

* `cis_id` - (Required) The ID of the CIS service instance
* `name` - (Required) A short name (tag) for the pool. Only alphanumeric characters, hyphens, and underscores are allowed.
* `origins` - (Required) The list of origins within this pool. Traffic directed at this pool is balanced across all currently healthy origins, provided the pool itself is healthy. It's a complex value. See description below.
* `check_regions` - (required) A list of regions (specified by region code) from which to run health checks. Empty means every region (the default), but requires an Enterprise plan. Region codes can be found on our partner Cloudflare's website [here](https://support.cloudflare.com/hc/en-us/articles/115000540888-Load-Balancing-Geographic-Regions).
* `description` - (Optional) Free text description.
* `enabled` - (required) Whether to enable (the default) this pool. Disabled pools will not receive traffic and are excluded from health checks. Disabling a pool will cause any load balancers using it to failover to the next pool (if any).
* `minimum_origins` - (Optional) The minimum number of origins that must be healthy for this pool to serve traffic. If the number of healthy origins falls below this number, the pool will be marked unhealthy and we will failover to the next available pool. Default: 1.
* `monitor` - (Optional) The ID of the Monitor to use for health checking origins within this pool.
* `notification_email` - (Optional) The email address to send health status notifications to. This can be an individual mailbox or a mailing list.

The **origins** block supports:

* `name` - (Required) A human-identifiable name for the origin.
* `address` - (Required) The IP address (IPv4 or IPv6) of the origin, or the publicly addressable hostname. Hostnames entered here should resolve directly to the origin, and not be a hostname proxied by CIS.
* `enabled` - (Optional) Whether to enable (the default) this origin within the Pool. Disabled origins will not receive traffic and are excluded from health checks. The origin will only be disabled for the current pool.

### Output parameters

The following attributes are exported:

* `id` - ID for this load balancer pool.
* `created_on` - The RFC3339 timestamp of when the load balancer was created.
* `modified_on` - The RFC3339 timestamp of when the load balancer was last modified.

### Import

The `ibm_cis_origin_pool` resource can be imported using the `id`. The ID is formed from the `Origin Pool Id` and the `CRN` (Cloud Resource Name) concatentated usinga `:` character.  

The CRN will be located on the **Overview** page of the Internet Services instance under the **Domain** heading. 

* **CRN** is a 120 digit character string of the form: `crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::`

* **Origin Pool ID** is a 32 digit character string of the form: `000f57b5c42bcff3c02d155c2d58aa97`. The id of an existing pool is not available via the UI. It can be retrieved programmatically via the CIS API or via the CLI using the CIS command to list the defined GLBs:  `bx cis glb-pools` 


```
$ terraform import ibm_cis_origin_pool.myorg <origin_pool_id>:<crn>

$ terraform import origin_pool.myorg 000f57b5c42bcff3c02d155c2d58aa97:crn:v1:bluemix:public:internet-svcs:global:a/4ea1882a2d3401ed1e459979941966ea:31fa970d-51d0-4b05-893e-251cba75a7b3::



## ibm_compute_autoscale_group

Provides a resource for auto scaling groups. This allows auto scaling groups to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Scale_Group).

### Sample Terraform code

In the following example, you can create an auto scaling group using a Debian image:

```hcl
resource "ibm_compute_autoscale_group" "test_scale_group" {
    name = "test_scale_group_name"
    regional_group = "as-sgp-central-1"
    minimum_member_count = 1
    maximum_member_count = 10
    cooldown = 30
    termination_policy = "CLOSEST_TO_NEXT_CHARGE"
    virtual_server_id = 267513
    port = 8080
    health_check = {
      type = "HTTP"
    }
    virtual_guest_member_template = {
      hostname = "test_virtual_guest_name"
      domain = "example.com"
      cores = 1
      memory = 1024
      network_speed = 100
      hourly_billing = true
      os_reference_code = "DEBIAN_8_64"

## Optional fields for virtual guest template (SoftLayer defaults apply):
      local_disk = false
      disks = [25]
      datacenter = "sng01"
      post_install_script_uri = ""
      ssh_key_ids = [383111]
      user_metadata = "#!/bin/bash ..."
    }

## Optional fields for scale_group:
    network_vlan_ids = [1234567, 7654321]
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the auto scaling group.
* `regional_group` - (Required, string) The regional group for the auto scaling group.
* `minimum_member_count` - (Required, integer) The fewest number of virtual guest members that are allowed in the auto scaling group.
* `maximum_member_count` - (Required, integer) The greatest number of virtual guest members that are allowed in the auto scaling group.
* `cooldown` - (Required, integer) The duration, expressed in seconds, that the auto scaling group waits before performing another scaling action.
* `termination_policy` - (Required, string) The termination policy for the auto scaling group.
* `virtual_guest_member_template` - (Required, array) The template with which to create guest members. Only one template can be configured. You can find accepted values in the [ibm_compute_vm_instance](compute_vm_instance.html) resource.
* `network_vlan_ids` - (Optional, array) The collection of VLAN IDs for the auto scaling group. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID in the resulting URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
* `virtual_server_id` - (Optional, integer) The ID of a virtual server in a local load balancer. You can find the ID with the following URL: `https://api.softlayer.com/rest/v3/SoftLayer_Network_Application_Delivery_Controller_LoadBalancer_VirtualIpAddress/<load_balancer_ID>/getObject?objectMask=virtualServers`. Replace _<load_balancer_ID>_ with the ID of the target load balancer. An IBM Cloud Classic Infrastructure (SoftLayer) user name and API key are required.
* `port` - (Optional, integer) The port number in a local load balancer. For example, `8080`.
* `health_check` - (Optional, map) The type of health check in a local load balancer. For example, `HTTP`. You can also use this value to specify custom HTTP methods.
* `tags` - (Optional, array of strings) Tags associated with the auto scaling group instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the auto scaling group.



## ibm_compute_autoscale_policy

Provides an auto scaling policy resource. This allows policies for auto scale groups to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Scale_Policy).

### Sample Terraform code

In the following example, you can create an auto scaling policy:

```hcl
resource "ibm_compute_autoscale_policy" "test_scale_policy" {
    name = "test_scale_policy_name"
    scale_type = "RELATIVE"
    scale_amount = 1
    cooldown = 30
    scale_group_id = "${ibm_compute_autoscale_group.sample-http-cluster.id}"
    triggers = {
        type = "RESOURCE_USE"
        watches = {
                    metric = "host.cpu.percent"
                    operator = ">"
                    value = "80"
                    period = 120
        }
    }
    triggers = {
        type = "ONE_TIME"
        date = "2016-07-30T23:55:00-00:00"
    }
    triggers = {
        type = "REPEATING"
        schedule = "0 1 ? * MON,WED *"
    }
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the auto scaling policy.
* `scale_type` - (Required, string) The scale type for the auto scaling policy. Accepted values are `ABSOLUTE`, `RELATIVE`, and `PERCENT`.
* `scale_amount` - (Required, integer) A count of the scaling actions to perform upon any trigger hit.
* `cooldown` - (Optional, integer) The duration, expressed in seconds, that the policy waits after the last action date before performing another scaling action. If you do not provide a value, the `scale_group` cooldown applies.
* `scale_group_id` - (Required, integer) The ID of the auto scale group associated with the policy.
* `triggers` - (Optional, array of integers and strings) The triggers to check for this group.
* `tags` - (Optional, array of strings) Tags associated with the auto scaling policy instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the auto scaling policy.



## ibm_compute_bare_metal

Provides a bare metal resource. This allows bare metal servers to be created, updated, and deleted. This resource supports both monthly bare metal servers and hourly bare metal servers. For more detail on bare metal severs, refer to the [IBM Cloud bare metal server page](https://www.ibm.com/cloud-computing/bluemix/bare-metal-servers).


### Sample Terraform code

#### Hourly bare metal server
When the `ibm_compute_bare_metal` resource definition has a `fixed_config_preset` attribute, Terraform creates an hourly bare metal server. Hardware specifications are predefined in the `fixed_config_preset` attribute and cannot be modified. The following example shows you how to create a new hourly bare metal server.

##### Example of an hourly bare metal server
```hcl
resource "ibm_compute_bare_metal" "hourly-bm1" {
    hostname = "hourly-bm1"
    domain = "example.com"
    os_reference_code = "UBUNTU_16_64"
    datacenter = "dal01"
    network_speed = 100 # Optional
    hourly_billing = true # Optional
    private_network_only = false # Optional
    fixed_config_preset = "S1270_8GB_2X1TBSATA_NORAID"

    user_metadata = "{\"value\":\"newvalue\"}" # Optional
    tags = [
      "collectd",
      "mesos-master"
    ]
    notes = "note test"
}
```

#### Monthly bare metal server
When the `fixed_config_preset` attribute is not configured, Terraform creates a monthly bare metal server resource. The monthly bare metal server resource provides options to configure process, memory, network, disk, and RAID. You can also can assign VLANs and subnets for the target monthly bare metal server. To configure the monthly bare metal server, you must provide additional attributes such as `package_key_name`, `proecss_key_name`, `disk_key_names`, and `os_key_name`. The following example shows you how to create a new monthly bare metal server.

##### Example of a monthly bare metal server
```hcl
resource "ibm_compute_bare_metal" "monthly_bm1" {


## Mandatory fields
    package_key_name = "DUAL_E52600_V4_12_DRIVES"
    process_key_name = "INTEL_INTEL_XEON_E52620_V4_2_10"
    memory = 64
    os_key_name = "OS_WINDOWS_2012_R2_FULL_DC_64_BIT_2"
    hostname = "cust-bm"
    domain = "ms.com"
    datacenter = "wdc04"
    network_speed = 100
    public_bandwidth = 500
    disk_key_names = [ "HARD_DRIVE_800GB_SSD", "HARD_DRIVE_800GB_SSD", "HARD_DRIVE_800GB_SSD" ]
    hourly_billing = false


## Optional fields
    private_network_only = false
    unbonded_network = true
    user_metadata = "{\"value\":\"newvalue\"}"
    public_vlan_id = 12345678
    private_vlan_id = 87654321
    public_subnet = "50.97.46.160/28"
    private_subnet = "10.56.109.128/26"
    tags = [
      "collectd",
      "mesos-master"
    ]
    redundant_power_supply = true
    storage_groups = {
       # RAID 5
       array_type_id = 3
       # Use three disks
       hard_drives = [ 0, 1, 2]
       array_size = 1600
       # Basic partition template for Windows
       partition_template_id = 17
    }
}
```

**Note**: Monthly bare metal servers do not support `immediate cancellation`. When Terraform deletes the monthly bare metal server, the `anniversary date cancellation` option is used.

#### Create a bare metal server using quote ID
If you already have a quote ID for the bare metal server, you can create a new bare metal server with the quote ID. The following example shows you how to create a new bare metal server with a quote ID.

##### Example of a quote based ordering
```hcl
resource "ibm_compute_bare_metal" "quote_test" {


## Mandatory fields
    hostname = "quote-bm-test"
    domain = "example.com"
    quote_id = 2209349


## Optional fields
    user_metadata = "{\"value\":\"newvalue\"}"
    public_vlan_id = 12345678
    private_vlan_id = 87654321
    public_subnet = "50.97.46.160/28"
    private_subnet = "10.56.109.128/26"
    tags = [
      "collectd",
      "mesos-master"
    ]  
}
```

### Input parameters

The following arguments are supported:

#### Arguments for all bare metal server types

* `hostname` - (Optional, string) The hostname for the computing instance.
* `domain` - (Required, string) The domain for the computing instance.
* `user_metadata` - (Optional, string) Arbitrary data to be made available to the computing instance.
* `notes` - (Optional, string) Notes to associate with the instance.
* `ssh_key_ids` - (Optional, array of numbers) The SSH key IDs to install on the computing instance when the instance is provisioned.  
    **NOTE:** If you don't know the ID(s) for your SSH keys, you can [reference your SSH keys by their labels](../d/compute_ssh_key.html).
* `post_install_script_uri` - (Optional, string) The URI of the script to be downloaded and executed after installation is complete.
* `tags` - (Optional, array of strings) Tags associated with this bare metal server. Permitted characters include: A-Z, 0-9, whitespace, _ (underscore), - (hyphen), . (period), and : (colon). All other characters will be removed.
* `file_storage_ids` - (Optional, array of numbers) File storage to which this computing instance should have access. File storage must be in the same data center as the bare metal server. If you use this argument to authorize access to file storage, do not use the `allowed_hardware_ids` argument in the `ibm_storage_file` resource in order to prevent the same storage being added twice.
* `block_storage_ids` - (Optional, array of numbers) Block storage to which this computing instance should have access. Block storage must be in the same data center as the bare metal server. If you use this argument to authorize access to block storage, do not use the `allowed_hardware_ids` argument in the `ibm_storage_file` resource in order to prevent the same storage being added twice.

#### Arguments common to hourly and monthly server

* `datacenter` - (Required, string) The datacenter in which you want to provision the instance.
* `gpu_key_name` - (Optional, string) The key name for the primary Graphics Processing Unit (GPU). For example - `GPU_NVIDIA_GRID_K2`.
Locate your package ID. See `package_key_name` attribute. Once you have the ID fetch its [details](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/<PACKAGE_ID>/getItems?objectMask=mask[prices[id,categories[id,name,categoryCode],capacityRestrictionType,capacityRestrictionMinimum,capacityRestrictionMaximum,locationGroupId]]). Select a gpu key name from the resulting available gpu key names where category code is `gpu0`.
* `gpu_secondary_key_name` - (Optional, string) The key name for the secondary Graphics Processing Unit (GPU). For example - `GPU_NVIDIA_GRID_K2`. Key names can be fetched in the similar way as `gpu_key_name` and  category code is `gpu1`.
* `hourly_billing` - (Required, boolean) The billing type for the instance. When set to `true`, the computing instance is billed on hourly usage. Otherwise the instance is billed on a monthly basis. The default value is `true`.
* `redundant_power_supply` - (Optional, boolean) When the value is `true`, an additional power supply is provided.
* `redundant_network` - (Optional, boolean) When the value is `true`, two physical network interfaces are provided with a bonding configuration. The default value is `false`.
* `unbonded_network` - (Optional, boolean) When the value is `true`, two physical network interfaces are provided without a bonding configuration. The default value is `false`.
* `network_speed` - (Optional, integer) The connection speed, expressed in Mbps,  for the instance's network components. The default value is `100`.
* `private_network_only` - (Optional, boolean) Specifies whether the instance only has access to the private network. When the value is `true`, a compute instance only has access to the private network. The default value is `false`.
* `extended_hardware_testing` - (Optional, boolean) Enable the extended hardware testing while ordering the bare metal server. The default value is `false`.  
    **NOTE**: Enabling the `extended_hardware_testing` will cause considerable delays in the deployment.
* `ipv6_enabled` - (Optional, boolean) The primary public IPv6 address. The default value is `false`.
* `ipv6_static_enabled` - (Optional, boolean) The public static IPv6 address block of `/64`. The default value is `false`.
* `secondary_ip_count` - (Optional, integer) Specifies secondary public IPv4 addresses. Accepted values are `4` and `8`.
* `image_template_id` - (Optional, integer) The image template ID you want to use to provision the computing instance. This is not the global identifier (UUID), but the image template group ID that should point to a valid global identifier. To retrieve the image template ID from the IBM Cloud infrastructure customer portal, navigate to **Devices > Manage > Images**, click the desired image, and note the ID number in the resulting URL.  
    **NOTE**: Conflicts with `os_reference_code`. If you don't know the ID(s) of your image templates, you can [reference them by name](../d/compute_image_template.html).

#### Arguments for hourly bare metal servers

* `fixed_config_preset` - (Required, string) The configuration preset with which you want to provision the bare metal server. This preset governs the type of CPU, number of cores, amount of RAM, and number of hard drives that the bare metal server has. To see the available presets, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3/SoftLayer_Hardware/getCreateObjectOptions.json) using your API key as the password. Find the key called `fixedConfigurationPresets`. The presets are identified by the key names.
* `os_reference_code` - (Optional, string) An operating system reference code that provisions the computing instance. To see available OS reference codes, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getVhdImportSoftwareDescriptions.json?objectMask=referenceCode), using your API key as the password.    
    **NOTE**: Conflicts with `image_template_id`.  
* `software_guard_extensions` - (Optional, boolean) The Software Guard Extensions product will be added to a compatible server package, selecting Intel SGX-enabled BIOS and hardware. The default value is `false`.

#### Arguments for monthly bare metal servers

* `public_vlan_id` - (Optional, integer) The public VLAN to be used for the public network interface of the instance. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID in the resulting URL.
* `private_vlan_id` - (Optional, integer) The private VLAN to be used for the private network interface of the instance. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID in the resulting URL.
* `public_subnet` - (Optional, string) The public subnet to be used for the public network interface of the instance. Accepted values are primary public networks. You can find accepted values in the [subnets docs](https://cloud.ibm.com/classic/network/subnets).
* `private_subnet` - (Optional, string) The private subnet to be used for the private network interface of the instance. Accepted values are primary private networks. You can find accepted values in the [subnets docs](https://cloud.ibm.com/classic/network/subnets).
* `package_key_name` - (Optional, string) The key name for the monthly bare metal server's package. Only use this argument when you create a new monthly bare metal server. You can find available package key names in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/getAllObjects?objectFilter={"type":{"keyName":{"operation":"BARE_METAL_CPU"}}}), using your API key as the password.
* `process_key_name` - (Optional, string). The key name for the monthly bare metal server's process. Only use this argument when you create a new monthly bare metal server. To get a process key name, first find the package key name in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/getAllObjects?objectFilter={"type":{"keyName":{"operation":"BARE_METAL_CPU"}}}). Then replace <PACKAGE_NAME> with your package key name in the following URL: `https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/<PACKAGE_NAME>/getItems?objectMask=mask[prices[id,categories[id,name,categoryCode],capacityRestrictionType,capacityRestrictionMinimum,capacityRestrictionMaximum,locationGroupId]]`. Select a process key name from the resulting available process key names.
* `disk_key_names` - (Optional, array of strings) The internal key names for the monthly bare metal server's disk. Only use this argument when you create a new monthly bare metal server. To get disk key names, first find the package key name in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/getAllObjects?objectFilter={"type":{"keyName":{"operation":"BARE_METAL_CPU"}}}). Then replace <PACKAGE_NAME> with your package key name in the following URL: `https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/PACKAGE_NAME/getItems?objectMask=mask[prices[id,categories[id,name,categoryCode],capacityRestrictionType,capacityRestrictionMinimum,capacityRestrictionMaximum,locationGroupId]]`. Select disk key names from the resulting available disk key names.
* `os_key_name` - (Optional, string) The operating system key name that you want to use to provision the computing instance. To get disk key names, first find the package key name in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/getAllObjects?objectFilter={"type":{"keyName":{"operation":"BARE_METAL_CPU"}}}). Then replace <PACKAGE_NAME> with your package key name in the following URL: `https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/<PACKAGE_NAME>/getItems?objectMask=mask[prices[id,categories[id,name,categoryCode],capacityRestrictionType,capacityRestrictionMinimum,capacityRestrictionMaximum,locationGroupId]]`. Select an OS key name from the resulting available OS key names.
* `public_bandwidth` - (Optional, integer) The amount of public network traffic, specified in gigabytes, allowed per month. The value can be greater than 0 when `private_network_only` is set to `false` and the server is a monthly-based server.
* `memory` - (Optional, integer) The amount of memory, specified in gigabytes, for the server.
* `storage_groups` - (Optional, array of storage group objects) Configurations for RAID and partition. For more information on configuring `storage_groups`, refer to [Ordering RAID Through API](https://sldn.softlayer.com/blog/hansKristian/Ordering-RAID-through-API). Each storage group object has the following sub-arguments:
    * `array_type_id` -(Required, integer) The RAID type. You can retrieve the value from the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Configuration_Storage_Group_Array_Type/getAllObjects).
    * `hard_drives` - (Required, array of integers) The index of hard drives for RAID configuration. The index starts at 0. For example, the array [0,1] is an index of two hard drives.
    * `array_size` - (Optional, integer) The target RAID disk size, specific in gigabytes.
    * `partition_template_id` - (Optional, string) The partition template ID for the OS disk. Templates are different based on the target OS. To get the partition template ID, first find the OS ID in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Hardware_Component_Partition_OperatingSystem/getAllObjects). Then replace <OS_ID> with your OS ID in the following URL: `https://api.softlayer.com/rest/v3/SoftLayer_Hardware_Component_Partition_OperatingSystem/<OS_ID>/getPartitionTemplates`. Select you template ID in resulting available parition template IDs.  
* `software_guard_extensions` - (Optional, boolean) The Software Guard Extensions product will be added to a compatible server package, selecting Intel SGX-enabled BIOS and hardware. The default value is `false`.
* `restricted_network` - (Optional, boolean) The non-datacenter restricted port speed. The default value is `false`.
* `tcp_monitoring` - (Optional) When the value is `false`, a ping monitoring service is provided. When the value is `true`, a ping monitoring service and a TCP monitoring service are provided.

#### Arguments for quote-based bare metal servers

* `public_vlan_id` - (Optional, integer) The public VLAN to be used for the public network interface of the instance. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID in the resulting URL.
* `private_vlan_id` - (Optional, integer) The private VLAN to be used for the private network interface of the instance. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID in the resulting URL.
* `public_subnet` - (Optional, string) The public subnet to be used for the public network interface of the instance. Accepted values are primary public networks. You can find accepted values in the [subnets docs](https://cloud.ibm.com/classic/network/subnets).
* `private_subnet` - (Optional, string) The private subnet to be used for the private network interface of the instance. Accepted values are primary private networks. You can find accepted values in the [subnets docs](https://cloud.ibm.com/classic/network/subnets).
* `quote_id` - (Optional, string) When you define `quote_id`, Terraform uses specifications in the quote to create a bare metal server. You can find the quote ID in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic) by navigating to **Account > Sales > Quotes**.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the bare metal server.
* `public_ipv4_address` - The public IPv4 address of the bare metal server.
* `public_ipv4_address_id` - The unique identifier for the public IPv4 address of the bare metal server.
* `private_ipv4_address` - The private IPv4 address of the bare metal server.
* `private_ipv4_address_id` - The unique identifier for the private IPv4 address of the bare metal server.
* `ipv6_address` - The public IPv6 address of the bare metal server instance provided when `ipv6_enabled` is set to `true`.
* `ipv6_address_id` - The unique identifier for the public IPv6 address of the bare metal server.
* `secondary_ip_addresses` - The public secondary IPv4 addresses of the bare metal server instance when `secondary_ip_count` is set to non-zero value.
* `global_identifier` - The unique global identifier of the bare metal server.




## ibm_compute_dedicated_host

Provides a Dedicated Host resource. This allows dedicated host to be created, updated, and cancelled.

For additional details please refer to the [SoftLayer API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Virtual_DedicatedHost).

### Sample Terraform code

In the following example, you can create a dedicated host:

```hcl
resource "ibm_compute_dedicated_host" "dedicatedhost" {
  hostname        = "host"
  domain          = "example.com"
  router_hostname = "bcr01a.dal09"
  datacenter      = "dal09"
}

```

### Input parameters

The following arguments are supported:

* `datacenter` - (Required, string) The data center in which the dedicated host resides.
* `hostname` - (Required, string) The host name of dedicatated host.
* `domian` - (Required, string) The domain of dedicatated host..
* `router_hostname` - (Required, string) The hostname of the primary router associated with the dedicated host.
* `hourly_billing` - (Optional, boolean) The billing type for the host. When set to `true`, the dedicated host is billed on hourly usage. Otherwise, the dedicated host is billed on a monthly basis. The default value is `true`.
* `flavor` - (Optional, string) The flavor of dedicated host. Default value `56_CORES_X_242_RAM_X_1_4_TB`. [Log in to the IBM-Cloud Infrastructure (SoftLayer) API to see available flavor types](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/813/getItems.json). Use your API as the password to log in. Log in and find the key called `keyName`.
* `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the dedicated host to become available before declaring it as created. The default value is `90`.
* `tags` - (Optional, array of strings) Tags associated with the dedicated host.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the dedicated host.
* `cpu_count` - The capacity that the dedicated host's CPU allocation is restricted to.
* `disk_capacity` - The capacity that the dedicated host's disk allocation is restricted to.
* `memory_capacity` - The capacity that the dedicated host's memory allocation is restricted to.

### Import

`ibm_compute_dedicated_host` can be imported using the ID.

Example:

```
$ terraform import ibm_compute_dedicated_host.dedicatedhost 238756

```



## ibm_compute_monitor

Provides a monitoring instance resource. This allows monitoring instances to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_Monitor_Version1_Query_Host).

### Sample Terraform code

In the following example, you can create a monitor:

```hcl
resource "ibm_compute_monitor" "test_monitor" {
    guest_id = ${ibm_compute_vm_instance.test_server.id}
    ip_address = ${ibm_compute_vm_instance.test_server.id.ipv4_address}
    query_type_id = 1
    response_action_id = 1
    wait_cycles = 5
    notified_users = [460547]
}
```

### Input parameters

The following arguments are supported:

* `guest_id` - (Required, integer) The ID of the virtual guest you want to monitor.
* `ip_address` - (Optional, strings) The IP address you want to monitor.
* `query_type_id` - (Required, integer) The ID of the query type.
* `response_action_id` - (Required, integer) The ID of the response action to take if the monitor fails. Accepted values are `1` or `2`.
* `wait_cycles` - (Optional, integer) The number of five-minute cycles to wait before the response action is taken.
* `notified_users` - (Optional, array of integers) The list of user IDs that will be notified.
* `tags` - (Optional, array of strings) Tags associated with the monitoring instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the monitor.
* `notified_users` - The list of user IDs that will be notified.




## ibm_compute_placement_group

Provides provisioning placement groups. This allows placement groups to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](https://softlayer.github.io/reference/datatypes/SoftLayer_Virtual_PlacementGroup).

### Sample Terraform code

```hcl
resource "ibm_compute_placement_group" "test_placement_group" {
    name = "test"
    pod = "pod01"
    datacenter = "dal05"  
}
```

#

The following arguments are supported:

* `name` - (Required, string) The descriptive name used to identify a placement group.
* `datacenter` - (Required, string) The datacenter in which you want to provision the placement group.
* `pod` - (Required, string) The pod in which you want to provision the placement group.
* `rule` - (Optional, string) The rule of the placement group. Default `SPREAD`. 
* `tags` - (Optional, array of strings) Tags associated with the placement group.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new placement group.

### Timeouts

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:  
  * `delete` - (Defaults to 10 mins) Used when deleting the placement group. There might be Virtual Guest resources on the placement group. The placement group delete request is issued once there are no Virtual Guests on the placement group.





## ibm_compute_provisioning_hook

Provides provisioning hooks containing all the information needed to add a hook into a server or virtual provision and OS reload. This allows provisioning hooks to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Provisioning_Hook).

### Sample Terraform code

```hcl
resource "ibm_compute_provisioning_hook" "test_provisioning_hook" {
    name = "test_provisioning_hook_name"
    uri  = "https://raw.githubusercontent.com/test/slvm/master/test-script.sh"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The descriptive name used to identify a provisioning hook.
* `uri` - (Required, string) The endpoint from which the script is downloaded or downloaded and executed.
* `tags` - (Optional, array of strings) Tags associated with the provisioning hook instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new provisioning hook.



## ibm_compute_ssh_key

Provide an SSH key resource. This allows SSH keys to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Security_Ssh_Key).

### Sample Terraform code

```
resource "ibm_compute_ssh_key" "test_ssh_key" {
    label = "test_ssh_key_name"
    notes = "test_ssh_key_notes"
    public_key = "ssh-rsa <rsa_public_key>"
}
```

### Input parameters

The following arguments are supported:

* `label` - (Required, string) The descriptive name used to identify an SSH key.
* `public_key` - (Required, string) The public SSH key.
* `notes` - (Optional, string) Descriptive text about the SSH key.
* `tags` - (Optional, array of strings) Tags associated with the SSH Key instance.  

**NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new SSH key.
* `fingerprint` - The sequence of bytes to authenticate or look up a longer SSH key.



## ibm_compute_ssl_certificate

Provides an SSL certificate resource. This allows SSL certificates to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) security certificates docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Security_Certificate).

### Sample Terraform code

In the following example, you can use a certificate on file:

```hcl
resource "ibm_compute_ssl_certificate" "test_cert" {
  certificate = "${file("cert.pem")}"
  private_key = "${file("key.pem")}"
}
```

You can also use an in-line certificate:

```hcl
resource "ibm_compute_ssl_certificate" "test_cert" {
    certificate = <<EOF
[......] # cert contents
-----END CERTIFICATE-----
    EOF

    private_key = <<EOF
-----BEGIN RSA PRIVATE KEY-----
[......] # cert contents
-----END RSA PRIVATE KEY-----
    EOF
}
```

### Input parameters

The following arguments are supported:

* `certificate` - (Required, string) The certificate provided publicly to clients requesting identity credentials.
* `intermediate_certificate` - (Optional, string) The certificate from the intermediate certificate authority, or chain certificate, that completes the chain of trust. Required when clients only trust the root certificate.
* `private_key` - (Required, string) The private key in the key/certificate pair.
* `tags` - (Optional, array of strings) Tags associated with the security certificates instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `common_name` - The common name encoded within the certificate. This name is usually a domain name.
* `create_date` - The date the certificate record was created.
* `id` - The ID of the certificate record.
* `key_size` - The size, expressed in number of bits, of the public key represented by the certificate.
* `modify_date` - The date the certificate record was last modified.
* `organization_name` - The organizational name encoded in the certificate.
* `validity_begin` - The UTC timestamp representing the beginning of the certificate's validity.
* `validity_days` - The number of days remaining in the validity period for the certificate.
* `validity_end` - The UTC timestamp representing the end of the certificate's validity period.



## ibm_compute_user

Provides a SoftLayer user resource. This allows you to retrieve, create, update, and delete either SoftLayer or IBMid accounts.

### SoftLayer Account
If you use this resource to create SoftLayer local credentials, you need to define the `username` and `password` arguments.

To access the SoftLayer portal, the user can log in with the username and password.

To access the SoftLayer API, the user can log in with the username and API key. An `api_key` is generated by SoftLayer when the `has_api_key` flag is `true`.

If the SoftLayer API [getBlueIdAuthenticationRequiredFlag](https://api.softlayer.com/rest/v3/SoftLayer_Account/getBlueIdAuthenticationRequiredFlag) returns `false`, the account is a local account.

#### Sample Terraform code

The following example shows how to use this resource with SoftLayer accounts:

```hcl
resource "ibm_compute_user" "joe" {
    address1     = "12345 Any Street"
    address2     = "Suite #99"
    city         = "Atlanta"
    company_name = "Comp Inc"
    country      = "US"
    email        = "joe@doe.com"
    first_name   = "Joe"
    has_api_key  = false
    last_name    = "Doe"
    username     = "testuser"
    password     = "Change3Me!"
    permissions  = [
        "ACCESS_ALL_GUEST",
        "ACCESS_ALL_HARDWARE",
        "SERVER_ADD",
        "SERVER_CANCEL",
        "RESET_PORTAL_PASSWORD"
    ]
    state        = "GA"
    timezone     = "EST"
}
```

### IBMid Account
You can use an IBMid instead of your SoftLayer credentials. An [IBMid](https://www.ibm.com/account/profile/us) is used as a consistent way to access IBM products. You can create an IBMid in advance.

If you use this resource for an IBMid account, the resource should not contain `username` and `password` arguments. The user name is generated by SoftLayer by combining the IBMid account number and the IBMid email address. The password is not used in an IBMid account.

To access the SoftLayer portal, the user can log in with the IBMid.

To access the SoftLayer API, the user can log in with the username generated by SoftLayer and the API key. An `api_key` is generated by SoftLayer when the `has_api_key` flag is `true`.

If the SoftLayer API  [getBlueIdAuthenticationRequiredFlag](https://api.softlayer.com/rest/v3/SoftLayer_Account/getBlueIdAuthenticationRequiredFlag)
returns `true`, the account is an IBMid.

#### Example Usage

The following example shows how to use this resource with IBMid accounts:

```hcl
resource "ibm_compute_user" "joe" {
    address1     = "12345 Any Street"
    address2     = "Suite #99"
    city         = "Atlanta"
    company_name = "Comp Inc"
    country      = "US"
    email        = "joe@doe.com"
    first_name   = "Joe"
    has_api_key  = false
    last_name    = "Doe"
    permissions  = [
        "ACCESS_ALL_GUEST",
        "ACCESS_ALL_HARDWARE",
        "SERVER_ADD",
        "SERVER_CANCEL",
        "RESET_PORTAL_PASSWORD"
    ]
    state        = "GA"
    timezone     = "EST"
}
```


**NOTE**: An [IBMid](https://www.ibm.com/account/profile/us) is used as a consistent way to access IBM products. You can create an IBMid in advance.

For additional details, see the [SoftLayer API documentation](http://sldn.softlayer.com/reference/datatypes/SoftLayer_User_Customer).

### Input parameters

The following arguments are supported:

* `address1` - (Required, string) The first line of the user's street address.
* `address2` - (Optional, string) The second line of the user's street address.
* `city` - (Required, string) The user's city.
* `company_name` - (Required, string) The user's company.
* `country` - (Required, string) The user's country.
* `email` - (Required, string) The email address associated with the account.
* `first_name` - (Required, string) The user's first name.
* `has_api_key` - (Optional, boolean) When the value is `true`, a SoftLayer API key is created for the user. The key is returned in the `api_key` attribute. When the value is `false`, any existing SoftLayer API keys for the user are deleted. The default value is `false`.
* `last_name` - (Required, string) The user's last name.
* `username` - (Required for SoftLayer accounts, optional for IBMid accounts, string) A unique name to identify a user globally across all SoftLayer logins. The username is also the user login. Once a username is created, it cannot be changed. You must define a username when the account is a SoftLayer account. The user name is generated by SoftLayer when the account is an IBMid account. For example, if an IBMid had an account number of `1234567` and an email addess (IBMid) of `test@example.com`, then Softlayer would generate `1234567_test@example.com` as the username. This argument is optional for an IBMid account.
* `password` - (Required for SoftLayer accounts, string) The initial password for the user account. The password is hashed and encoded before it is stored in the Terraform state file. For an IBMid account, the password argument is ignored. For a SoftLayer account, the password must conform to SoftLayer's password policies to avoid failures.
  Valid passwords must meet the following rules:
  * Be 8 to 20 characters in length.
  * Have a combination of upper and lowercase characters.
  * Contain at least one number.
  * Contain at least one of the following special characters: `_`, `-`, `|`, `@`, `.`, `, `, `?`, `/`, `!`, `~`, `#`, `$`, `%`, `^`, `&`, `*`, `(`, `)`, `{`, `}`, `[`, `]`, `=`.
* `permissions` - (Optional, string) Permissions assigned to this user. This is a set of zero or more string values. See the [SoftLayer API doc for user permissions](https://sldn.softlayer.com/reference/datatypes/SoftLayer_User_Customer_CustomerPermission_Permission).
* `state` - (Required, string) The state of a user's street address.
* `timezone` - (Required, string) The user's timezone as a short name value (e.g., "EST"). For accepted values, see the [SoftLayer API doc for timezones ](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Locale_Timezone).
* `user_status` - (Optional, string) The user's login status. You can find accepted values in the [SoftLayer API doc for user status](http://sldn.softlayer.com/reference/datatypes/SoftLayer_User_Customer_Status). The default value is `ACTIVE`.
* `tags` - (Optional, array of strings) Tags associated with the user account instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `api_key`: The SoftLayer API key that is created for the user.
* `id`: The unique SoftLayer identifier that is created for the user.
* `ibm_id`: The username for IBMid accounts. This attribute is generated by SoftLayer when the IBMid is activated.

### Additional Notes

In SoftLayer, there is a delay when user logins are deleted from SoftLayer backend systems. SoftLayer acknowledges delete requests and immediately updates the user status to `CANCEL_PENDING`. The actual deletion from the SoftLayer system processes at an unspecified time in the future.

This delay may be significant, especially during your project's testing phase. You may receive an error if you create a user login, then delete it, and then create it again. This results in an error because the SoftLayer backend has not completely processed the previous delete operation. If you want create, delete, and recreate a user login, you must specify a new, globally-unique username in your subsequent requests.



## ibm_compute_vm_instance

Provides a resource for VM instances. This allows VM instances to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/services/SoftLayer_Virtual_Guest).

**NOTE**: Update is not supported when the `bulk_vms` parameter is used.

### Sample Terraform code

In the following example, you can create a VM instance using a Debian image:

```hcl
resource "ibm_compute_vm_instance" "twc_terraform_sample" {
    hostname = "twc-terraform-sample-name"
    domain = "bar.example.com"
    os_reference_code = "DEBIAN_8_64"
    datacenter = "wdc01"
    network_speed = 10
    hourly_billing = true
    private_network_only = false
    cores = 1
    memory = 1024
    disks = [25, 10, 20]
    user_metadata = "{\"value\":\"newvalue\"}"
    dedicated_acct_host_only = true
    local_disk = false
    public_vlan_id = 1391277
    private_vlan_id = 7721931
    private_security_group_ids = [576973]
}
```

In the following example, you can create a VM instance using a block device template:

```hcl
resource "ibm_compute_vm_instance" "terraform-sample-BDTGroup" {
   hostname = "terraform-sample-blockDeviceTemplateGroup"
   domain = "bar.example.com"
   datacenter = "ams01"
   public_network_speed = 10
   hourly_billing = false
   cores = 1
   memory = 1024
   local_disk = false
   image_id = 12345
   tags = [
     "collectd",
     "mesos-master"
   ]
   public_subnet = "50.97.46.160/28"
   private_subnet = "10.56.109.128/26"
}
```

In the following example, you can create a VM instance using a flavor:

```hcl
resource "ibm_compute_vm_instance" "terraform-sample-flavor" {
    hostname = "terraform-sample-flavor"
    domain = "bar.example.com"
    os_reference_code = "DEBIAN_8_64"
    datacenter = "dal06"
    network_speed = 10
    hourly_billing = true
    local_disk = false
    private_network_only = false
    flavor_key_name = "B1_2X8X25"
    user_metadata = "{\\\"value\\\":\\\"newvalue\\\"}"
    // provide disk 3, 4, 5 and so on
    disks = [10, 20, 30]
    tags = ["collectd"]
    // It should be false
    dedicated_acct_host_only = false
    ipv6_enabled = true
    secondary_ip_count = 4
    notes = "VM notes"
}
```
In the following example, you can create multiple vm's

```hcl
resource "ibm_compute_vm_instance" "terraform-bulk-vms" {
  bulk_vms = [{
    hostname = "vm1"

    domain = "bar.example.com"
  },
    {
      hostname = "vm2"

      domain = "bar.example.com"
    },
  ]

  os_reference_code    = "CENTOS_7_64"
  datacenter           = "dal09"
  network_speed        = 100
  hourly_billing       = true
  private_network_only = true
  cores                = 1
  memory               = 1024
  disks                = [25]
  local_disk           = false
}
```

In the following example, you can retry to create a VM instance using a datacenter_choice. If VM fails to place order on first datacenter or vlans it retries to place order on subsequent datacenters and vlans untill place order is successfull:

```hcl
resource "ibm_compute_vm_instance" "terraform-retry" {
  hostname          = "vmretry"
  domain            = "example.com"
  network_speed     = 100
  hourly_billing    = true
  cores             = 1
  memory            = 1024
  local_disk        = false
  os_reference_code = "DEBIAN_7_64"
  disks             = [25]

  datacenter_choice = [
    {
      datacenter      = "dal09"
      public_vlan_id  = 123245
      private_vlan_id = 123255
    },
    {
      datacenter = "wdc54"
    },
    {
      datacenter      = "dal09"
      public_vlan_id  = 153345
      private_vlan_id = 123255
    },
    {
      datacenter = "dal06"
    },
    {
      datacenter      = "dal09"
      public_vlan_id  = 123245
      private_vlan_id = 123255
    },
    {
      datacenter      = "dal09"
      public_vlan_id  = 1232454
      private_vlan_id = 1234567
    },
  ]
}

```


### Input parameters

The following arguments are supported:

* `hostname` - (Optional, string) The hostname for the computing instance.</br>
    **NOTE**: Conflicts with `bulk_vms`.
* `domain` - (Optional, string)  The domain for the computing instance.</br>
    **NOTE**: Conflicts with `bulk_vms`.
* `bulk_vms` - (Optional, list) List of hostname and domain of the computing instance. The minimum number of vm's to be defined is 2. Nested `bulk_vms` blocks must have the following structure:
    * `hostname` - (Required, string) The hostname for the computing instance.
    * `domain` - (Required, string) The domain for the computing instance.</br>
    **NOTE**: Conflicts with `hostname` and `domain`.
* `cores` - (Optional, integer) The number of CPU cores that you want to allocate.  
    **NOTE**: Conflicts with `flavor_key_name`.
* `memory` - (Optional, integer) The amount of memory, expressed in megabytes, that you want to allocate.  
    **NOTE**: Conflicts with `flavor_key_name`.
* `flavor_key_name` - (Optional, string) The flavor key name that you want to use to provision the instance. To see available Flavor key name, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3/SoftLayer_Virtual_Guest/getCreateObjectOptions.json), using your API key as the password.  
    **NOTE**: Conflicts with `cores` and `memory`.
* `datacenter` - (Optional, string) The datacenter in which you want to provision the instance.  
    **NOTE**: If `dedicated_host_name` or `dedicated_host_id`
    is provided then the datacenter should be same as the dedicated host datacenter. 
    If `placement_group_name` or `placement_group_id`
    is provided then the datacenter should be same as the placement group datacenter.
    Conflicts with `datacenter_choice`. 
* `hourly_billing` - (Optional, boolean) The billing type for the instance. When set to `true`, the computing instance is billed on hourly usage. Otherwise, the instance is billed on a monthly basis. The default value is `true`.
* `local_disk`- (Optional, boolean) The disk type for the instance. When set to `true`, the disks for the computing instance are provisioned on the host that the instance runs. Otherwise, SAN disks are provisioned. The default value is `true`.
* `dedicated_acct_host_only` - (Optional, boolean) Specifies whether the instance must only run on hosts with instances from the same account. The default value is `false`. If VM is provisioned using flavorKeyName, value should be set to `false`.  
     **NOTE**: Conflicts with `dedicated_host_name`, `dedicated_host_id`, `placement_group_name` and `placement_group_id`.
* `dedicated_host_id` - (Optional, integer) Specifies [dedicated host](https://cloud.ibm.com/docs/vsi/vsi_dedicated.html) for the instance by its id.  
     **NOTE**: Conflicts with `dedicated_acct_host_only`, `dedicated_host_name`, `placement_group_name` and `placement_group_id`.
* `dedicated_host_name` - (Optional, string) Specifies [dedicated host](https://cloud.ibm.com/docs/vsi/vsi_dedicated.html) for the instance by its name.  
     **NOTE**: Conflicts with `dedicated_acct_host_only`, `dedicated_host_id`, `placement_group_name` and `placement_group_id`.
* `placement_group_id` - (Optional, integer) Specifies [placement group](https://cloud.ibm.com/docs/vsi/vsi_dedicated.html) for the instance by its id.  
     **NOTE**: Conflicts with `dedicated_acct_host_only`, `dedicated_host_name`, `dedicated_host_id` and `placement_group_name`.
* `placement_group_name` - (Optional, string) Specifies [placement group](https://cloud.ibm.com/docs/vsi/vsi_dedicated.html) for the instance by its name.  
     **NOTE**: Conflicts with `dedicated_acct_host_only`, `dedicated_host_id`, `dedicated_host_name` and `placement_group_id`
* `transient` - (Optional, boolean) Specifies whether to provision a transient virtual server. The default value is `false`. Transient instances cannot be upgraded or downgraded. Transient instances cannot use local storage.  
    **NOTE**: Conflicts with `dedicated_acct_host_only`, `dedicated_host_id`, `dedicated_host_name`, `cores`, `memory`, `public_bandwidth_limited` and `public_bandwidth_unlimited`
* `os_reference_code` - (Optional, string) The operating system reference code that is used to provision the computing instance. To see available OS reference codes, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3/SoftLayer_Virtual_Guest_Block_Device_Template_Group/getVhdImportSoftwareDescriptions.json?objectMask=referenceCode), using your API key as the password.  
    **NOTE**: Conflicts with `image_id`.
*   `image_id` - (Optional, integer) The image template ID you want to use to provision the computing instance. This is not the global identifier (UUID), but the image template group ID that should point to a valid global identifier. To retrieve the image template ID from the IBM Cloud infrastructure customer portal, navigate to **Devices > Manage > Images**, click the desired image, and note the ID number in the resulting URL.  

    **NOTE**: Conflicts with `os_reference_code`. If you don't know the ID(s) of your image templates, you can [refer to an image template ID by name using a data source](../d/compute_image_template.html).
*  `network_speed` - (Optional, integer) The connection speed (in Mbps) for the instance's network components. The default value is `100`.
*  `private_network_only` - (Optional, boolean) When set to `true`, a compute instance only has access to the private network. The default value is `false`.
*  `private_security_group_ids` - (Optional, array of integers) The ids of security groups to apply on the private interface.
This attribute can't be updated. This is provided so that you can apply security groups to  your VSI right from the beginning, the first time it comes up. If you would like to add or remove security groups in the future to this VSI then you should consider using `ibm_network_interface_sg_attachment` resource. If you use this attribute in addition to `ibm_network_interface_sg_attachment` resource you might get some spurious diffs. So use one of these consistently for a particular VSI.
*  `public_vlan_id` - (Optional, integer) The public VLAN ID for the public network interface of the instance. Accepted values are in the [VLAN doc](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID number in the browser URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).  
    **NOTE**: Conflicts with `datacenter_choice`.
* `private_vlan_id` - (Optional, integer) The private VLAN ID for the private network interface of the instance. You can find accepted values in the [VLAN doc](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID number in the browser URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).  
    **NOTE**: Conflicts with `datacenter_choice`.
* `public_security_group_ids` - (Optional, array of integers) The ids of security groups to apply on the public interface.
This attribute can't be updated. This is provided so that you can apply security groups to  your VSI right from the beginning, the first time it comes up. If you would like to add or remove security groups in the future to this VSI then you should consider using `ibm_network_interface_sg_attachment` resource. If you use this attribute in addition to `ibm_network_interface_sg_attachment` resource you might get some spurious diffs. So use one of these consistently for a particular VSI.
* `public_subnet` - (Optional, string) The public subnet for the public network interface of the instance. Accepted values are primary public networks. You can find accepted values in the [subnets doc](https://cloud.ibm.com/classic/network/subnets).
* `private_subnet` - (Optional, string) The private subnet for the private network interface of the instance. Accepted values are primary private networks. You can find accepted values in the [subnets doc](https://cloud.ibm.com/classic/network/subnets).
* `disks` - (Optional, array of integers) The numeric disk sizes (in GBs) for the instance's block device and disk image settings. The default value is the smallest available capacity for the primary disk. If you specify an image template, the template provides the disk capacity. If you specify the flavorKeyName, first disk is provided by the flavor.
* `user_metadata` - (Optional, string) Arbitrary data to be made available to the computing instance.
*  `notes` - (Optional, string) Descriptive text of up to 1000 characters about the VM instance.
* `ssh_key_ids` - (Optional, array of numbers) The SSH key IDs to install on the computing instance when the instance provisions.  
    **NOTE:** If you don't know the ID(s) for your SSH keys, you can [reference your SSH keys by their labels](../d/compute_ssh_key.html).
* `file_storage_ids` - (Optional, array of numbers) File storage to which this computing instance should have access. File storage must be in the same data center as the bare metal server. If you use this argument to authorize access to file storage, then do not use the `allowed_virtual_guest_ids` argument in the `ibm_storage_file` resource in order to prevent the same storage being added twice.
* `block_storage_ids` - (Optional, array of numbers) File storage to which this computing instance should have access. File storage must be in the same data center as the bare metal server. If you use this argument to authorize access to file storage, then do not use the `allowed_virtual_guest_ids` argument in the `ibm_storage_block` resource in order to prevent the same storage being added twice.
* `post_install_script_uri` - (Optional, string) The URI of the script to be downloaded and executed after installation is complete.
* `tags` - (Optional, array of strings) Tags associated with the VM instance. Permitted characters include: A-Z, 0-9, whitespace, _ (underscore), - (hyphen), . (period), and : (colon). All other characters are removed.
* `ipv6_enabled` - (Optional, boolean) The primary public IPv6 address. The default value is `false`.
* `ipv6_static_enabled` - (Optional, boolean) The public static IPv6 address block of `/64`. The default value is `false`.
*  `secondary_ip_count` - (Optional, integer) Specifies secondary public IPv4 addresses. Accepted values are `4` and `8`.
*  `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the VM instance to become available before declaring it as created. It is also the same amount of time waited for no active transactions before proceeding with an update or deletion. The default value is `90`.
* `public_bandwidth_limited` - (Optional, int). Allowed public network traffic(GB) per month. It can be greater than 0 when the server is a monthly based server. Defaults to the smallest available capacity for the public bandwidth are used.  
    **NOTE**: Conflicts with `private_network_only` and `public_bandwidth_unlimited`.
* `public_bandwidth_unlimited` - (Optional, boolean). Allowed unlimited public network traffic(GB) per month for a monthly based server. The `network_speed` should be 100 Mbps. Default value: `false`.  
    **NOTE**: Conflicts with `private_network_only` and `public_bandwidth_limited`.
* `evault` - (Optional, int). Allowed evault(GB) per month for monthly based servers.
* `datacenter_choice` - (Optional, list) A nested block to describe datacenter choice options to retry on different datacenters and vlans. Nested `datacenter_choice` blocks must have the following structure:
    * `datacenter` - (Required, string) The datacenter in which you want to provision the instance.
    * `public_vlan_id` - (Optional, string) The public VLAN ID for the public network interface of the instance. Accepted values are in the [VLAN doc](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID number in the browser URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
    * `private_vlan_id` - (Optional, string) The private VLAN ID for the private network interface of the instance. You can find accepted values in the [VLAN doc](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID number in the browser URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).   
    **NOTE**: Conflicts with `datacenter`, `private_vlan_id`, `public_vlan_id`, `placement_group_name` and `placement_group_id`.



### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VM instance.
* `ipv4_address` - The public IPv4 address of the VM instance.
* `ip_address_id_private` - The unique identifier for the private IPv4 address assigned to the VM instance.
* `ipv4_address_private` - The private IPv4 address of the VM instance.
* `ip_address_id` - The unique identifier for the public IPv4 address assigned to the VM instance.
* `ipv6_address` - The public IPv6 address of the VM instance provided when `ipv6_enabled` is set to `true`.
* `ipv6_address_id` - The unique identifier for the public IPv6 address assigned to the VM instance provided when `ipv6_enabled` is set to `true`.
* `private_subnet_id` - The unique identifier of the subnet `ipv4_address_private` belongs to.
* `public_ipv6_subnet` - The public IPv6 subnet provided when `ipv6_enabled` is set to `true`.
* `public_ipv6_subnet_id` - The unique identifier of the subnet `ipv6_address` belongs to.
* `public_subnet_id` - The unique identifier of the subnet `ipv4_address` belongs to.
* `secondary_ip_addresses` - The public secondary IPv4 addresses of the VM instance.
* `public_interface_id` - The ID of the primary public interface.
* `private_interface_id` - The ID of the primary private interface.



## ibm_database

Creates an IBM Cloud Database (ICD) instance resource. This resource allows database instances to be created, updated, and deleted. The ibmcloud_api_key used by Terraform must have been granted sufficient IAM rights to create and modify IBM Cloud Databases and have access to the Resource Group the ICD instance will be associated with. See https://cloud.ibm.com/docs/services/databases-for-postgresql/reference-access-management.html#identity-and-access-management for more details on setting IAM and Access Group rights to manage ICD instances.  

If no resource_group_id is specified, the ICD instance is created under the default resource group. The API_KEY must have been assigned permissions for this group.  

Configuration of an ICD resource requires that the `region` parameter is set for the IBM provider in the `provider.tf` to be the same as the target ICD `location/region`. If not specified it will default to `us-south`. A `terraform apply` will fail if the ICD `location` is set differently. If the Terraform configuration needs to deploy resources into multiple regions, provider alias' can be used. https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances

### Sample Terraform code

```hcl
data "ibm_resource_group" "group" {
  name = "<your_group>"
}

resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-etcd"
  resource_group_id = "${data.ibm_resource_group.group.id}"
  tags              = ["tag1", "tag2"]

  adminpassword     = "password12"
  members_memory_allocation_mb = 3072
  members_disk_allocation_mb   = 61440
  users = {
          name     = "user123"
          password = "password12"
          }
  whitelist = {
          address     = "172.168.1.1/32"
          description = "desc"
          }
}

output "ICD Etcd database connection string" {
  value = "http://${"${ibm_database.test_acc.connectionstrings.0.composed}"}"
}
```

provider.tf

```hcl

provider "ibm" {
  ibmcloud_api_key    = "${var.ibmcloud_api_key}"
  region             = "eu-gb"
}
```

See https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-database for an example of a VSI configured to connect to a PostgreSQL DB.  




The following arguments are supported:

* `name` - (Required, string) A descriptive name used to identify the database instance. The name must not include spaces. 
* `plan` - (Required, string) The name of the plan type for an IBM Cloud Database. The only currently supported value is "standard"
* `location` - (Required, string) Any of the currently supported ICD regions. The IBM provider `location` in the provider definition also needs to be set to the same region as the target ICD region. The default provider region is `us-south`. The following regions are currently supported: `us-south`, `us-east`, `eu-gb`, `eu-de`, `au-syd`, `jp-tok`, `oslo01`.  
* `resource_group_id` - (Optional, string) The ID of the resource group where you want to create the service. You can retrieve the value from data source `ibm_resource_group`. If not provided it creates the service in default resource group.
* `tags` - (Optional, array of strings) Tags associated with the instance.
* `service` - (Required, string) The ICD database type to be created. Only the following services are currently accepted: 
`databases-for-etcd`, `databases-for-postgresql`, `databases-for-redis`, `databases-for-elasticsearch`, `messages-for-rabbitmq`, `databases-for-mongodb`
* `version` - (Optiona, string)  The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version.
* `adminpassword` - (Optional, string) If not specified the password is unitialised and the id unusable. In this case addditional users must be specified in a user block.   
* `members_memory_allocation_mb` - (Optional) The memory size for the database, split across all members. If not specified defaults to the database default. These vary by database type. See the documentation related to each database for the defaults. https://cloud.ibm.com/docs/services/databases-for-postgresql/howto-provisioning.html#list-of-additional-parameters
* `members_disk_allocation_mb`  - (Optional) The disk size of the database, split across all members. As above.
* `members_cpu_allocation_count` - (Optional, int) Enables and allocates the number of specified dedicated cores to your deployment. 
* `backup_id` - (Optional, string) A CRN of a backup resource to restore from. The backup must have been created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format crn:v1:<...>:backup:<uuid>. If omitted, the database is provisioned empty.
* `key_protect_key` - (Optional, string) The CRN of a Key Protect key, which is then used for disk encryption. A key protect CRN is in the format crn:v1:<...>:key:<id>.
* `key_protect_instance` - (Optional, string) The CRN of a Key Protect instance, which is then used for disk encryption. A key protect CRN is in the format crn:v1:<...>::.
* `service_endpoints` - (Optional, string) Selects the types Service Endpoints supported on your deployment. Options are public, private, or public-and-private. The default is `public`.

* `users` - (Optional) - Multiple blocks allowed       
  * `name` - Name of the userid to add to the database instance, Minimum of 5 characters up to 32.  
  * `password` - Password for the userid, minimum of 10 characters up to 32. 
            
* `whitelist` - (Optional) - Multiple blocks allowed             
  * `address` - IP address or range of db client addresses to be whitelisted in CIDR format, `172.168.1.2/32`
  * `description` -  Unique description for white list range



### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the new database instance (CRN).
* `status` - Status of resource instance.
* `adminuser` - userid of the default admistration user for the database, usually `admin` or `root`.
* `version` - Database version. 
* `connectionstrings` - List of connection strings by userid for the database. See the IBM Cloud documentation for more details of how to use connection strings in ICD for database access: https://cloud.ibm.com/docs/services/databases-for-postgresql/howto-getting-connection-strings.html#getting-your-connection-strings. The results are returned in pairs of the userid and string:
  `connectionstrings.1.name = admin`
  `connectionstrings.1.string = postgres://admin:$PASSWORD@79226bd4-4076-4873-b5ce-b1dba48ff8c4.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:32554/ibmclouddb?sslmode=verify-full`
Individual string parameters can be retrieved using TF vars and outputs  `connectionstrings.x.hosts.x.port` and `connectionstrings.x.hosts.x.host` 


### Import

The `ibm_database` resource can be imported using the `ID`. The ID is formed from the `CRN` (Cloud Resource Name) from the **Overview** page of the Cloud Database instance. It can be found under the heading **Deployment Details**
* CRN is a 120 digit character string of the form: `crn:v1:bluemix:public:databases-for-postgresql:us-south:a/4ea1882a2d3401ed1e459979941966ea:79226bd4-4076-4873-b5ce-b1dba48ff8c4::`

The `region` parameter must be set for the IBM provider in `provider.tf` to be the same as the ICD service `location(region)`. If not specified it will default to `us-south`. A `terraform refresh/apply` of the data_source will fail if the ICD instance is not in the same region as specified for the provider or its alias.  

```
$ terraform import ibm_database.my_db <crn>

$ terraform import ibm_database.my_db crn:v1:bluemix:public:databases-for-postgresql:us-south:a/4ea1882a2d3401ed1e459979941966ea:79226bd4-4076-4873-b5ce-b1dba48ff8c4::
```

Import requires a minimal Terrform config file to allow importing. 

```hcl
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
```

Run `terraform state show ibm_database.<your_database>` after import to retrieve the additional values to be included in the resource config file. Note that ICD only exports the admin userid. It does not export any additional userids and passwords configured on the instance. These values must be retrieved from an alternative source. If new passwords need to be configured or the connection string retrieved to use the service, a new `users` block must be defined to create new users. This limitation is due to a lack of ICD functionality.  


### Timeouts

ibm_database provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for Creating Instance.
* `update` - (Default 20 minutes) Used for Updating Instance.
* `delete` - (Default 10 minutes) Used for Deleting Instance.

ICD instance create typically takes between 10 to 20 minutes. Delete and update in minutes. Provisioning time can be unpredictable. If the apply fails due to a timeout, import the database resource after it has finished creation.  





## ibm_dns_domain

Provides a single DNS domain managed on IBM Cloud Classic Infrastructure (SoftLayer). Domains contain general information about the domain name, such as the name and serial number.

Individual records, such as `A`, `AAAA`, `CTYPE`, and `MX` records, are stored in the domain's associated resource records using the [`ibm_dns_record` resource](../r/dns_record.html).


### Sample Terraform code

```hcl
resource "ibm_dns_domain" "dns-domain-test" {
    name = "dns-domain-test.com"
    target = "127.0.0.10"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The domain's name, including the top-level domain. For example, "example.com". When the domain is created, proper `NS` and `SOA` records are created automatically for the domain.
* `target` - (Optional, string) The primary target IP address to which the domain resolves. When the domain is created, an `A` record with a host value of `@` and a data-target value of the IP address are provided and associated with the new domain
* `tags` - (Optional, array of strings) Tags associated with the DNS domain instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique internal identifier of the domain record.
* `serial` - A unique number denoting the latest revision of the domain.
* `update_date` - The date that the domain record was last updated.



## ibm_dns_domain_registration_nameservers

Configures the (custom) name servers associated with a DNS domain registration managed by the IBM Cloud DNS Registration Service. The default IBM Cloud name servers specified when the domain was initially registered are replaced with the values passed when this resource is created. 

This resource is typically used in conjunction with IBM Cloud Internet Services to enable DNS services for the domain to be managed via IBM Cloud Internet Services. All futher configuration of the domain is then performed using the Cloud Internet Services resource instances. To transfer management control, the IBM Cloud DNS domain registration is updated with the Internet Services specific name servers. This step is required before the domain in Cloud Internet Services becomes active and will start serving web traffic. Using interpolation syntax, the computed name servers of the CIS resource are passed into this resource. 


### Sample Terraform code

```hcl
resource "ibm_dns_domain_registration_nameservers" "dnstestdomain" {
    dns_registration_id = "${data.ibm_dns_domain_registration.dnstestdomain.id}"
    name_servers = "${ibm_cis_domain.dnstestdomain.name_servers}" 
}
data "ibm_dns_domain_registration" "dnstestdomain" {
    name = "dnstestdomain.com"
}
resource "ibm_cis_domain" "dnstestdomain" {
   
}
```

Or 

```hcl
resource "ibm_dns_domain_registration_nameservers" "dns-domain-test" {
    dns_registration_id = "${data.ibm_dns_domain_registration.dns-domain-test.id}"
    name_servers = ["ns006.name.ibm.cloud.com", "ns017.name.ibm.cloud.com"] 
}
data "ibm_dns_domain_registration" "dns-domain-test" {
    name = "test-domain.com"
}
```


### Input parameters

The following arguments are supported:

* `dns_registration_id` - (Required, string) The unique id of the domain's registration. This is exported by the ibm_dns_domain_registration data source. 
* `name_servers` - (Required, Array of strings) E.g. an array of name servers returned from configuration of a domain on a instance of IBM Cloud Internet Services. This is of the format: ["ns006.name.cloud.ibm.com", "ns017.name.cloud.ibm.com"]


### Output parameters

The following attributes are exported:

* `id` - The unique internal identifier of the domain registration record.
* `name_servers` - The new name servers pointing to the new DNS management service provider
* `original_name_servers` - The original name servers configured at the time of domain registration.



## ibm_dns_secondary

The `ibm_dns_secondary` resource represents a single secondary DNS zone managed on SoftLayer. Each record created within the secondary DNS service defines which zone is transferred, what server it is transferred from, and the frequency that zone transfers occur at. Zone transfers are performed automatically based on the transfer frequency set on the secondary DNS record.

### Sample Terraform code

```hcl
resource "ibm_dns_secondary" "dns-secondary-test" {
    zone_name = "dns-secondary-test.com"
    master_ip_address = "127.0.0.10"
    transfer_frequency = 10
}
```

### Input parameters

The following arguments are supported:

* `zone_name` - (Required, string) The name of the zone that is transferred.
* `transfer_frequency` - (Required, int) Signifies how often a secondary DNS zone should be transferred in minutes.
* `master_ip_address` - (Required, string)  The IP address of the master name server where a secondary DNS zone is transferred from.
* `tags` - (Optional, array of strings) Tags associated with the DNS secondary instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - A secondary zone's internal identifier.
* `status_id` - The current status of a secondary DNS record.
* `status_text` - The textual representation of a secondary DNS zone's status.


## ibm_dns_reverse_record

Provides a single DNS reverse record managed on IBM Cloud Classic Infrastructure (SoftLayer). Record contain general information about the reverse record, such as the hostname, ip address and time to leave(ttl).

The IBM Cloud Classic Infrastructure (SoftLayer) object  [SoftLayer_Dns_Domain_ResourceRecord](https://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord) is used for most CRUD operations.

### Sample Terraform code
```hcl
resource "ibm_dns_reverse_record" "testreverserecord" {
    ipaddress="123.123.123.123"
    hostname="www.example.com"
    ttl=900
}
```

### Input parameters

The following arguments are supported:

* `ipaddress` - (Required, string) The IP address or a hostname of a domain's resource record.

* `hostname` - (Required, string) The host defined by a reverse record.

* `ttl` - (Optional, integer) The time to live (TTL) duration, expressed in seconds, of a resource record. Default value is 604800 seconds.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of reverse dns record.


## ibm_dns_record

Provides a single-resource record entry in `ibm_dns_domain`. Each resource record contains a `host` and a `data` property to define the name and target data of a resource.

The IBM Cloud Classic Infrastructure (SoftLayer) object  [SoftLayer_Dns_Domain_ResourceRecord](https://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord) is used for most CRUD operations. The IBM Cloud Classic Infrastructure (SoftLayer) object [SoftLayer_Dns_Domain_ResourceRecord_SrvType](https://sldn.softlayer.com/reference/services/SoftLayer_Dns_Domain_ResourceRecord_SrvType) is used for SRV record types.

The SOA and NS records are automatically created by IBM Cloud Classic Infrastructure (SoftLayer) when the domain is created, you don't need to create those manually.

### Sample Terraform code

#### `A` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_AType) to properly implement the `A` record.

```hcl
resource "ibm_dns_domain" "main" {
    name = "main.example.com"
}

resource "ibm_dns_record" "www" {
    data = "123.123.123.123"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "www.example.com"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "a"
}
```

#### `AAAA` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_AaaaType) to properly implement the `AAAA` record.

```hcl
resource "ibm_dns_record" "aaaa" {
    data = "fe80:0000:0000:0000:0202:b3ff:fe1e:8329"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "www.example.com"
    responsible_person = "user@softlayer.com"
    ttl = 1000
    type = "aaaa"
}
```

#### `CNAME` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs] to properly implement the `CNAME` record.

```hcl
resource "ibm_dns_record" "cname" {
    data = "real-host.example.com."
    domain_id = "${ibm_dns_domain.main.id}"
    host = "alias.example.com"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "cname"
}
```

#### `NS` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_NsType) to properly implement the `NS` record.

```hcl
resource "ibm_dns_record" "recordNS" {
    data = "ns.example.com."
    domain_id = "${ibm_dns_domain.main.id}"
    host = "example.com"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "ns"
}
```

#### `MX` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_MxType) to properly implement the `MX` record.

```hcl
resource "sibm_dns_record" "recordMX-1" {
    data = "mail-1"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "@"
    mx_priority = "10"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "mx"
}
```

#### `SOA` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_SoaType) to properly implement the `SOA` record.

```hcl
resource "ibm_dns_record" "recordSOA" {
    data = "ns1.example.com. abuse.example.com. 2018101002 7200 600 1728000 43200"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "example.com"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "soa"
}
```

#### `SPF` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_SpfType) to properly implement the `SPF` record.

```hcl
resource "ibm_dns_record" "recordSPF" {
    data = "v=spf1 mx:mail.example.org ~all"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "mail-1"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "spf"
}
```

#### `TXT` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_TxtType/) to properly implement the `TXT` record.

```hcl
resource "ibm_dns_record" "recordTXT" {
    data = "host"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "A SPF test host"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "txt"
}
```

#### `SRV` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_SrvType) to properly implement the `SRV` record.

```hcl
resource "ibm_dns_record" "recordSRV" {
    data = "ns1.example.org"
    domain_id = "${ibm_dns_domain.main.id}"
    host = "hosta-srv.com"
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "srv"
    port = 8080
    priority = 3
    protocol = "_tcp"
    weight = 3
    service = "_mail"
}
```

#### `PTR` Record

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Dns_Domain_ResourceRecord_PtrType/) to properly implement the `PTR` record.

```hcl
resource "ibm_dns_record" "recordPTR" {
    data = "ptr.example.com"
    domain_id = "${ibm_dns_domain.main.id}"

## The host is the last octet of IP address in the range of the subnet
    host = "45"  
    responsible_person = "user@softlayer.com"
    ttl = 900
    type = "ptr"
}
```

### Input parameters

The following arguments are supported:

* `data` - (Required, string) The IP address or a hostname of a domain's resource record. Fully qualified host and domain name data must end with the `.` character.
* `domain_id` - (Required, integer) The ID for the domain associated with the resource record.
* `expire` - (Optional, integer) The duration, expressed in seconds, that a secondary name server (or servers) holds a zone before it is no longer considered authoritative.
* `host` - (Required, string) The host defined by a resource record. The `@` symbol denotes a wildcard.
* `minimum_ttl` - (Optional, integer) The duration, expressed in seconds, that a domain's resource records are valid. This is also known as a minimum time to live (TTL), and can be overridden by an individual resource record's TTL.
* `mx_priority` - (Optional, integer) The priority of the mail exchanger that delivers mail for a domain. This is useful in cases where a domain has more than one mail exchanger. A lower number denotes a higher priority, and mail will attempt to deliver through the highest priority exchanger before moving to lower priority exchangers. The default value is `0`.
* `refresh` - (Optional, integer) The duration, expressed in seconds, that a secondary name server waits to check the domain's primary name server for a new copy of a DNS zone. If a zone file has changed, the secondary DNS server updates its copy of the zone to match the primary DNS server's zone.
* `responsible_person` - (Required, string) The email address of the person responsible for a domain. Replace the `@` symbol in the address with a `.`. For example: root@example.org would be expressed as `root.example.org.`.
* `retry` - (Optional, integer) The duration, expressed in seconds, that the domain's primary name server (or servers) waits before attempting to refresh the domain's zone with the secondary name server. A failed attempt to refresh by a secondary name server triggers the retry action.
* `ttl` - (Required, integer) The time to live (TTL) duration, expressed in seconds, of a resource record. A name server uses TTL to determine how long to cache a resource record. An SOA record's TTL value defines the domain's overall TTL.
* `type` - (Required, string) The type of domain resource record. Accepted values are as follows:
    * `a` for address records
    * `aaaa` for address records
    * `cname` for canonical name records
    * `mx` for mail exchanger records
    * `ptr` for pointer records in reverse domains
    * `spf` for sender policy framework records
    * `srv` for service records
* `txt` - (Optional, string) Used for text records.
* `service` - (`SRV` records only, required, string) The symbolic name of the desired service.
* `protocol` - (`SRV` records only, required, string) The protocol of the desired service. This is usually TCP or UDP.
* `port` - (`SRV` records only, required, integer) The TCP or UDP port on which the service will be found.
* `priority` - (`SRV` records only, required, integer) The priority of the target host. The lowest numerical value is given the highest priority. The default value is `0`.
* `weight` - (`SRV` records only, required, integer) A relative weight for records that have the same priority. The default value is `0`.
* `tags` - (Optional, array of strings) Tags associated with the DNS domain record instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The internal identifier of the domain resource record.



## ibm_firewall

Provides a firewall in IBM. One firewall protects one public VLAN and provides in-bound network packet filtering.

You can order or find firewalls in the IBM Cloud infrastructure customer portal by navigating to **Network > IP Management > VLANs** and clicking the **Gateway/Firewall** column.

For more information about how to configure a firewall, see the [docs](https://knowledgelayer.softlayer.com/procedure/configure-hardware-firewall-dedicated).

### Sample Terraform code

```hcl
resource "ibm_firewall" "testfw" {
  firewall_type = "HARDWARE_FIREWALL_DEDICATED"
  ha_enabled = false
  public_vlan_id = 12345678
  tags = [
     "collectd",
     "mesos-master"
   ]
}
```

### Input parameters

The following arguments are supported:

* `firewall_type` - (Optional, string) Specifies the type of firewall to create. Valid options are HARDWARE_FIREWALL_DEDICATED or FORTIGATE_SECURITY_APPLIANCE. Defaults to HARDWARE_FIREWALL_DEDICATED
* `ha_enabled` - (Required, boolean) Specifies whether the local load balancer needs to be HA-enabled.
* `public_vlan_id` - (Required, integer) The target public VLAN ID that you want the firewall to protect. You can find accepted values [here](https://cloud.ibm.com/classic/network/vlans). Click the desired VLAN and note the ID number in the resulting URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
* `tags` - (Optional, array of strings) Set tages on the firewall. Permitted characters include: A-Z, 0-9, whitespace, _ (underscore), - (hyphen), . (period), and : (colon). All other characters are removed.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VLAN.
* `location` - The location/datacenter of the created firewall device.
* `primary_ip:` - The public gateway IP address.
* `username:` - The username used to login into the device in case of Fortigate Appliances.
* `password:` - The password used to login into the device in case of Fortigate Appliances.


## ibm_multivlan_firewall

Provides an Multi-Vlan Firewall Resource.

For additional details, see the [IBM Cloud (SoftLayer) multivlan firewall Request docs](https://softlayer.github.io/reference/datatypes/SoftLayer_Container_Product_Order_Network_Protection_Firewall_Dedicated/)

### Sample Terraform code

In the following example, you can create a multi-vlan firewall:

```hcl
resource "ibm_multi_vlan_firewall" "firewall_first" {
	datacenter = "dal13"
	pod = "pod01"
	name = "Checkdelete1"
	firewall_type = "FortiGate Security Appliance"
	addon_configuration = ["FortiGate Security Appliance - Web Filtering Add-on (High Availability)","FortiGate Security Appliance - NGFW Add-on (High Availability)","FortiGate Security Appliance - AV Add-on (High Availability)"]
	}
```


### Input parameters

The following arguments are supported:

* `datacenter` - (Required, string) The data center in which the firewall appliance resides.
* `pod` - (Required, string) The pod in which the firewall resides
* `name` - (Required, string) The name of the firewall device
* `firewall_type` - (Required, string) The type of the firewall device. Allowed values are:- FortiGate Security Appliance,FortiGate Firewall Appliance HA Option
* `addon_configuration` - (Required, list) The list of addons that are allowed. Allowed values:- ["FortiGate Security Appliance - Web Filtering Add-on (High Availability)","FortiGate Security Appliance - NGFW Add-on (High Availability)","FortiGate Security Appliance - AV Add-on (High Availability)"] or ["FortiGate Security Appliance - Web Filtering Add-on","FortiGate Security Appliance - NGFW Add-on","FortiGate Security Appliance - AV Add-on"]

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the Multi-Vlan firewall
* `public_vlan_id` - The id of the Public Vlan for accessing this gateway
* `private_vlan_id` - The id of the Private Vlan for accessing this gateway
* `public_ip` - The public gateway IP address.
* `public_ipv6` - The public gateway IPv6 address.
* `private_ip` - The private gateway IP address.
* `username` - The username used to login into the device
* `password` - The password used to login into the device



## ibm_firewall_policy

Provides rules for firewall resources in IBM. One rule resource is allowed per firewall. However, a rule resource can contain multiple firewall rules within it.

For more details about how to configure a firewall, see the [docs](https://knowledgelayer.softlayer.com/procedure/configure-hardware-firewall-dedicated).  

**NOTE**: The target VLAN should have at least one subnet for rule configuration. To express any IP addresses externally, configure `src_ip_address` as `0.0.0.0` and `src_ip_cidr` as `0`. To express API IP addresses internally, configure `dst_ip_address` as `any` and `src_ip_cidr` as `32`.

When a rules resource is created, it cannot be deleted. IBM does not allow entire rule deletion.

Firewalls should have at least one rule. If Terraform destroys the rules resources, _permit from any to any with TCP, UDP, ICMP, GRE, PPTP, ESP, and HA_ rules will be configured.

### Sample Terraform code

```hcl
resource "ibm_firewall" "demofw" {
  ha_enabled = false
  public_vlan_id = 1234567
}

resource "ibm_firewall_policy" "rules" {
 firewall_id = "${ibm_firewall.demofw.id}"
 rules = {
      "action" = "permit"
      "src_ip_address"= "10.1.1.0"
      "src_ip_cidr"= 24
      "dst_ip_address"= "any"
      "dst_ip_cidr"= 32
      "dst_port_range_start"= 80
      "dst_port_range_end"= 80
      "notes"= "Permit from 10.1.1.0"
      "protocol"= "udp"
 }
  rules = {
       "action" = "deny"
       "src_ip_address"= "10.1.1.0"
       "src_ip_cidr"= 24
       "dst_ip_address"= "any"
       "dst_ip_cidr"= 32
       "dst_port_range_start"= 81
       "dst_port_range_end"= 81
       "notes"= "Permit from 10.1.1.0"
       "protocol"= "udp"
  }
}
```

### Input parameters

The following arguments are supported:

* `firewall_id` - (Required, integer) The device ID for the target hardware firewall.
* `rules` - (Required, array) The firewall rules. At least one rule is required.
* `rules.action` - (Required, string) Specifies whether traffic is allowed when rules are matched. Accepted values are `permit` or `deny`.
* `rules.src_ip_address` - (Required, string) Specifies either a specific IP address or the network address for a specific subnet.
* `rules.src_ip_cidr` - (Required, string) Specifies the standard CIDR notation for the selected source. `32` implements the rule for a single IP while, for example, `24` implements the rule for 256 IPs.
* `rules.dst_ip_address` - (Required, string) Accepted values are `any`, a specific IP address, or the network address for a specific subnet.
* `rules.dst_ip_cidr` - (Required, string) Specifies the standard CIDR notation for the selected destination.
* `rules.dst_port_range_start` - (Optional, string) The start of the range of ports for TCP and UDP. Accepted values are `1` - `65535`.
* `rules.dst_port_range_end` - (Optional, string) The end of the range of ports for TCP and UDP. Accepted values are `1` - `65535`.
* `rules.notes` - (Optional, string) Descriptive text about the rule.
* `rules.protocol` - (Required, string) The protocol for the rule. Accepted values are `tcp`,`udp`,`icmp`,`gre`,`pptp`,`ah`, or `esp`.
* `tags` - (Optional, array of strings) Tags associated with the firewall policy instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.



## ibm_hardware\_firewall\_shared

Provides a firewall in IBM. One firewall protects one public VLAN and provides in-bound network packet filtering. 



For more information about how to configure a firewall, see the [docs](https://knowledgelayer.softlayer.com/procedure/configure-hardware-firewall).

### Sample Terraform code

```hcl
resource "ibm_hardware_firewall_shared" "test_firewall" {
    firewall_type="100MBPS_HARDWARE_FIREWALL"
    hardware_instance_id="12345678"
}
```

### Input parameters

The following arguments are supported:

* `firewall_type` - (Required, string) Specifies whether it needs to be of particular speed. Firewall type is in between [10MBPS_HARDWARE_FIREWALL, 20MBPS_HARDWARE_FIREWALL,100MBPS_HARDWARE_FIREWALL, 1000MBPS_HARDWARE_FIREWALL]
* `virtual_instance_id` - (Optional, string) Specifies the id of particular guest on which firewall shared is to be deployed.**NOTE**: This is conflicting parameter with hardware_instance_id.
* `hardware_instance_id` - (Optional, string) Specifies the id of particular guest on which firewall shared is to be deployed.**NOTE**: This is conflicting parameter with virtual_instance_id.

### Output parameters

The following attributes are exported:

 * `id` - The unique identifier of the hardware firewall.



## ibm_lb

Provides a resource for local load balancers. This allows local load balancers to be created, updated, and deleted.

### Sample Terraform code

In the following example, you can create a local load balancer:

```hcl
resource "ibm_lb" "test_lb_local" {
  connections = 1500
  datacenter  = "tok02"
  ha_enabled  = false
  dedicated   = false

  //User can increase timeouts
  timeouts {
    create = "45m"
  }
}
```



The following arguments are supported:

* `connections` - (Required, integer) The number of connections for the local load balancer. Only incremental upgrade is supported . For downgrade, please open the softlayer support ticket.
* `datacenter` - (Required, string) The data center for the local load balancer.
* `ha_enabled` - (Required, boolean) Specifies whether the local load balancer must be HA-enabled.
* `security_certificate_id` - (Optional, integer) The ID of the security certificate associated with the local load balancer.
* `dedicated` - (Optional, boolean) Specifies whether the local load balancer must be dedicated. The default value is `false`.
* `ssl_offload` - (Optional, boolean) Specifies the local load balancer ssl offload. If `true` start SSL acceleration on all SSL virtual services (those with a type of HTTPS). This action should be taken only after configuring an SSL certificate for the virtual IP. If `false` stop SSL acceleration on all SSL virtual services (those with a type of HTTPS). The default value is `false`.
* `tags` - (Optional, array of strings) Tags associated with the local load balancer instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the local load balancer.
* `hostname` - The host name of the local load balancer.
* `ip_address` - The IP address of the local load balancer.
* `subnet_id` - The unique identifier of the subnet associated with the local load balancer.
* `ssl_enabled` - The status of whether the local load balancer provides SSL capability.

### Timeouts

ibm_subnet provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 30 minutes) Used for Creating Instance.





## ibm_lbaas

Provides a resource for a load balancer as a service. This allows a load balancer as a service to be created, updated, and deleted. Currently, only one subnet is supported.

Cloud load balancer creation takes 5 to 10 minutes. Destroy can take up to 30 minutes. Cloud Load Balancer does not support customisation of acceptable response codes. Only the range 2xx is considered healthy. Redirects in the range 3xx are considered unhealthy. 
 
### Sample Terraform code

```hcl

resource "ibm_lbaas" "lbaas" {
  name        = "terraformLB"
  description = "delete this"
  subnets     = [1511875]

  protocols = [{
    frontend_protocol     = "HTTPS"
    frontend_port         = 443
    backend_protocol      = "HTTP"
    backend_port          = 80
    load_balancing_method = "round_robin"
    tls_certificate_id    = 11670
  },
    {
      frontend_protocol     = "HTTP"
      frontend_port         = 80
      backend_protocol      = "HTTP"
      backend_port          = 80
      load_balancing_method = "round_robin"
    },
  ]
}


```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The load balancer's name.
* `description` - (Optional, string) A description of the load balancer.
* `type` - (Optional, string) Specify whether this load balancer is a public or internal facing load balancer. Accepted values are `PUBLIC` or `PRIVATE`. 
The default is 'PUBLIC'.
* `subnets` - (Required, array) The subnet where the load balancer will be provisioned. Only one subnet is supported.
* `protocols` - (Optional, array) A nested block describing the protocols assigned to load balancer. Nested `protocols` blocks have the following structure:
  * `frontend_protocol` - (Required, string) The frontend protocol. Accepted values are 'TCP', 'HTTP', and 'HTTPS'.
  * `frontend_port` - (Required, integer) The frontend protocol port number. The port number must be in the range of 1 - 65535.
  * `backend_protocol` - (Required, string) The backend protocol. Accepted values are 'TCP', 'HTTP', and 'HTTPS'.
  * `backend_port` - (Required, integer) The backend protocol port number. The port number must be in the range of 1 - 65535.
  * `load_balancing_method` - (Optional, string) The load balancing algorithm. Accepted values are 'round_robin', 'weighted_round_robin', and 'least_connection'. The default is 'round_robin'.
  * `session_stickiness` - (Optional, string) The SOURCE_IP for session stickiness.
  * `max_conn` - (Optional, integer) The maximum number of connections the listener can accept. The number must be 1 - 64000.
  * `tls_certificate_id` - (Optional, integer) The ID of the SSL/TLS certificate being used for a protocol. This ID should be specified when `frontend protocol` has a value of `HTTPS`.
* `ssl_ciphers` - (Optional, list) The comma seperated list of SSL Ciphers. You can find list of supported cipheres [ssl_offload](https://cloud.ibm.com/docs/infrastructure/loadbalancer-service?topic=loadbalancer-service-ssl-offload-with-ibm-cloud-load-balancer).
* `use_system_public_ip_pool` - (Optional, bool) Applicable for public load balancer only. It specifies whether the public IP addresses are allocated from system public IP pool or public subnet from the account ordering the load balancer. Default Value is `true`.
* `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the lbaas instance to become available before declaring it as created. It is also the same amount of time waited for deletion to finish. The default value is `90`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the created policy.
* `datacenter` - The datacenter where the load balancer is provisioned. This is based on the subnet chosen while creating load-balancer.
* `status` - Specifies the operation status of the load balancer as `ONLINE` or `OFFLINE`.
* `vip` - The virtual IP address of the load balancer.
* `protocol_id` - The UUID of a load balancer protocol.
* `health_monitors` - A nested block describing the health_monitors assigned to the load balancer. Nested `health_monitors` blocks have the following structure:
  * `protocol` - Backends protocol
  * `port` - Backends port
  * `interval` - Interval in seconds to perform 
  * `max_retries` - Maximum retries
  * `timeout` - Health check methods timeout in 
  * `url_path` - If monitor is "HTTP", this specifies URL path
  * `monitor_id` - Health Monitor UUID



## ibm_lbaas\_health\_monitor

Provides a resource for the health monitors of IBM Lbaas. This allows to update a health monitor configuration of the load balancer. Health monitors are created and deleted by the creation and deletion of lbaas protocols.
 
### Sample Terraform code

```hcl

resource "ibm_lbaas" "lbaas" {
  name        = "terraformLB"
  description = "delete this"
  subnets     = [1511875]

  protocols = [{
    frontend_protocol     = "HTTPS"
    frontend_port         = 443
    backend_protocol      = "HTTP"
    backend_port          = 80
    load_balancing_method = "round_robin"
    tls_certificate_id    = 11670
  },
    {
      frontend_protocol     = "HTTP"
      frontend_port         = 80
      backend_protocol      = "HTTP"
      backend_port          = 80
      load_balancing_method = "round_robin"
    },
  ]
}

resource "ibm_lbaas_health_monitor" "lbaas_hm" {
  protocol = "${ibm_lbaas.lbaas.health_monitors.0.protocol}"
  port = "${ibm_lbaas.lbaas.health_monitors.0.port}"
  timeout = 3
  interval = 5
  max_retries = 6
  url_path = "/"
  lbaas_id = "${ibm_lbaas.lbaas.id}"
  monitor_id = "${ibm_lbaas.lbaas.health_monitors.0.monitor_id}"
}

```

### Input parameters

The following arguments are supported:

* `monitor_id` - (Required,string) Health Monitor unique identifier. The monitor id can be imported from either the ibm_lbaas resource or datasource.
ex: ibm_lbaas.lbaas.health_monitors.X.monitor_id or data.ibm_lbaas.lbaas.health_monitors.X.monitor_id
* `lbaas_id` - (Required,string) Lbaas unique identifier
* `protocol` - (Required, string) Backends protocol
* `port` - (Required, int) Backends port
* `interval` - (Optional,int) Interval in seconds to perform 
* `max_retries` - (Optional,int) Maximum retries
* `timeout` - (Optional,int) Health check methods timeout in 
* `url_path` - (Optional,string) If monitor is "HTTP", this specifies URL path

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the lbaas health monitor resource. The id is composed of \<lbaas_id\>/\<monitor_id/>

### Import

ibm_lbaas_health_monitor can be imported using lbaas_id and monitor_id, eg

```
$ terraform import ibm_lbaas_health_monitor.example 988-454f-45vf-454542/d343f-f44r-wer3-fe3


## ibm_lbaas\_server\_instance\_attachment

Provides a resource for attaching the server instance to IBM cloud load balancer. This allows to attach, detach and update server instances as loadbalancer members to IBM cloud load balancer. A `depends_on` statement is required for the associated load balancer to ensure that attach and detach only occur after and before load balancer creation and deletion. If the `depends_on` is ommited intemitent attach failures will occur on creation and load balancer deletion will fail. Typically when apply or destroy is rerun the operation will be successful. 


 
### Sample Terraform code

```hcl

resource "ibm_compute_vm_instance" "vm_instances" {
  count = "2"
  ....
}

resource "ibm_lbaas" "lbaas" {
  name        = "terraformLB"
  description = "delete this"
  subnets     = [1511875]

  protocols = [{
    frontend_protocol     = "HTTPS"
    frontend_port         = 443
    backend_protocol      = "HTTP"
    backend_port          = 80
    load_balancing_method = "round_robin"
    tls_certificate_id    = 11670
  },
    {
      frontend_protocol     = "HTTP"
      frontend_port         = 80
      backend_protocol      = "HTTP"
      backend_port          = 80
      load_balancing_method = "round_robin"
    },
  ]
}

resource "ibm_lbaas_server_instance_attachment" "lbaas_member" {
  count				 = 2
  private_ip_address = "${element(ibm_compute_vm_instance.vm_instances.*.ipv4_address_private,count.index)}"
  weight             = 40
  lbaas_id           = "${ibm_lbaas.lbaas.id}"
  depends_on         = ["ibm_lbaas.lbaas.id"]
}

```

### Input parameters

The following arguments are supported:

* `private_ip_address` - (Required, string) The private IP address of a load balancer member.
* `weight` - (Optional, integer) The weight of a load balancer member.
* `lbaas_id` - (Required, string) The UUID of a load balancer.
* `depends_on` - (Required, string) the UUID of a load balancer 

### Output parameters

The following attributes are exported:

* `uuid` - The unique identifier of the load balancer member.



## ibm_lb_service

Provides a resource for local load balancer services. This allows local load balancer services to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_Application_Delivery_Controller_LoadBalancer_Service).

### Sample Terraform code

In the following example, you can create a local load balancer service:

```hcl
resource "ibm_lb_service" "test_lb_local_service" {
    port = 80
    enabled = true
    service_group_id = "${ibm_lb_service_group.test_service_group.service_group_id}"
    weight = 1
    health_check_type = "DNS"
    ip_address_id = "${ibm_compute_vm_instance.test_server.ip_address_id}"
}

```

### Input parameters

The following arguments are supported:

* `service_group_id` - (Required, integer) The ID of the local load balancer service group.
* `ip_address_id` - (Required, integer) The ID of the virtual server.
* `port` - (Required, integer) The port for the local load balancer service.
* `enabled` - (Required, boolean) Specifies whether you want to enable the load balancer service. The default value is `false`.
* `health_check_type` - (Required, string) The health check type for the load balancer service.
* `weight` - (Required, integer) The weight for the load balancer service.
* `tags` - (Optional, array of strings) Tags associated with the local load balancer service instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.



## ibm_lb_service_group

Provides a resource for local load balancer service groups. This allows local load balancer service groups to be created, updated, and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_Application_Delivery_Controller_LoadBalancer_Service_Group).

### Sample Terraform code

In the following example, you can create a local load balancer service group:

```hcl
resource "ibm_lb_service_group" "test_service_group" {
    port = 82
    routing_method = "CONSISTENT_HASH_IP"
    routing_type = "HTTP"
    load_balancer_id = "${ibm_lb.test_lb_local.id}"
    allocation = 100
}
```

### Input parameters

The following arguments are supported:

* `load_balancer_id` - (Required, integer) The ID of the local load balancer.
* `allocation` - (Required, integer) The connection allocation for the load balancer service group.
* `port` - (Required, integer) The port for the local load balancer service group.
* `routing_method` - (Required, string) The routing method for the load balancer group. For example, `CONSISTENT_HASH_IP`.
* `routing_type` - (Required, string) The routing type for the group.
* `timeout` - (Optional, int) The timeout value for connections from remote clients to the load balancer. Timeout values are only valid for HTTP service groups. 
* `tags` - (Optional, array of strings) Tags associated with the local load balancer service group instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `virtual_server_id` - The unique identifier of the virtual server.
* `service_group_id` - The unique identifier of the load balancer service group.



## ibm_lb_vpx

Provides a resource for VPX load balancers. This allows VPX load balancers to be created, updated, and deleted.  

**NOTE**: IBM VPX load balancers consist of Citrix NetScaler VPX devices (virtual), which are currently priced on a per-month basis. Use caution when creating the resource because the cost for an entire month is incurred immediately upon creation. For more information about pricing, see the [network appliance docs](http://www.softlayer.com/network-appliances). Under the Citrix log, click **See more pricing** for a current price matrix.

You can also use the following REST URL to get a listing of VPX choices along with version numbers, speed, and plan type:

```
https://<userName>:<apiKey>@api.softlayer.com/rest/v3/SoftLayer_Product_Package/192/getItems.json?objectMask=id;capacity;description;units;keyName;prices.id;prices.categories.id;prices.categories.name
```

### Sample Terraform code

Review the [IBM Cloud Classic Infrastructure (SoftLayer) docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_Application_Delivery_Controller) for more information.

```hcl
resource "ibm_lb_vpx" "test_vpx" {
    datacenter = "dal06"
    speed = 10
    version = "10.1"
    plan = "Standard"
    ip_count = 2
    public_vlan_id = 1251234
    private_vlan_id = 1540786
    public_subnet = "23.246.226.248/29"
    private_subnet = "10.107.180.0/26"
}
```

### Input parameters

The following arguments are supported:

* `datacenter` - (Required, string) The data center in which you want to provision the VPX load balancer. You can find accepted values in the [data center docs](http://www.softlayer.com/data-centers).
* `speed` - (Required, integer) The speed, expressed in Mbps. Accepted values are `10`, `200`, and `1000`.
* `version` - (Required, string) The VPX load balancer version. Accepted values are `10.1`, `10.5`, `11.0`, `11.1` and `12.1`.
* `plan` - (Required, string) The VPX load balancer plan. Accepted values are `Standard` and `Platinum`.
* `ip_count` - (Required, integer) The number of static public IP addresses assigned to the VPX load balancer. Accepted values are `1`,`2`, `4`, `8`, and `16`.
* `public_vlan_id` - (Optional, integer) The public VLAN ID that is used for the public network interface of the VPX load balancer. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans) by clicking the desired VLAN and noting the ID in the resulting URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
* `private_vlan_id` - (Optional, integer) The private VLAN ID that is used for the private network interface of the VPX load balancer. You can find accepted values in the [VLAN docs](https://cloud.ibm.com/classic/network/vlans) by clicking the desired VLAN and noting the ID in the resulting URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
* `public_subnet` - (Optional, string) The public subnet that is used for the public network interface of the VPX load balancer. Accepted values are primary public networks. You can find accepted values in the [subnet docs](https://cloud.ibm.com/classic/network/subnets).
* `private_subnet` - (Optional, string) Public subnet that is used for the private network interface of the VPX load balancer. Accepted values are primary private networks. You can find accepted values in the [subnet docs](https://cloud.ibm.com/classic/network/subnets).
* `tags` - (Optional, array of strings) Tags associated with the VPX load balancer instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The internal identifier of a VPX load balancer
* `name` - The internal name of a VPX load balancer.
* `vip_pool` - A list of virtual IP addresses for the VPX load balancer.
* `management_ip_address` - The private address of the VPX UI.



## ibm_lb_vpx_ha

Configure a high availability (HA) pair with two NetScaler VPX devices. The two NetScaler VPXs must be version 10.5 and located in the same subnet. A primary NetScaler VPX provides load balancing services in active mode, and a secondary NetScaler VPX provides load balancing services when the primary NetScaler VPX fails. For additional details, refer to the  [Citrix support docs](https://support.citrix.com/article/CTX116748) and the [KnowledgeLayer NetScaler docs](http://knowledgelayer.softlayer.com/articles/netscaler-vpx-10-high-availability-setup).

**NOTE**: This resource only supports NetScaler VPX 10.5. The [NITRO API](https://docs.citrix.com/en-us/netscaler/11/nitro-api.html) is used to configure HA. Terraform can only access the NITRO API in the IBM Cloud Classic Infrastructure (SoftLayer) private network, so connect to the private network when running Terraform. You can also use the [SSL VPN](http://www.softlayer.com/VPN-Access) to access a private network connection.

The two NetScaler VPXs use the same password in HA mode. When you create this resource, Terraform changes the password of the secondary NetScaler VPX to the password of the primary NetScaler VPX. When you destroy this resource, Terraform restores the original password of the secondary NetScaler VPX.

### Sample Terraform code

```hcl

## Create a primary NetScaler VPX
resource "ibm_lb_vpx" "test_pri" {
    datacenter = "lon02"
    speed = 10
    version = "10.5"
    plan = "Standard"
    ip_count = 2
}


## Create a secondary NetScaler VPX in the same subnets
resource "ibm_lb_vpx" "test_sec" {
    datacenter = "lon02"
    speed = 10
    version = "10.5"
    plan = "Standard"
    ip_count = 2
    public_vlan_id = "${ibm_lb_vpx.test_pri.public_vlan_id}"
    private_vlan_id = "${ibm_lb_vpx.test_pri.private_vlan_id}"
    public_subnet = "${ibm_lb_vpx.test_pri.public_subnet}"
    private_subnet = "${ibm_lb_vpx.test_pri.private_subnet}"
}


## Configure high availability with the primary and secondary NetScaler VPXs
resource "ibm_lb_vpx_ha" "test_ha" {
    primary_id = "${ibm_lb_vpx.test_pri.id}"
    secondary_id = "${ibm_lb_vpx.test_sec.id}"
    stay_secondary = false
}
```

### Input parameters

The following arguments are supported:

* `primary_id` - (Required, string) The ID of the primary NetScaler VPX.
* `secondary_id` - (Required, string) The ID of the secondary NetScaler VPX.
* `stay_secondary` - (Optional, boolean) Specifies whether the secondary NetScaler VPX will  take over the service. Set this argument to `true` to prevent the secondary NetScaler VPX from taking over the service even if the primary NetScaler VPX fails. For additional details, see the [Citrix NetScaler docs](https://docs.citrix.com/en-us/netscaler/10-5/ns-system-wrapper-10-con/ns-nw-ha-intro-wrppr-con/ns-nw-ha-frcng-scndry-nd-sty-scndry-tsk.html) and the [Citrix support docs](https://support.citrix.com/article/CTX116748). The default value is `false`.
* `tags` - (Optional, array of strings) Tags associated with the high availability NetScale VPX pair instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the high availability NetScale VPX pair.



## ibm_lb_vpx_service

Provides a resource for VPX load balancer services. This allows VPX load balancer services to be created, updated, and deleted. For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_LoadBalancer_Service).  

**NOTE**: If you use NetScaler VPX 10.5, Terraform uses NetScaler's [NITRO REST API](https://docs.citrix.com/en-us/netscaler/11/nitro-api.html) to manage the resource.  Terraform can only access the NITRO API in the IBM Cloud Classic Infrastructure (SoftLayer) private network, so connect to the private network when running Terraform. You can also use the [SSL VPN](http://www.softlayer.com/VPN-Access) to access a private network connection.

### Sample Terraform code

In the following example, you can create a VPX load balancer:

```hcl
resource "ibm_lb_vpx_service" "test_service" {
  name = "test_load_balancer_service"
  vip_id = "${ibm_lb_vpx_vip.testacc_vip.id}"
  destination_ip_address = "${ibm_compute_vm_instance.test_server.ipv4_address}"
  destination_port = 80
  weight = 55
  connection_limit = 5000
  health_check = "HTTP"
  usip = "NO"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The ID of the VPX load balancer service.
* `vip_id` - (Required, string) The ID of the VPX load balancer virtual IP address to which the service is assigned.
* `destination_ip_address` - (Required, string) The IP address of the server to which traffic directs. If you use NetScaler VPX 10.1, you must indicate a public IP address in an IBM Cloud Classic Infrastructure (SoftLayer) account. If you use NetScaler VPX 10.5, you can use any IP address.
* `destination_port` - (Required, integer) The destination port of the server to which traffic directs.
* `weight` - (Required, integer) The percentage of the total connection limit allocated to the load balancer between all your services. See the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_LoadBalancer_Service) for details.  
    **NOTE**: If you use NetScaler VPX 10.5, the weight value is ignored.
* `connection_limit` - (Required, integer) The connection limit for this service. Acceptable values are `0` - `4294967294`. See the [Citrix NetScaler docs](https://docs.citrix.com/en-us/netscaler/11/reference/netscaler-command-reference/basic/service.html) for details.
* `health_check` - (Required, string) The health check type. See the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_LoadBalancer_Service) for details.
* `usip` - (Optional, string) Whether the service reports the source IP of the client to the service being load balanced. Acceptable values are `YES` or `NO`. The default value is `NO`. See the [Citrix NetScaler docs](https://developer-docs.citrix.com/projects/netscaler-nitro-api/en/11.0/configuration/basic/service/service) for more details.  
    **NOTE**: This argument is only available for VPX 10.5.
* `tags` - (Optional, array of strings) Tags associated with the VPX load balancer service instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VPX load balancer service.



## ibm_lb_vpx_vip

Provides a resource for VPX load balancer virtual IP addresses. This allows VPX load balancer virtual IP addresses to be created, updated, and deleted.  

**NOTE**: If you use NetScaler VPX 10.5, Terraform uses NetScaler's [NITRO REST API](https://docs.citrix.com/en-us/netscaler/11/nitro-api.html) to manage the resource.  Terraform can only access the NITRO API in the IBM Cloud Classic Infrastructure (SoftLayer) private network, so connect to the private network when running Terraform. You can also use the [SSL VPN](http://www.softlayer.com/VPN-Access) to access a private network connection.

### Sample Terraform code

The following example configuration supports NetScaler VPX 10.1 and 10.5:

```hcl
resource "ibm_lb_vpx_vip" "testacc_vip" {
    name = "test_load_balancer_vip"
    nad_controller_id = 1234567
    load_balancing_method = "lc"
    source_port = 80
    virtual_ip_address = "211.233.12.12"
    type = "HTTP"
}
```

The following example configuration supports only NetScaler VPX 10.5. Additional options for the `load_balancing_method` and `persistence` arguments are shown. A private IP address can be used for the `virtual_ip_address` argument.

```hcl
resource "ibm_lb_vpx_vip" "testacc_vip" {
    name = "test_load_balancer_vip"
    nad_controller_id = "1234567"
    load_balancing_method = "DESTINATIONIPHASH"
    persistence = "SOURCEIP"
    source_port = 80
    virtual_ip_address = "10.10.2.2"
    type = "HTTP"
}
```

NetScaler VPX 10.5 also supports SSL offload. If you set the `type` argument to `SSL` and configure the `security_certificate_id` argument, then the `virtual_ip_address` argument provides the `HTTPS` protocol. The following example shows an SSL-offload configuration:

```hcl

## Create a NetScaler VPX 10.5
resource "ibm_lb_vpx" "test" {
    datacenter = "lon02"
    speed = 10
    version = "10.5"
    plan = "Standard"
    ip_count = 2
}

resource "ibm_lb_vpx_vip" "test_vip1" {
    name = "test_vip1"
    nad_controller_id = "${ibm_lb_vpx.test.id}"
    load_balancing_method = "rr"
    source_port = 443

## SSL type provides SSL offload
    type = "SSL"
    virtual_ip_address = "${ibm_lb_vpx.test.vip_pool[0]}"

## Use a security certificate in the SoftLayer portal
    security_certificate_id = 80347
}

resource "ibm_lb_vpx_service" "testacc_service1" {
  name = "test_load_balancer_service1"
  vip_id = "${ibm_lb_vpx_vip.test_vip1.id}"

## 10.6.218.166 should provides HTTP service with port 80
  destination_ip_address = "10.66.218.166"
  destination_port = 80
  weight = 100
  connection_limit = 4294967294
  health_check = "ICMP"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The ID of the VPX load balancer virtual IP address.
* `nad_controller_id` - (Required, integer) The ID of the VPX load balancer that the virtual IP address is assigned to.
* `load_balancing_method` - (Required, string) See the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_LoadBalancer_VirtualIpAddress) for available methods. If you use NetScaler VPX 10.5, see the [Citrix docs](https://docs.citrix.com/en-us/netscaler/10-5/ns-tmg-wrapper-10-con/ns-lb-wrapper-con-10/ns-lb-customizing-lbalgorithms-wrapper-con.html) for additional methods that you can use.
* `persistence` - (Optional, string) Applies to NetScaler VPX 10.5 only. See the available persistence types in the [Citrix docs](https://docs.citrix.com/en-us/netscaler/10-5/ns-tmg-wrapper-10-con/ns-lb-wrapper-con-10/ns-lb-persistence-wrapper-con/ns-lb-persistence-about-con.html).  
* `virtual_ip_address` - (Required, string) The public IP address for the VPX load balancer virtual IP.
* `source_port` - (Required, integer) The source port for the VPX load balancer virtual IP address.
* `type` - (Required, string) The connection type for the VPX load balancer virtual IP address. Accepted values are `HTTP`, `FTP`, `TCP`, `UDP`, `DNS`, and `SSL`. If you set the type to `SSL`, then `security_certificate_id` provides certification for SSL offload services.
* `security_certificate_id` - (Optional, integer) Applies to NetScaler VPX 10.5 only. The ID of a security certificate you want to use. This argument provides security certification for SSL offload services. For additional information, see the  [ibm_compute_ssl_certificate resource](compute_ssl_certificate.html).
* `tags` - (Optional, array of strings) Tags associated with the VPX load balancer virtual IP instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VPX load balancer virtual IP.



## ibm_network_gateway

Provides a resource for an IBM Cloud network gateway appliance. This resource allows a network gateway to be created, updated, and deleted.  

A network gateway can be created in standalone mode and HA mode with both members, with either the same or different configurations.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway).

For more information about getting started, see the [IBM Virtual Router Appliance docs](https://cloud.ibm.com/docs/infrastructure/virtual-router-appliance/getting-started.html#getting-started).

### Sample Terraform code

#### Standalone configuration

```hcl
resource "ibm_network_gateway" "gateway" {
  name = "my-gateway"

  members = [{
    hostname             = "host-name"
    domain               = "ibm.com"
    datacenter           = "ams01"
    network_speed        = 100
    private_network_only = false
    tcp_monitoring       = true
    process_key_name     = "INTEL_SINGLE_XEON_1270_3_50"
    os_key_name          = "OS_VYATTA_5600_5_X_UP_TO_1GBPS_SUBSCRIPTION_EDITION_64_BIT"
    redundant_network    = false
    disk_key_names       = ["HARD_DRIVE_2_00TB_SATA_II"]
    public_bandwidth     = 20000
    memory               = 8
    tags                 = ["gateway tags 1", "terraform test tags 1"]
    notes                = "gateway notes 1"
    ipv6_enabled         = true
  },
  ]
}
```
#### HA configuration

```hcl
resource "ibm_network_gateway" "gateway" {
  name = "my-ha-gateway"

  members = [{
    hostname             = "host-name-1"
    domain               = "ibm.com"
    datacenter           = "ams01"
    network_speed        = 100
    private_network_only = false
    tcp_monitoring       = true
    process_key_name     = "INTEL_SINGLE_XEON_1270_3_50"
    os_key_name          = "OS_VYATTA_5600_5_X_UP_TO_1GBPS_SUBSCRIPTION_EDITION_64_BIT"
    redundant_network    = false
    disk_key_names       = ["HARD_DRIVE_2_00TB_SATA_II"]
    public_bandwidth     = 20000
    memory               = 8
    tags                 = ["gateway tags", "terraform test tags 1"]
    notes                = "gateway notes"
    ipv6_enabled         = true
  },
    {
      hostname             = "host-name-2"
      domain               = "ibm.com"
      datacenter           = "ams01"
      network_speed        = 100
      private_network_only = false
      tcp_monitoring       = true
      process_key_name     = "INTEL_SINGLE_XEON_1270_3_50"
      os_key_name          = "OS_VYATTA_5600_5_X_UP_TO_1GBPS_SUBSCRIPTION_EDITION_64_BIT"
      redundant_network    = false
      disk_key_names       = ["HARD_DRIVE_2_00TB_SATA_II"]
      public_bandwidth     = 20000
      memory               = 8
      tags                 = ["gateway tags 1", "terraform test tags 1"]
      notes                = "my ha mode gateway"
      ipv6_enabled         = true
    },
  ]
}
```


### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the gateway.
* `ssh_key_ids` - (Optional, list) The SSH key IDs to install on the gateway when the gateway gets created.
* `post_install_script_uri` - (Optional, string) The URI of the script to be downloaded and executed after the gateway installation is complete. Default value: `nil`. 
* `members` - (Required, list) A nested block describing the hardware members of this network gateway.
Nested `members` blocks have the following structure:
  * `hostname` - (Optional, string) Hostname of the member.
  * `domain` - (Required, string) The domain of the member
  * `notes` - (Optional, string) Descriptive text of up to 1000 characters about the member.
  * `datacenter` - (Required, string) The data center in which you want to provision the member.
  * `network_speed` - (Optional, integer) The connection speed (in Mbps) for the member network components. Default value: `100`.
  * `redundant_power_supply` - (Optional, boolean) When the value is `true`, an additional power supply is provided. Default value: `false`
  * `tcp_monitoring` - (Optional, boolean) Whether to enable TCP monitoring for the member. Default value: `false`.
  * `process_key_name` - (Optional, string) The process key name for the member. Default value:  `INTEL_SINGLE_XEON_1270_3_40_2`. Refer to the same attribute on the `ibm_compute_bare_metal` resource.
  * `package_key_name` - (Optional, string) The key name for the network gateway package. You can find available package key names in the [Softlayer API](https://api.softlayer.com/rest/v3/SoftLayer_Product_Package/getAllObjects?objectFilter={"type":{"keyName":{"operation":"BARE_METAL_GATEWAY"}}}), using your API key as the password. Default value: `NETWORK_GATEWAY_APPLIANCE`. The default will allow to order Single Processor Multi-Core Servers. Use `2U_NETWORK_GATEWAY_APPLIANCE_1O_GBPS` for ordering Dual Processor Multi-Core Servers.
  * `os_key_name` - (Optional, string) The os key name for member. Default value:  `OS_VYATTA_5600_5_X_UP_TO_1GBPS_SUBSCRIPTION_EDITION_64_BIT`. Refer to the same attribute on the `ibm_compute_bare_metal` resource.
  * `redundant_network` - (Optional, boolean) When the value is `true`, two physical network interfaces are provided with a bonding configuration. Default value: `false`. 
  * `unbonded_network` - (Optional, boolean) When the value is `true`, two physical network interfaces are provided without a bonding configuration. Default value: `false`. 

  * `tags` - (Optional, set) Tags associated with the VM instance. Permitted characters include: A-Z, 0-9, whitespace, _ (underscore), - (hyphen), . (period), and : (colon). All other characters are removed.
  * `public_bandwidth` - (Optional, integer) Allowed public network traffic (in GB) per month. Default value: `20000`.
  * `memory` - (Required, integer) The amount of memory, expressed in megabytes, that you want to allocate.
  * `storage_groups` - (Optional, list) A nested block describing the storage group for the member of the network gateway. Nested `storage_groups` blocks have the following structure:
      * `array_type_id` - (Required, integer) The ID of the array type.
      * `hard_drives` - (Required, list) The list of hard drive associated with the gateway member.
      * `array_size` - (Optional, integer) The size of the array.
      * `partition_template_id` - (Optional, integer) The partition template ID for the member.
  * `ssh_key_ids` - (Optional, list) The SSH key IDs to install on the member.
  * `post_install_script_uri` - (Optional, string) The URI of the script to be downloaded and executed on the member. Default value: `nil`.
  * `user_metadata` - (Optional, string) Arbitrary data to be made available to the member.
  * `disk_key_names` - (Optional, list) Provide the disk key name. Refer to the same attribute in the `ibm_compute_bare_metal` resource.
  * `public_vlan_id` - (Optional, integer) ID of the public VLAN.
  * `private_vlan_id` - (Optional, integer) ID of the private VLAN.  
    **NOTE**: If there are two members in this gateway, then both should have same value for `public_vlan_id` and `private_vlan_id`.
    
  * `ipv6_enabled` - (Optional, boolean) Whether to enable IPv6. Default value: `true`.
  * `private_network_only` - (Optional, boolean) Whether to enable a private network only. Default value: `false`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the network gateway.
* `public_ipv4_address` - The public IP address of the network gateway.
* `private_ipv4_address` - The private IP address of the network gateway.
* `private_ip_address_id` - The private IP address ID of the network gateway.
* `private_vlan_id` - The private VLAN ID of the network gateway.
* `public_ip_address_id` - The public IP address ID of the network gateway.
* `public_ipv6_address_id` - The public IPv6 address ID for the network gateway.
* `public_vlan_id` - The public VLAN ID for the network gateway.
* `status` - Status of the network gateway.
* `associated_vlans` - A nested block describing the associated VLANs for the member of the network gateway. Nested `associated_vlans` blocks export the following attributes:
  * `vlan_id` - The VLAN ID.
  * `network_vlan_id` - The ID of the VLAN that is associated.
  * `bypass` -  Indicates if the VLAN is in bypass or routed mode.
* `members` - A nested block describing the hardware members of this network gateway.
Nested `members` blocks export the following attributes:
  * `member_id` -  ID of the member.
  * `public_ipv4_address` - Public IPv4 address associated with the member.
  * `private_ipv4_address` - Private IPv4 address associated with the member.
  * `ipv6_address` -  IPv6 address associated with the member.



## ibm_network_gateway_vlan_association

Provide a resource to associate a VLAN with a network gateway. The VLANs can be disassociated or updated later to be bypassed or routed.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan).

For more information about getting started, see the [IBM Virtual Router Appliance docs](https://cloud.ibm.com/docs/infrastructure/virtual-router-appliance/getting-started.html#getting-started).

### Sample Terraform code

```hcl
resource "ibm_network_gateway" "gateway" {
  name = "gateway"

  members {
    hostname             = "my-virtual-router"
    domain               = "terraformuat1.ibm.com"
    datacenter           = "ams01"
    network_speed        = 100
    private_network_only = false
    tcp_monitoring       = true
    process_key_name     = "INTEL_SINGLE_XEON_1270_3_40_2"
    os_key_name          = "OS_VYATTA_5600_5_X_UP_TO_1GBPS_SUBSCRIPTION_EDITION_64_BIT"
    redundant_network    = false
    disk_key_names       = ["HARD_DRIVE_2_00TB_SATA_II"]
    public_bandwidth     = 20000
    memory               = 4
    ipv6_enabled         = true
  }
}

resource "ibm_network_gateway_vlan_association" "gateway_vlan_association" {
  gateway_id      = "${ibm_network_gateway.gateway.id}"
  network_vlan_id = 645086
}

```

### Input parameters

The following arguments are supported:

* `gateway_id` - (Required, integer) The ID of the network gateway.
* `network_vlan_id` - (Required, integer) The ID of the network VLAN to associate with the network gateway.
* `bypass` - (Optional, boolean) Indicates if the VLAN should be in bypass or routed mode. Default value: `true`. 


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the gateway/VLAN association.


## ibm_network_interface_sg_attachment

Provide a resource to attach security group to a network interface. This allows attachments to be created and deleted.

For additional details, see the [IBM Cloud Classic Infrastructure  (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Virtual_Network_SecurityGroup_NetworkComponentBinding).

### Sample Terraform code

```
data "ibm_security_group" "allowssh" {
  name = "allow_ssh"
}

resource "ibm_compute_vm_instance" "vsi"{
   ....
}
resource "ibm_network_interface_sg_attachment" "sg1" {
    security_group_id = "${data.ibm_security_group.allowssh.id}"
    network_interface_id = "${ibm_compute_vm_instance.vsi.public_interface_id}"
    //User can increase timeouts 
    timeouts {
      create = "15m"
    }
}
```



The following arguments are supported:

* `security_group_id` - (Required, int) The ID of the security group.
* `network_interface_id` - (Required, int) The ID of the network interface to which the security group must be applied.
* `soft_reboot` - (Optional, boolean) Default `true`. If true and if a reboot is required to apply the attachment then VSI on which the network interface lies would be soft rebooted. If false then no reboot is perfomed.

**Note**: A reboot is required if this is first time any security group is applied to this network interface and it has never been rebooted since then.

### Timeouts

ibm_network_interface_sg_attachment provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 15 minutes) Used for Creating Instance.




## ibm_network_public_ip

Provides a public IP resource to route between servers. This allows public IPs to be created, updated, and deleted. Public IPs are not restricted to routing within the same data center.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Subnet_IpAddress_Global) and [public IP address overview](https://knowledgelayer.softlayer.com/learning/global-ip-addresses).

### Sample Terraform code

```hcl
resource "ibm_network_public_ip" "test_public_ip " {
    routes_to = "119.81.82.163"
    notes     = "public ip notes"
}
```



The following arguments are supported:

* `routes_to` - (Required, string) The destination IP address that the public IP routes traffic through. The destination IP address can be a public IP address of IBM resources in the same account, such as a public IP address of a VM or public virtual IP addresses of NetScaler VPXs.
* `notes` - (Optional, string) Descriptive text to associate with the public IP instance.
* `tags` - (Optional, array of strings) Tags associated with the public IP instance.  

  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the public IP.
* `ip_address` - The address of the public IP.

### Timeouts
ibm_network_public_ip provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 30 minutes) Used for Creating Instance.




## ibm_network_vlan

Provides a VLAN resource. This allows public and private VLANs to be created, updated, and cancelled.

If you have a default SoftLayer account, you do not have permission to create a VLAN using the SoftLayer API. If you want to create a VLAN with Terraform, you must get the required permissions in advance. Contact a SoftLayer sales person or open a ticket.

You can manage existing VLANs with Terraform by using the `terraform import` command. The command requires the VLAN IDs, which you can find in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic/network/vlans). After the VLAN IDs are imported inot Softlayer, the IDs provide useful information such as subnets and child resource counts. When you run the `terraform destroy` command, the billing item for the VLAN is deleted. The VLAN remains in SoftLayer until you delete remaining resources on the VLAN, such as virtual guests, secondary subnets, and firewalls.

For additional details please refer to the [SoftLayer API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_Vlan).

### Sample Terraform code

In the following example, you can create a VLAN:

```hcl
resource "ibm_network_vlan" "test_vlan" {
   name = "test_vlan"
   datacenter = "dal06"
   type = "PUBLIC"
   router_hostname = "fcr01a.dal06"
   tags = [
     "collectd",
     "mesos-master"
   ]
}

```

### Input parameters

The following arguments are supported:

* `datacenter` - (Required, string) The data center in which the VLAN resides.
* `type` - (Required, string) The type of VLAN. Accepted values are `PRIVATE` and `PUBLIC`.
* `subnet_size` - (Removed, integer) The size of the primary subnet for the VLAN. Accepted values are `8`, `16`, `32`, and `64`. This field has been removed.
* `name` - (Optional, string) The name of the VLAN. Maximum length of 20 characters.
* `router_hostname` - (Optional, string) The hostname of the primary router associated with the VLAN.
* `tags` - (Optional, array of strings) Tags associated with the VLAN. Permitted characters include: A-Z, 0-9, whitespace, _ (underscore), - (hyphen), . (period), and : (colon). All other characters are removed.

#

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:
* `delete` - (Defaults to 10 mins) Used when deleting the VLAN. There might be some resources(like Virtual Guests) on the VLAN. The VLAN delete request is issued once there are no Virtual Guests on the VLAN.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VLAN.
* `vlan_number` - The VLAN number as recorded within the SoftLayer network. This attribute is configured directly on SoftLayer's networking equipment.
* `softlayer_managed` - Whether or not Softlayer manages the VLAN. If Softlayer creates the VLAN automatically when Softlayer creates other resources, this attribute is set to `true`. If a user creates the VLAN using the SoftLayer API, portal, or ticket, this attribute is set to `false`.
* `child_resource_count` - A count of the resources, such as virtual servers and other network components, that are connected to the VLAN.
* `subnets` - The collection of subnets associated with the VLAN.
    * `subnet` - The subnet for the vlan.
    * `subnet-type` - A subnet can be one of several types. `PRIMARY, ADDITIONAL_PRIMARY, SECONDARY, ROUTED_TO_VLAN, SECONDARY_ON_VLAN, STORAGE_NETWORK, and STATIC_IP_ROUTED`. A `PRIMARY` subnet is the primary network bound to a VLAN within the softlayer network. An `ADDITIONAL_PRIMARY` subnet is bound to a network VLAN to augment the pool of available primary IP addresses that may be assigned to a server. A `SECONDARY` subnet is any of the secondary subnet's bound to a VLAN interface. A `ROUTED_TO_VLAN` subnet is a portable subnet that can be routed to any server on a vlan. A `SECONDARY_ON_VLAN` subnet also doesn't exist as a VLAN interface, but is routed directly to a VLAN instead of a single IP address by SoftLayer's.
    * `subnet-size` - The size of the subnet for the VLAN.
    * `gateway` - A subnet's gateway address.
    * `cidr` - A subnet's Classless Inter-Domain Routing prefix. This is a number between 0 and 32 signifying the number of bits in a subnet's netmask. 

### Timeouts


## ibm_network_vlan_spanning

This resource configures the VLAN spanning attribute for an IaaS account. By default VLAN spanning on the private networt is disabled (off) and servers provisioned on separate private VLANs will not be able to communicate with each other over the private network. When enabled, the private network VLAN spanning service allows all private network VLANs to communicate with one another and hence all servers in the account to communicate with each other. Future servers will be added as they are provisioned. VLAN spanning enables servers to communicate across VLANs in the same data center and across data centers. 

VLAN Spanning must be enabled to use Security Groups containing servers provisioned over multiple VLANs or across multiple data centers and regions. Note VLAN Spanning does not implement network security or firewalls and must be used with Security Groups or Virtual Router Appliances (Vyatta or Juniper) to provide network isolation. 

VRF at an IaaS account level can be used as an alternative to VLAN Spanning and is required if DirectLink is used.  



### Sample Terraform code

```hcl
resource "ibm_network_vlan_spanning" "spanning" {
   "vlan_spanning" = "on"
}`
```


### Input parameters

The following arguments are supported:

* `vlan_spanning` - (Required, string) The desired state of VLAN spanning for the account. Accepted values are `on`, `off`.


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VLAN spanning resource.



## ibm_object_storage_account

Retrieve the account name for an existing Object Storage instance within your IBM account. If no Object Storage instance exists, you can use this resource to order an Object Storage instance and to store the account name.

Do not use this resource for managing the lifecycle of an Object Storage instance in IBM. For lifecycle management, see the [Swift API](https://developer.openstack.org/api-ref/object-store/) or [Swift resources](https://github.com/TheWeatherCompany/terraform-provider-swift).

### Sample Terraform code

```hcl
resource "ibm_object_storage_account" "foo" {
}
```

### Input parameters

* `tags` - (Optional, array of strings) Tags associated with the object storage account instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Computed Fields

The following attributes are exported:

* `id` - The Object Storage account name, which you can use with Swift resources.



## ibm_security_group

Provides a networking security group resource that controls access to the public and private interfaces of a virtual server instance. This resource allows security groups to be created, updated, and deleted. To create rules for the security group, use the `security_group_rule` resource.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_SecurityGroup).

### Sample Terraform code

```
resource "ibm_security_group" "sg1" {
    name = "sg1"
    description = "allow my app traffic"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The descriptive name used to identify the security group.
* `description` - (Optional, string) Additional details to describe the security group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the security group.



## ibm_security_group_rule

Provide a rule for a security group. You can set the IP range to manage incoming (ingress) and outgoing (egress) traffic to a virtual server instance. This resources allows rules for security groups to be created, updated, and deleted. To create the security group, use the `security_group` resource.

For additional details, see the [IBM Cloud Classic Infrastructure (SoftLayer) API docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Network_SecurityGroup_Rule).

### Sample Terraform code

```
resource "ibm_security_group_rule" "allow_port_8080" {
    direction = "ingress"
    ether_type = "IPv4"
    port_range_min = 8080
    port_range_max = 8080
    protocol = "tcp"
    security_group_id = 123456
}
```

### Input parameters

The following arguments are supported:

* `direction` - (Required, string) The direction of traffic. Accepted values: `ingress` or `egress`.
* `ether_type` - (Optional, string) The IP version. Accepted values  (case sensitive): `IPv4` or `IPv6`. Default value: 'IPv4'.
* `port_range_min` - (Optional, int) The start of the port range for allowed traffic.
* `port_range_max` - (Optional, int) The end of the port range for allowed traffic.
* `protocol` - (Optional, string) The IP protocol type. Accepted values (case sensitive): `icmp`,`tcp`, or `udp`.
* `remote_group_id` - (Optional, int) The ID of the remote security group allowed as part of the rule.  

    **NOTE**: Conflicts with `remote_ip`.
* `remote_ip` - (Optional, string) The CIDR or IP address for allowed connections.  

    **NOTE**: Conflicts with `remote_group_id`.
* `security_group_id` - (Required, int) The ID of the security group this rule belongs to.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the security group rule.


## ibm_storage_block

Provides a block storage resource. This allows iSCSI-based [Endurance](https://knowledgelayer.softlayer.com/topic/endurance-storage) and [Performance](https://knowledgelayer.softlayer.com/topic/performance-storage) block storage to be created, updated, and deleted.

Block storage can be accessed and mounted through a Multipath I/O (MPIO) Internet Small Computer System Interface (iSCSI) connection.

To access block storage, see the KnowledgeLayer docs [for Linux](https://knowledgelayer.softlayer.com/procedure/block-storage-linux) or [for Windows](https://knowledgelayer.softlayer.com/procedure/accessing-block-storage-microsoft-windows).

### Sample Terraform code

In the following example, you can create 20G of Endurance block storage with 10G snapshot capacity and 0.25 IOPS/GB.

```hcl
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal05"
        capacity = 20
        iops = 0.25
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true
}
```

In the following example, you can create 20G of Performance block storage and 100 IOPS.

```hcl
resource "ibm_storage_block" "test2" {
        type = "Performance"
        datacenter = "dal05"
        capacity = 20
        iops = 100
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        hourly_billing = true
}
```

### Input parameters

The following arguments are supported:

* `type` - (Required, string) The type of the storage. Accepted values are `Endurance` and `Performance`.
* `datacenter` - (Required, string) The data center where you want to provision the block storage instance.
* `capacity` - (Required, integer) The amount of storage capacity you want to allocate, specified in gigabytes.
* `iops` - (Required, float) The IOPS value for the storage. You can find available values for Endurance storage in the [IBM Cloud Classic Infrastructure (SoftLayer) docs](https://knowledgelayer.softlayer.com/learning/introduction-endurance-storage).
* `os_format_type` - (Required, string) The OS type used to format the storage space. This OS type must match the OS type that connects to the LUN. [Log in to the IBM Cloud Classic Infrastructure (SoftLayer) API to see available OS format types](https://api.softlayer.com/rest/v3/SoftLayer_Network_Storage_Iscsi_OS_Type/getAllObjects/). Use your API as the password to log in. Log in and find the key called `name`.
* `snapshot_capacity` - (Optional, integer) The amount of snapshot capacity to allocate, specified in gigabytes.
* `allowed_virtual_guest_ids` - (Optional, array of integers) The virtual guests that you want to give access to this instance. Virtual guests must be in the same data center as the block storage. You can also use this field to import the list of virtual guests that have access to this storage from the `block_storage_ids` argument in the `ibm_compute_vm_instance` resource.
* `allowed_hardware_ids` - (Optional, array of integers) The bare metal servers that you want to give access to this instance. Bare metal servers must be in the same data center as the block storage. You can also use this field to import the list of bare metal servers that have access to this storage from the `block_storage_ids` argument in the `ibm_compute_bare_metal` resource.
* `allowed_ip_addresses` - (Optional, array of string) The IP addresses that you want to give access to this instance. IP addresses must be in the same data center as the block storage.
* `notes` - (Optional, string) A descriptive note that you want to associate with the block storage.
* `tags` - (Optional, array of strings) Tags associated with the storage block instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.
* `hourly_billing` - (Optional,Boolean) Set true to enable hourly billing.Default is false  
**NOTE**: `Hourly billing` is only available in updated datacenters with improved capabilities.Plesae refer the link to get the updated list of datacenter. http://knowledgelayer.softlayer.com/articles/new-ibm-block-and-file-storage-location-and-features



### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the storage.
* `hostname` - The fully qualified domain name of the storage.
* `volumename` - The name of the storage volume.
* `allowed_virtual_guest_info` - Deprecated please use `allowed_host_info` instead.
* `allowed_hardware_info` - Deprecated please use `allowed_host_info` instead.
* `allowed_host_info` - The user name, password, and host IQN of the hosts with access to the storage.


## ibm_storage_evault

Provides a evault storage resource. This allows [Evault Backup](https://cloud.ibm.com/docs/infrastructure/Backup/index.html#getting-started-with-evault-backup-services) storage to be created, updated, and deleted.

### Sample Terraform code

In the following example, you can create 20G of evault storage 

```hcl
resource "ibm_storage_evault" "test" {
  datacenter          = "dal05"
  capacity            = "20"
  virtual_instance_id = "62870765"
}
```



The following arguments are supported:

* `datacenter` - (Required, string) The data center where you want to provision the evault storage instance.
* `capacity` - (Required, integer) The amount of storage capacity you want to allocate, specified in gigabytes.
* `virtual_instance_id` - (Optional, integer) The id of the virtual instance.
    **NOTE**: Conflicts with `hardware_instance_id`.
* `hardware_instance_id` - (Optional, integer) The id of the hardware instance.
    **NOTE**: Conflicts with `virtual_instance_id`.
* `tags` - (Optional, array of strings) Tags associated with the storage evault instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the evault.
* `username` - The username of the evault.
* `password` - The password of the evault.
* `service_resource_name` - The name of a evault storage network resource.

### Timeouts

ibm_storage_evault provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for Creating Instance.
* `update` - (Default 10 minutes) Used for Updating Instance.




## ibm_storage_file

ibm_storage_file resource provides a file storage resource. This allows NFS-based Endurance and Performance [file storage](https://cloud.ibm.com/docs/infrastructure/FileStorage/index.html) to be created, updated, and deleted.

File storage is mounted using the NFS protocol. For example, a file storage resource with the `hostname` argument set to `nfsdal0501a.service.softlayer.com` and the `volumename` argument set to ` IBM01SV278685_7` has the mount point `nfsdal0501a.service.softlayer.com:\IBM01SV278685_7`.

See [Mounting File Storage](https://cloud.ibm.com/docs/infrastructure/FileStorage/accessing-file-storage-linux.html) for NFS configuration.

### Sample Terraform code

In the following example, you can create 20G of Endurance file storage with a 10G snapshot capacity and 0.25 IOPS/GB.

```hcl
resource "ibm_storage_file" "fs_endurance" {
  type       = "Endurance"
  datacenter = "dal06"
  capacity   = 20
  iops       = 0.25

  # Optional fields
  allowed_virtual_guest_ids = ["28961689"]
  allowed_subnets           = ["10.146.139.64/26"]
  allowed_ip_addresses      = ["10.146.139.84"]
  snapshot_capacity         = 10
  hourly_billing            = true

  # Optional fields for snapshot
  snapshot_schedule = [
    {
      schedule_type   = "WEEKLY"
      retention_count = 20
      minute          = 2
      hour            = 13
      day_of_week     = "SUNDAY"
      enable          = true
    },
    {
      schedule_type   = "HOURLY"
      retention_count = 20
      minute          = 2
      enable          = true
    }
  ]
}

```

In the following example, you can create 20G of Performance file storage with 100 IOPS.

```hcl
resource "ibm_storage_file" "fs_performance" {
        type = "Performance"
        datacenter = "dal06"
        capacity = 20
        iops = 100
        # Optional fields
        allowed_virtual_guest_ids = [ "28961689" ]
        allowed_subnets = [ "10.146.139.64/26" ]
        allowed_ip_addresses = [ "10.146.139.84" ]
        hourly_billing = true
}
```

### Input parameters

The following arguments are supported:

* `type` - (Required, string) The type of the storage. Accepted values are `Endurance` and `Performance`
* `datacenter` - (Required, string) The data center where you want to provision the file storage instance.
* `capacity` - (Required, integer) The amount of storage capacity you want to allocate, expressed in gigabytes.
* `iops` - (Required, float) The IOPS value for the storage instance. You can find available values for Endurance storage in the [IBM docs](https://cloud.ibm.com/docs/infrastructure/FileStorage/index.html#provisioning-with-endurance-tiers).
* `snapshot_capacity` - (Optional, integer) The amount of snapshot capacity you want to allocate, expressed in gigabytes.
* `allowed_virtual_guest_ids` - (Optional, array of integers) The virtual guests that you want to give access to this instance. Virtual guests must be in the same data center as the block storage. You can also use this field to import the list of virtual guests that have access to this storage from the `block_storage_ids` argument in the `ibm_compute_vm_instance` resource.
* `allowed_hardware_ids` - (Optional, array of integers) The bare metal servers that you want to give access to this instance. Bare metal servers must be in the same data center as the block storage. You can also use this field to import the list of bare metal servers that have access to this storage from the `block_storage_ids` argument in the `ibm_compute_bare_metal` resource.
* `allowed_subnets` - (Optional, array of integers) The subnets that you want to give access to this instance. Subnets must be in the same data center as the block storage.
* `allowed_ip_addresses` - (Optional, array of string) The IP addresses that you want to allow. IP addresses must be in the same data center as the block storage. 
* `snapshot_schedule` - (Optional, array) Applies only to Endurance storage. Specifies the parameters required for a snapshot schedule.
    * `schedule_type` - (String) The snapshot schedule type. Accepted values are `HOURLY`, `WEEKLY`, and `DAILY`.
    * `retention_count` - (Integer) The retention count for a snapshot schedule. Required for all types of `schedule_type`.
    * `minute` - (Integer) The minute for a snapshot schedule. Required for all types of `schedule_type`.
    * `hour` - (Integer) The hour for a snapshot schedule. Required if `schedule_type` is set to `DAILY` or `WEEKLY`.
    * `day_of_week` - (String) The day of the week for a snapshot schedule. Required if the `schedule_type` is set to `WEEKLY`.
    * `enable` - (Boolean) Whether to disable an existing snapshot schedule.

* `notes` - (Optional, string) Descriptive text to associate with the file storage.  

* `tags` - (Optional, array of strings) Tags associated with the file storage instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.  
* `hourly_billing` - (Optional,Boolean) Set true to enable hourly billing. Default is false.  
**NOTE**: `Hourly billing` is only available in updated datacenters with improved capabilities.Plesae refer the [link](https://cloud.ibm.com/docs/infrastructure/FileStorage/new-ibm-block-and-file-storage-location-and-features.html#new-locations-and-features-of-file-storage) to get the updated list of datacenter.


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the storage volume.
* `hostname` - The fully qualified domain name of the storage.
* `volumename` - The name of the storage volume.
* `mountpoint` - The network mount address of the storage.


## ibm_subnet

This resource provides portable and static subnets that consist of either IPv4 and IPv6 addresses. Users are able to create 
public portable subnets, private portable subnets, and public static subnets with an IPv4 option, and public portable subnets and public static subnets with an IPv6 option. 
 
The portable IPv4 subnet is created as a secondary subnet on a VLAN. IP addresses in the portable subnet can be assigned as secondary IP 
addresses for IBM resources in the VLAN. Because each portable subnet has a default gateway IP address, network IP address, and broadcast IP address, the number of usable IP addresses is `capacity` - 3. A `capacity` of 4 means that the number of usable IP addresses is 1; a `capacity` of 8 means that the number of usable IP addresses is 5. For example, consider a portable subnet of `10.0.0.0/30` that has `10.0.0.1` as a default gateway IP address, `10.0.0.0` as a network IP address, and `10.0.0.3` as a broadcast IP address. Only `10.0.0.2` can be assigned to IBM resources as a secondary IP address. For additional details, refer to [Static and Portable IP blocks](https://knowledgelayer.softlayer.com/articles/static-and-portable-ip-blocks).

The static IPv4 subnet provides secondary IP addresses for primary IP addresses. It provides secondary IP addresses for IBM resources such as virtual servers, bare metal servers, and netscaler VPXs. Consider a virtual server that requires secondary IP addresses. Users can create a static subnet on the public IP address of the virtual server. Unlike the portable subnet, the number of usable IP addresses for the stactic subnet is the same as the value of `capacity`. For example, when a static subnet of `10.0.0.0/30` has a `capacity` of 4, then four IP addresses (10.0.0.0 - 10.0.0.3) can be used as secondary IP addresses. For additional details, refer to [Subnet](https://knowledgelayer.softlayer.com/topic/subnets).

Both the public portable IPv6 subnet and the public static IP only accept `64` as a value for the `capacity` attribute. They provide 2^64 IP addresses. For additional detail, refer to [IPv6 address](http://blog.softlayer.com/tag/ipv6)

### Sample Terraform code of portable subnet
The following example creates a private portable subnet which has one available IPv4 address:

```hcl
resource "ibm_subnet" "portable_subnet" {
  type = "Portable"
  private = true
  ip_version = 4
  capacity = 4
  vlan_id = 1234567
  notes = "portable_subnet"
  //User can increase timeouts 
  timeouts {
    create = "45m"
  }
}
```

Users can use Terraform built-in functions to get IP addresses from `portable subnet`. The following example returns the first usable IP address of the portable subnet `test`.:

```hcl
resource "ibm_subnet" "test" {
  type = "Portable"
  private = true
  ip_version = 4
  capacity = 4
  vlan_id = 1234567
}


## Use a built-in function cidrhost with index 1.
output "first_ip_address" {
  value = "${cidrhost(ibm_subnet.test.subnet_cidr,1)}"
}

```

### Example Usage of static subnet
The following example creates a public static subnet which has four available IPv4 address:

```hcl
resource "ibm_subnet" "static_subnet" {
  type = "Static"
  private = false
  ip_version = 4
  capacity = 4
  endpoint_ip="151.1.1.1"
  notes = "static_subnet_updated"
}
```

Users can use Terraform built-in functions to get IP addresses from `subnet`. The following example returns the first usable IP address in the static subnet `test`:

```hcl
resource "ibm_subnet" "test" {
  type = "Static"
  private = false
  ip_version = 4
  capacity = 4
  endpoint_ip="159.8.181.82"
}


## Use a built-in function cidrhost with index 0.
output "first_ip_address" {
  value = "${cidrhost(ibm_subnet.test.subnet_cidr,0)}"
}

```



The following arguments are supported:

* `private` - (Optional, boolean) Specifies whether the network is public or private.
* `type` - (Required, string) The type of the subnet. Accepted values are `portable` and `static`.
* `ip_version` - (Optional, integer) The IP version of the subnet. Accepted values are 4 and 6.
* `capacity` - (Required, integer) The size of the subnet.
    * Accepted values for a public portable IPv4 subnet are 4, 8, 16, and 32.
    * Accepted values for a private portable IPv4 subnet are 4, 8, 16, 32, and 64.
    * Accepted values for a public static IPv4 subnet are 1, 2, 4, 8, 16, and 32.
    * Accepted value for a public portable IPv6 subnet is 64. A /64 block is created and 2^64 IP addresses are provided.
    * Accepted value for a public static IPv6 subnet is 64. A /64 block is created and 2^64 IP addresses are provided.
* `vlan_id` - (Optional, integer) The VLAN ID for portable subnet. You can configure both public and private VLAN ID. You can find accepted values in the [Softlayer VLAN documentation](https://cloud.ibm.com/classic/network/vlans) by clicking on the desired VLAN and noting the ID in the resulting URL. You can also [refer to a VLAN by name using a data source](../d/network_vlan.html).
* `endpoint_ip` - (Optional, string) The target primary IP address for a static subnet. Only public IP addresses of virtual servers, bare metal servers, and netscaler VPXs can be configured as an `endpoint_ip`. The `static subnet` will be created on the VLAN where the `endpoint_ip` is located.
* `notes` - (Optional, string) Descriptive text or comments about the subnet.
* `tags` - (Optional, array of strings) Tags associated with the subnet instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the subnet.
* `subnet_cidr` - The IP address/cidr format (ex. 10.10.10.10/28), which you can use to get an available IP address in `subnet`.

### Timeouts

ibm_subnet provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 30 minutes) Used for Creating Instance.




## ibm_ssl_certificate

Provides an SSL certificate resource. This allows SSL certificates to be requested, and delete request for ssl certificates.

For additional details, see the [IBM Cloud Classic Infrastructure(SoftLayer) security certificates Request docs](http://sldn.softlayer.com/reference/datatypes/SoftLayer_Security_Certificate/Request).

### Sample Terraform code

In the following example, you can use a certificate on file:

```hcl
resource "ibm_ssl_certificate" "my_ssllllll" {
  	certificate_signing_request= "-----BEGIN CERTIFICATE REQUEST-----\nCERTIFICATE CONTENT\n-----END CERTIFICATE REQUEST-----"
	organization_information = {
		org_address = {
			org_address_line1= "abc"
			org_address_line2= "xyz"
			org_city="pune"
			org_country_code= "IN"
			org_state="MH"
			org_postal_code= "411045"
		}
		org_organization_name= "GSLAB"
		org_phone_number= "8657072955"
		org_fax_number = ""
	}	
	technical_contact_same_as_org_address_flag = "false"
	technical_contact = {
		tech_address = {
			tech_address_line1= "fcb"
			tech_address_line2= "pqr"
			tech_city="pune"
			tech_country_code= "IN"
			tech_state="MH"
			tech_postal_code= "411045"
		}
		tech_organization_name= "IBM"
		tech_phone_number= "8657072955"
		tech_fax_number = ""
		tech_first_name = "qwerty"
		tech_last_name = "ytrewq"
		tech_email_address = "abc@gmail.com"
		tech_title= "SSL CERT"
	}
	billing_contact = {
		billing_address = {
			billing_address_line1= "plk"
			billing_address_line2= "PLO"
			billing_city="PUNE"
			billing_country_code= "IN"
			billing_state="MH"
			billing_postal_code= "411045"
		}
		billing_organization_name= "IBM"
		billing_phone_number= "8657072955"
		billing_fax_number = ""
		billing_first_name = "ERTYU"
		billing_last_name = "SDFGHJK"
		billing_email_address = "kjjj@gsd.com"
		billing_title= "PFGHJK"
	}
	administrative_contact = {
		admin_address = {
			admin_address_line1= "fghds"
			admin_address_line2= "twyu"
			admin_city="pune"
			admin_country_code= "IN"
			admin_state="MH"
			admin_postal_code= "411045"
		}
		admin_organization_name = "GSLAB"
		admin_phone_number = "8657072955"
		admin_fax_number = ""
		admin_first_name = "DFGHJ"
		admin_last_name = "dfghjkl"
		admin_email_address = "fghjk@gshhds.com"
		admin_title= "POIUYGHJK"
	}	
	administrative_contact_same_as_technical_flag = "false"
	billing_contact_same_as_technical_flag = "false"	
	billing_address_same_as_organization_flag = "false"
	administrative_address_same_as_organization_flag = "false"
	ssl_type="SSL_CERTIFICATE_QUICKSSL_PREMIUM_2_YEAR"
	renewal_flag= true
	server_count= 1
	server_type= "apache2"
	validity_months= 24
	order_approver_email_address= "admin@pune.in"	
}
```


### Input parameters

The following arguments are supported:

* `certificateSigningRequest` - (Required, string) The Certificate Signing Request which is specially formatted encrypted message sent from a Secure Sockets Layer (SSL) digital certificate applicant to a certificate authority.
* `sslType` - (Required, string) The ssl certificate type.
* `serverType` - (Required, string) The server type for which we are requesting ssl certficate.
* `serverCount` - (Required, string) The number of servers with provided server tye .
* `validityMonths` - (Required, integer) The validity of ssl certificate in months it should be multiple of 12.
* `orderApproverEmailAddress` - (Required, string) The email of approver to approve ssl certificate request.
* `organization_information`- (Required, set) Organization information from issuer belongs to.
	* `org_address`- (Required, string) Organization address of the issuer.
		* `org_addressLine1` - (Required, string) The address of organization who is requesting for ssl certificate.
		* `org_addressLine2` - (optional, string) The address of organization who is requesting for ssl certificate.
		* `org_city` - (Required, string) The city of organization which is requesting for ssl certificate .
		* `org_postalCode` - (Required, integer) The postal code for the city of organization.
		* `org_state` - (Required, string) The two letter state code of organization who is requesting for ssl certificate. Allowed value for country which doesn't have states is `OT`.
		* `org_countryCode` - (Required, string) The two letter country code of organization.
	* `org_organizationName` - (Required, string) Name of organization.
	* `org_phone_number` - (Required, string) Phone number of organization
	* `org_fax_number` - (Optional, string) Fax number for organization
* `technical_contact` - (Required, set) Technical contact details of issuer.
	* `tech_address` - (Optional, set) Technical address details
		* `tech_addressLine1` - (Required, string) The address for technical contact.
		* `tech_addressLine2` - (Optional, string) The address for technical contact.
		* `tech_city` - (Required, string) The city for technical contact.
		* `tech_postalCode` - (Required, integer) The postal code for technical contact.
		* `tech_state` - (Required, string) The two letter state code of technical contact. Allowed value for country which doesn't have states is `OT`.
		* `tech_countryCode` - (Required, string) The two letter country code for technical contact.
	* `tech_organizationName` - (Required, string) Name of organization for technical contact.
	* `tech_firstName` - (Required, string) The first name for technical contact.
	* `tech_lastName` - (Required, string) The last name for technical contact.
	* `tech_title` - (Required, string) The title for for technical contact.
	* `tech_emailAddress` -(Required, string) email address for technical contact.
	* `tech_phone_number`- (Required, string) phone number for technical detail.
	* `tech_fax_number` - (Optional, string) Fax number for technical detail.
* `administrative_contact` - (Optional, set) Administrator contact details.
	* `admin_address` - (Optional, set) Administrator address details.
		* `admin_addressLine1` - (Optional, string) The address for administrative contact.
		* `admin_addressLine2` - (optional, string) The address for administrative contact.
		* `admin_city` - (Optional, string) The city for administrative contact.
		* `admin_postalCode` - (Optional, integer) The postal code for administrative contact.
		* `admin_state` - (Optional, string) The two letter state code of administrative contact. Allowed value for country which doesn't have states is `OT`.
		* `admin_countryCode` - (Optional, string) The two letter country code for administrative contact.
	* `admin_organizationName` - (Optional, string) Name of organization for administrative contact.
	* `admin_firstName` - (Optional, string) The first name for administrative contact.
	* `admin_lastName` - (Optional, string) The last name for administrative contact.
	* `admin_title` - (Optional, string) The title for for administrative contact.
	* `admin_emailAddress` -(Optional, string) email address for administrative contact.
	* `admin_phone_number` - (Optional, string) Phone number of administrator.
	* `admin_fax_number` - (Optional, string) Fax number for administrator.

* `billing_contact` - (Optional, set) Billing Contact details.
	* `billing_address` - (Optional, set) Billing address details.
		* `billing_addressLine1` - (Optional, string) The address for billing contact.
		* `billing_addressLine2` - (optional, string) The address for billing contact.
		* `billing_city` - (Optional, string) The city for billing contact.
		* `billing_postalCode` - (Optional, integer) The postal code for billing contact.
		* `billing_state` - (Optional, string) The two letter state code of billing contact. Allowed value for country which doesn't have states is `OT`.
		* `billing_countryCode` - (Optional, string) The two letter country code for billing contact.
	* `billing_organizationName` - (Optional, string) Name of organization for billing contact.
	* `billing_firstName` - (Optional, string) The first name for billing contact.
	* `billing_lastName` - (Optional, string) The last name for billing contact.
	* `billing_title` - (Optional, string) The title for for billing contact.
	* `billing_emailAddress` - (Optional, string) email address for billing contact.
	* `billing_phone_number` - (Optional, string) Phone number for billing contact.
	* `billing_fax_number` - (Optional, string) Fax number for billing contact.
* `technicalContactSameAsOrgAddressFlag` -(Optional, bool) If your organization address and technical contact address is the same make this flag as true and skip technical contact address details.
* `administrativeContactSameAsTechnicalFlag` -(Required, bool)- If your technical contact details and administrative contact details is the same then make this as true and skip details of administrative contact.
* `billingContactSameAsTechnicalFlag` -(Required, bool)- If your technical contact details and billing contact details is the same then make this as true and skip details of billing contact. 
* `administrativeAddressSameAsOrganizationFlag` -(Required,bool)If administrative address is same as organization address then make this flag as true and skip address details.
* `billingAddressSameAsOrganizationFlag` -(Required,bool)If billing address is same as organization address then make this flag as true and skip address details. 