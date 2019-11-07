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

# Power Virtual Server Data Sources
{: #power-virtual-server-data-sources}


## ibm_pi_image

Import the details of an existing IBM Power Virtual Server Cloud image as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_image" "ds_image" {
  pi_image_name        = "7200-03-03"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_image_name` - (Required, string) The name of the image.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this image.
* `state` - The state for this image.
* `size` - The size of the image.
* `architecture` - The architecture for this image.
* `operatingsystem` - The operating system for this image.



## ibm_pi_images

Import the details of existing IBM Power Virtual Server Cloud images as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_images" "ds_images" {
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

The following arguments are supported:

* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `images` - List of all images in the IBM Power Virtual Server Cloud.
  * `name` - The name for this image.
  * `id` - The unique identifier for this image
  * `state` - The state of the operating system.
  * `href` - The href  of this image.



## ibm_pi_instance

Import the details of an existing IBM Power Virtual Server instance as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_instance" "ds_instance" {
  pi_instance_name     = "terraform-test-instance"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_instance_name` - (Required, string) The name of the instance.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this instance.
* `address` - The address associated with this instance.
* `state` - The state of the instance.



## ibm_pi_key

Import the details of an existing IBM Power Virtual Server key as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_key" "ds_instance" {
  pi_key_name          = "terraform-test-key"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_key_name` - (Required, string) The name of the key.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this instance.
* `creationdate` - The creation date.
* `sshkey` - The SSH RSA key.



## ibm_pi_network

Import the details of an existing IBM Power Virtual Server Cloud network as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_network" "ds_network" {
  pi_network_name = "APP"
  powerinstanceid = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_network_name` - (Required, string) The name of the network.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this network.
* `gateway_ip` - The gateway for this network.
* `available_ip_count` - The available IPs for this network.
* `used_ip_count` - The IPs that are in use for this network.
* `used_ip_percent` - The used ip in percent.
* `vlan_type` - The type of vlan for this network.



## ibm_pi_public_network

Import the details of an existing IBM Power Virtual Server Cloud public network as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_public_network" "ds_public_network" {
  pi_network_name      = "PUBLIC"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_network_name` - (Required, string) The name of the network.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `networkid` - The unique identifier for this network.
* `type` - The network type for this network.
* `name` - The name of the network.
* `vlanid` - The VLAN id for the network.



## ibm_pi_tenant

Import the details of an existing IBM Power Virtual Server Cloud tenant as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_tenant" "ds_tenant" {
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `tenantid` - The unique identifier for this tenant.
* `creationdate` - The date on which the tenant was created.
* `enabled` - Indicates whether the tenant is enabled.
* `tenantname` - The name of the tenant.
* `cloudinstances` - Lists the regions and instance IDs this tenant owns.



## ibm_pi_volume

Import the details of an existing IBM Power Virtual Server Cloud volume as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_volume" "ds_volume" {
  pi_volume_name       = "volume_1"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_volume_name` - (Required, string) The name of the volume.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this volume.
* `type` - The disk type for this volume.
* `state` - The state of the volume.
* `bootable` - If this volume is bootable or not.
* `size` - The size of this volume.



## ibm_pi_instance_volumes

Import the details of existing IBM Power Virtual Server Cloud instance volumes as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_pi_instance_volumes" "ds_volumes" {
  pi_instance_name     = "volume_1"
  pi_cloud_instance_id = "49fba6c9-23f8-40bc-9899-aca322ee7d5b"
}
```

### Input parameters

The following arguments are supported:

* `pi_instance_name` - (Required, string) The name of the instance whose volumes to retrieve.
* `pi_cloud_instance_id` - (Required, string) The service instance associated with the account.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier for this volume.
* `type` - The disk type for this volume.
* `state` - The state of the volume.
* `bootable` - If this volume is bootable or not.
* `size` - The size of this volume.
* `shareable` - If this volume is shareable or not.
* `href` - The href of this volume.
* `name` - The name of this volume.
