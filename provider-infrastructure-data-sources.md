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

# Infrastructure Data Sources
{: #infrastructure-data-sources}


## ibm_cis

Imports a read only copy of an existing Internet Services resource. This allows CIS sub-resources to be added to an existing CIS instance. This includes domains, DNS records, pools, healthchecks and global load balancers. 

### Sample Terraform code

```hcl
data "ibm_cis" "cis_instance" {
  name              = "test"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name used to identify the Internet Services instance in the IBM Cloud UI. 

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of this CIS instance.
* `plan` - The service plan for this Internet Services' instance
* `location` - The location for this Internet Services' instance
* `status` - Status of the CIS instance.



## ibm_cis_domain

Imports a read only copy of an existing Internet Services domain resource. This allows new/additional CIS sub-resources to be added to an existing CIS domain registration, specifically DNS records and global load balancers. It is used in conjunction with the CIS data-source. 

### Sample Terraform code

```hcl
data "ibm_cis_domain" "cis_instance_domain" {
  domain = "example.com"
  cis_id = "${ibm_cis.instance.id}"
}
data "ibm_cis" "cis_instance" {
  name              = "test"
}
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


## ibm_cis_ip_addresses

Import the IP addresses used for name servers by Cloud Internet Services. You can then reference the IP addresses by interpolation to configure firewalls, network ACLs and Security Groups to white list these addresses. 

### Sample Terraform code

```hcl
data "ibm_cis_ip_addresses" "cisname" {
}
```


### Input parameters

No arguments are required. All CIS instances on an account use the same range of name servers. 

### Output parameters

The following attributes are exported:

* `ipv4_cidrs` - The ipv4 address ranges used by CIS for name servers. To be whitelisted by the service user. 
* `ipv6_cidrs` - The ipv6 address ranges used by CIS for name servers. To be whitelisted by the service user. 



## ibm_compute_bare_metal

Import the details of an existing bare metal as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_compute_bare_metal" "bare_metal" {
  hostname    = "jumpbox"
  domain      = "example.com"
  most_recent = true
}

data "ibm_compute_bare_metal" "bare_metal" {
  global_identifier = "a471e9a6-82e7-41a7-ac8d-39ced672c0ed"
}
```

### Input parameters

The following arguments are supported:

* `hostname` - (Optional, string) The hostname of the bare metal server.  
  **NOTE**: Conflicts with `global_identifier`.
* `domain` - (Optional, string) The domain of the bare metal server.  
  **NOTE**: Conflicts with `global_identifier`.
* `global_identifier` - (Optional, string) The unique global identifier of the bare metal server. To see global identifier, log in to the [IBM Cloud Classic Infrastructure (SoftLayer) API](https://api.softlayer.com/rest/v3.1/SoftLayer_Account/getHardware.json), using your API key as the password.  
  **NOTE**: Conflicts with `hostname`, `domain`, `most_recent`.
* `most_recent` - (Optional, boolean) If there are multiple bare metals, you can set this argument to `true` to import only the most recently created server.  
   **NOTE**: Conflicts with `global_identifier`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the bare metal server.
* `datacenter` - The data center in which the bare metal server is deployed.
* `network_speed` - The connection speed, expressed in Mbps,  for the server network components.
* `public_bandwidth` - The amount of public network traffic, allowed per month.
* `public_ipv4_address` - The public IPv4 address of the bare metal server.
* `public_ipv4_address_id` - The unique identifier for the public IPv4 address of the bare metal server.
* `private_ipv4_address` - The private IPv4 address of the bare metal server.
* `private_ipv4_address_id` - The unique identifier for the private IPv4 address of the bare metal server.
* `public_vlan_id` - The public VLAN used for the public network interface of the server. 
* `private_vlan_id` - The private VLAN used for the private network interface of the server. 
* `public_subnet` - The public subnet used for the public network interface of the server. 
* `private_subnet` - The private subnet used for the private network interface of the server. 
* `hourly_billing` -  The billing type of the server.
* `private_network_only` - Specifies whether the server only has access to the private network.
* `user_metadata` - Arbitrary data available to the computing server.
* `notes` -  Notes associated with the server.
* `memory` - The amount of memory in gigabytes, for the server.
* `redundant_power_supply` -  When the value is `true`, it indicates additional power supply is provided.
* `redundant_network` - When the value is `true`, two physical network interfaces are provided with a bonding configuration.
* `unbonded_network` - When the value is `true`, two physical network interfaces are provided without a bonding configuration.
* `os_reference_code` - An operating system reference code that provisioned the computing server.
*  `tags` - Tags associated with this bare metal server.
* `block_storage_ids` - Block storage to which this computing server have access.
* `file_storage_ids` - File storage to which this computing server have access.
* `ipv6_enabled` - Indicates whether the public IPv6 address enabled or not.
* `ipv6_address` - The public IPv6 address of the bare metal server.
* `ipv6_address_id` - The unique identifier for the public IPv6 address of the bare metal server.
* `secondary_ip_count` - The number of secondary IPv4 addresses of the bare metal server.
* `secondary_ip_addresses` - The public secondary IPv4 addresses of the bare metal server.



## ibm_compute_image_template

Import the details of an existing image template as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_compute_image_template" "img_tpl" {
    name = "jumpbox"
}
```

