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

# Virtual Private Cloud Classic Services Data Sources
{: #virtual-private-cloud-classic-services-data-sources}


## ibm_is_image

Import the details of an existing IBM Cloud Infrastructure image as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_image" "ds_image" {
    name = "centos-7.x-amd64"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the image.
* `visibility` - (Optional, string) The visibility of the image. Accepted values are `public` or `private`.


### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this image.
* `crn` - The CRN for this image.
* `os` - The name of the operating system.
* `status` - The status of this image.
* `architecture` - The architecture for this image.






## ibm_is_images

Import the details of an existing IBM Cloud Infrastructure images as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_images" "ds_images" {
}

```

### Output parameters

The following attributes are exported:

* `images` - List of all images in the IBM Cloud Infrastructure.
  * `name` - The name for this image.
  * `id` - The unique identifier for this image.
  * `crn` - The CRN for this image.
  * `os` - The name of the operating system.
  * `status` - The status of this image.
  * `architecture` - The architecture for this image.
  * `visibility` - The visibility of the image public or private.






## ibm_is_instance_profile

Import the details of an existing IBM Cloud virtual server instance profile as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_instance_profile" "profile" {
	name = "b-2x8"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name for this virtual server instance profile.

### Output parameters

The following attributes are exported:

* `family` - The family of the virtual server instance profile.


## ibm_is_instance_profiles

Import the details of an existing IBM Cloud virtual server instance profiles as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_instance_profiles" "ds_instance_profiles" {
}

```

### Output parameters

The following attributes are exported:

* `profiles` - List of all server instance profiles in the region.
  * `name` - The name for this virtual server instance profile.
  * `family` - The family of the virtual server instance profile.




## ibm_is_region

Import the details of an existing IBM Cloud region as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_region" "ds_region" {
    name = "us-south"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the region.

### Output parameters

The following attributes are exported:

* `status` - The status of region.
* `endpoint` - The endpoint of the region.


## ibm_is_ssh_key

Import the details of an existing IBM VPC SSh Key as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_ssh_key" "ds_key" {
    name = "test"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the Key.

### Output parameters

The following attributes are exported:

* `id` - The id of the ssh key.
* `fingerprint` -  The SHA256 fingerprint of the public key.
* `length` - The length of this key.
* `type` - The cryptosystem used by this key.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.



## ibm_is_subnet

Import the details of an existing IBM cloud subnet as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_subnet" "testacc_subnet" {
	name = "test_subnet"
	vpc = "${ibm_is_vpc.testacc_vpc.id}"
	zone = "us-south-1"
	ipv4_cidr_block = "192.168.0.0/1"
}
data "ibm_is_subnet" "ds_subnet" {
	identifier = "${ibm_is_subnet.testacc_subnet.id}"
}

```

### Input parameters

The following arguments are supported:

* `identifier` - (Required, string) The id of the subnet.

### Output parameters

The following attributes are exported:

* `ipv4_cidr_block` -  The IPv4 range of the subnet.
* `ipv6_cidr_block` - The IPv6 range of the subnet.
* `total_ipv4_address_count` - The total number of IPv4 addresses.
* `ip_version` - The Ip Version.
* `name` - The name of the subnet.
* `network_acl` - The ID of the network ACL for the subnet.
* `public_gateway` - The ID of the public-gateway for the subnet.
* `status` - The status of the subnet.
* `vpc` - The vpc id.
* `zone` - The subnet zone name.
* `available_ipv4_address_count` - The total number of available IPv4 addresses.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.



## ibm_is_vpc

Import the details of an existing IBM Virtual Private cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}

data "ibm_is_vpc" "ds_vpc" {
    name = "test"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the VPC.

### Output parameters

The following attributes are exported:

* `status` - The status of VPC.
* `default_network_acl` - ID of the default network ACL.
* `classic_access` - Indicates whether this VPC is connected to Classic Infrastructure.
* `resource_group` - The resource group ID where the VPC created.
* `tags` - Tags associated with the instance.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.



## ibm_is_zone

Import the details of an existing IBM Cloud zone in a particular region as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_zone" "ds_zone" {
    name = "us-south-1"
    region = "us-south"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the zone.
* `region` - (Required, string) The name of the region.

### Output parameters

The following attributes are exported:

* `status` - The status of zone.


## ibm_is_zones

Import the details of an existing IBM Cloud zones in a particular region as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl

data "ibm_is_zones" "ds_zones" {
    region = "us-south"
}

```

### Input parameters

The following arguments are supported:

* `region` - (Required, string) The name of the region.
* `status` - (Optional, string) Filter the list by status of zones.

### Output parameters

The following attributes are exported:

* `zones` - The list of zones in a region.