The following example shows how you can use this data source to reference the image template ID in the `ibm_compute_vm_instance` resource because the numeric IDs are often unknown.

```hcl
resource "ibm_compute_vm_instance" "vm1" {
    ...
    image_id = "${data.ibm_compute_image_template.img_tpl.id}"
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the image template as it was defined in IBM Cloud Classic Infrastructure (SoftLayer). You can find the name in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic) by navigating to **Devices > Manage > Images**.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the image template.



## ibm_compute_placement_group

Import the details of an existing placement group as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_compute_placement_group" "group" {
    name = "demo"
}
```

The following example shows how you can use this data source to reference the placement group ID in the `ibm_compute_vm_instance` resource because the numeric IDs are often unknown.

```hcl
resource "ibm_compute_vm_instance" "vm1" {
    ...
    placement_group_id = "${data.ibm_compute_placement_group.group.id}"
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the placement group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the placement group.
* `datacenter` - The datacenter in which placement group resides.
* `pod` - The pod in which placement group resides.
* `rule` - The rule of the placement group.
* `virtual_guests` - A nested block describing the VSIs attached to the placement group. Nested `virtual_guests` blocks have the following structure:
  * `id` - The ID of the virtual guest.
  * `domain` - The domain of the virtual guest.
  * `hostname` - The hostname of the virtual guest.






## ibm_compute_ssh_key

Import the details of an existing SSH key as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_compute_ssh_key" "public_key" {
    label = "Terraform Public Key"
}
```

The following example shows how you can use this data source to reference the SSH key IDs in the `ibm_compute_vm_instance` resource because the numeric IDs are often unknown.

```hcl
resource "ibm_compute_vm_instance" "vm1" {
    ...
    ssh_key_ids = ["${data.ibm_compute_ssh_key.public_key.id}"]
    ...
}
```

### Input parameters

The following arguments are supported:

* `label` - (Required, string) The label of the SSH key, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer).
* `most_recent` - (Optional, boolean) If more than one SSH key matches the label, you can set this argument to `true` to import only the most recent key.
  **NOTE**: The search must return only one match. More or less than one match causes Terraform to fail. Ensure that your label is specific enough to return a single SSH key only, or use the `most_recent` argument.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the SSH key.  
* `fingerprint` - The sequence of bytes to authenticate or look up a longer SSH key.
* `public_key` - The public key contents.
* `notes` - The notes that are stored with the SSH key.



## ibm_compute_vm_instance

Import the details of an existing VM instance as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_compute_vm_instance" "vm_instance" {
  hostname    = "jumpbox"
  domain      = "example.com"
  most_recent = true
}
```

### Input parameters

The following arguments are supported:

* `hostname` - (Required, string) The hostname of the VM instance.
* `domain` - (Required, string) The domain of the VM instance.
* `most_recent` - (Optional, boolean) If there are multiple VM instances, you can set this argument to `true` to import only the most recently created instance.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VM instance.
* `datacenter` - The data center in which the VM instance is deployed.
* `public_interface_id` - The ID of the primary public interface.
* `private_interface_id` - The ID of the primary private interface.
* `cores` - The number of CPU cores.
* `status` - The VSI status.
* `last_known_power_state` - The last known power state of a VM instance, in the event the instance is turned off outside the information management system (IMS) or has gone offline.
* `power_state` - The current power state of a VM instance.
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
* `secondary_ip_count` - Number of secondary public IPv4 addresses.


## ibm_database

Creates a read only copy of an existing IBM Cloud Databases resource.  

Configuration of an ICD data_source requires that the `region` parameter is set for the IBM provider in the `provider.tf` to be the same as the ICD service `location/region` that the service will be deployed in. If not specified it will default to `us-south`. A `terraform refresh` of the data_source will fail if the ICD `location` is  different to that specified on the provider.  

### Sample Terraform code

```hcl
data "ibm_database" "<your_database>" {
  name = "<your_database>"
  location = "<your-db-location>"
}
```

### Input parameters

The following arguments are required:

* `name` - (Required, string) The name used to identify the IBM Cloud Database instance in the IBM Cloud UI. IBM Cloud does not enforce that service names are unique and it is possible that duplicate service names exist. The first located service instance is used by Terraform. The name must not include spaces.

* `resource_group_id` - (Optional, string) The id of the resource group where the resource instance exists. If not provided it takes the default resource group.

* `location` - (Optional, string) The location or the environment in which instance exists.

* `service` - (Optional, string) The service type of the instance. You can retrieve the value by running the `ibmcloud catalog service-marketplace` or `ibmcloud catalog search` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of this ICD instance.
* `plan` - The service plan for this ICD instance
* `location` - The location or region for this ICD instance. This will be the same as defined for the provider or its alias.
* `status` - Status of the ICD instance.
* `id` - The unique identifier of the new database instance (CRN).
* `status` - Status of resource instance.
* `adminuser` - userid of the default admistration user for the database, usually `admin` or `root`.
* `version` - Database version. 
* `connectionstrings` - List of connection strings by userid for the database. See the IBM Cloud documentation for more details of how to use connection strings in ICD for database access: https://cloud.ibm.com/docs/services/databases-for-postgresql/howto-getting-connection-strings.html#getting-your-connection-strings. The results are returned in pairs of the userid and string:
  `connectionstrings.1.name = admin`
  `connectionstrings.1.string = postgres://admin:$PASSWORD@79226bd4-4076-4873-b5ce-b1dba48ff8c4.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:32554/ibmclouddb?sslmode=verify-full`
* `whitelist` - List of whitelisted IP addresses or ranges.

Note that the provider only exports the admin userid and associated connectionstring. It does not export any userids additionally configured for the instance. This is due to a lack of ICD function. 




## ibm_dns_domain

Import the name of an existing domain as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_dns_domain" "domain_id" {
    name = "test-domain.com"
}
```

The following example shows how you can use this data source to reference the domain ID in the `ibm_dns_record` resource, since the numeric IDs are often unknown.

```hcl
resource "ibm_dns_record" "www" {
    ...
    domain_id = "${data.ibm_dns_domain.domain_id.id}"
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the domain, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the domain.



## ibm_dns_domain_registration

Import an existing domain registration from the IBM DNS Domain Registration Service as a read-only data source. This DNS registration resource can then be used as a base from which DNS configurations can be performed via Terraform. The domain must inititally be registered via the UI of the IBM Cloud DNS Registration Service. Typically the Domain Registration datasource is used in configuration with global load balancing services, e.g. CloudFlare, Akamai or IBM Cloud Internet Services (Cloudflare). For additional usage instructions see the resource `ibm_dns_domain_registration_nameservers`. 

### Sample Terraform code

```hcl
data "ibm_dns_domain_registration" "dnstestdomain" {
    name = "dnstestdomain.com"
}
```

The following example shows how you can use this data source to reference the domain ID in the `ibm_dns_registration_nameservers` resource, since the numeric IDs are often unknown.

```hcl
resource "ibm_dns_domain_registration_nameservers" "dnstestdomain" {
    ...
    dns_registration_id = "${data.ibm_dns_domain_registration.dnstestdomain.id}"
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the DNS domain registration as it was defined in IBM Cloud Infrastructure DNS Registration Service (SoftLayer).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the domain.



## ibm_dns_secondary

Import the name of an existing dns secondary zone as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_dns_secondary" "secondary_id" {
    name = "test-secondary.com"
}
```

### Input parameters

The following arguments are supported:

* `zone_name` - (Required, string) The name of the secondary zone, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the secondary.
* `transfer_frequency` - Signifies how often a secondary DNS zone transferred in minutes.
* `master_ip_address` - The IP address of the master name server where a secondary DNS zone is transferred from.
* `status_id` - The current status of a secondary DNS record.
* `status_text` - The textual representation of a secondary DNS zone's status.



## ibm_lbaas

Import the details of an existing IBM Cloud load balancer as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.
 
### Sample Terraform code

```hcl
resource "ibm_lbaas" "lbaas" {
  name        = "test"
  description = "updated desc-used for terraform uat"
  subnets     = [1878778]
  datacenter  = "dal09"

  protocols = [{
    "frontend_protocol" = "HTTP"
    "frontend_port" = 80
    "backend_protocol" = "HTTP"
    "backend_port" = 80
    "load_balancing_method" = "round_robin"
  }]

  server_instances = [{
    "private_ip_address" = "10.1.19.26",
  },
  ]
}
    data "ibm_lbaas" "tfacc_lbaas" {
    name = "test"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The load balancer's name.

### Output parameters

The following attributes are exported:

* `description` - A description of the load balancer.
* `datacenter` - The datacenter where load balancer is located.
* `protocols` - A nested block describing the protocols assigned to the load balancer. Nested `protocols` blocks have the following structure:
  * `frontend_protocol` - The frontend protocol.
  * `frontend_port` - The frontend protocol port number.
  * `backend_protocol` - The backend protocol.
  * `backend_port` - The backend protocol port number.
  * `load_balancing_method` - The load balancing algorithm.
  * `session_stickiness` - Session stickiness.
  * `max_conn` - The number of connections the listener can accept.
  * `tls_certificate_id` - The ID of the SSL/TLS certificate being used for a protocol.
  * `protocol_id` - The UUID of a load balancer protocol.
* `server_instances` - A nested block describing the server instances for this load balancer. Nested `server_instances` blocks have the following structure:
  * `private_ip_address` - The private IP address of a load balancer member.
  * `weight` - The weight of a load balancer member.
  * `status` - Specifies the status of a load balancer member as `UP` or `DOWN`.
  * `member_id` - The UUID of a load balancer member.
* `health_monitors` - A nested block describing the health_monitors assigned to the load balancer. Nested `health_monitors` blocks have the following structure:
  * `protocol` - Backends protocol
  * `port` - Backends port
  * `interval` - Interval in seconds to perform 
  * `max_retries` - Maximum retries
  * `timeout` - Health check methods timeout in 
  * `url_path` - If monitor is "HTTP", this specifies URL path
  * `monitor_id` - Health Monitor UUID
* `type` - Specifies whether a load balancer is public or private.
* `status` - Specifies the operation status of the load balancer as 'ONLINE' or 'OFFLINE'.
* `vip` - The virtual IP address of the load balancer.
* `server_instances_up` - The number of service instances which are in the `UP` health state.
* `server_instances_down` - The number of service instances which are in the `DOWN` health state.
* `active_connections` - The number of total established connections.
* `use_system_public_ip_pool` - It specifies whether the public IP addresses are allocated from system public IP pool or public subnet from the account ordering the load balancer.
* `ssl_ciphers` - The list of ssl offloads.


## ibm_network_vlan


Import the details of an existing VLAN as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
data "ibm_network_vlan" "vlan_foo" {
    name = "FOO"
}
```


The following example shows how you can use this data source to reference a VLAN ID in the _ibm_compute_bare_metal_ resource because the numeric IDs are often unknown.

```hcl
resource "ibm_compute_bare_metal" "bm1" {
    ...
    public_vlan_id = "${data.ibm_network_vlan.vlan_foo.id}"
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required if neither the number nor the router hostname are provided, string) The name of the VLAN, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer). You can find names in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic/network/vlans) by navigating to **Network > IP Management > VLANs**.
* `number` - (Required if the name is not provided, integer) The VLAN number. You can find the numbers in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic/network/vlans).
* `router_hostname` - (Required if the name is not provided, string) The primary VLAN router hostname. You can find the  hostnames in the [IBM Cloud infrastructure customer portal](https://cloud.ibm.com/classic/network/vlans).

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VLAN.
* `subnets` - The collection of subnets associated with the VLAN.
    * `id` - The ID of the subnet.  
    * `subnet` - The subnet for the vlan.
    * `subnet-type` - A subnet can be one of several types. `PRIMARY, ADDITIONAL_PRIMARY, SECONDARY, ROUTED_TO_VLAN, SECONDARY_ON_VLAN, STORAGE_NETWORK, and STATIC_IP_ROUTED`. A `PRIMARY` subnet is the primary network bound to a VLAN within the softlayer network. An `ADDITIONAL_PRIMARY` subnet is bound to a network VLAN to augment the pool of available primary IP addresses that may be assigned to a server. A `SECONDARY` subnet is any of the secondary subnet's bound to a VLAN interface. A `ROUTED_TO_VLAN` subnet is a portable subnet that can be routed to any server on a vlan. A `SECONDARY_ON_VLAN` subnet also doesn't exist as a VLAN interface, but is routed directly to a VLAN instead of a single IP address by SoftLayer's.
    * `subnet-size` - The size of the subnet for the VLAN.
    * `gateway` - A subnet's gateway address.
    * `cidr` - A subnet's Classless Inter-Domain Routing prefix. This is a number between 0 and 32 signifying the number of bits in a subnet's netmask. 
* `virtual_guests` - A nested block describing the VSIs attached to the VLAN. Nested `virtual_guests` blocks have the following structure:
  * `id` - The ID of the virtual guest.
  * `domain` - The domain of the virtual guest.
  * `hostname` - The hostname of the virtual guest.




## ibm_security_group

Import the details of an existing security group as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_security_group" "allow_ssh" {
    name = "allow_ssh"
}
```

The following example shows how you can use this data source to reference the security group IDs in the `ibm_compute_vm_instance` resource because the numeric IDs are often unknown.

```hcl
resource "ibm_compute_vm_instance" "vm1" {
    ...
    private_security_group_ids = ["${data.ibm_security_group.allow_ssh.id}"]
    ...
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the security group, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer).
* `description` - (Optional, string) The description of the security group, as it was defined in IBM Cloud Classic Infrastructure (SoftLayer).
* `most_recent` - (Optional, boolean) If more than one security group has the same name or description, you can set this argument to `true` to import only the most recent security group.
  **NOTE**: The search must return only one match, otherwise Terraform fails. Ensure that your name and description combinations are specific enough to return a single security group key only, or set the `most_recent` argument to `true`.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the security group.
* `description` - The description of the security group.
