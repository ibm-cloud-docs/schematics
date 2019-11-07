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

# Power Virtual Server Resources
{: #power-virtual-server-resources}


## ibm_pi_instance

Provides an instance resource. This allows instances to be created or updated in the Power Virtual Server Cloud.

### Sample Terraform code

In the following example, you can create an instance:

```hcl
resource "ibm_pi_instance" "test-instance" {
    pi_memory             = "4"
    pi_processors         = "2"
    pi_instance_name      = "test-vm"
    pi_proc_type          = "shared"
    pi_migratable         = "true"
    pi_image_id           = "<id of the image to deploy - e.g., 7200-03-03>"
    pi_volume_ids         = []
    pi_network_ids        = ["<id of the VM's network IDs>"]
    pi_key_pair_name      = "<name of SSH key>"
    pi_sys_type           = "s922"
    pi_replication_policy = "none"
    pi_replicants         = "1"
    pi_cloud_instance_id  = "<value of the cloud_instance_id>"
```



The following arguments are supported:

* `pi_instance_name` - (Required, string) The name of the VM.
* `pi_creation_date` - (Required, string) The date on which the VM was created.
* `pi_key_pair_name` - (Required, string) The name of the Power Virtual Server Cloud SSH key to used to login to the VM.
* `pi_image_id` - (Required, string) The name of the image to deploy (e.g., 7200-03-03).
* `pi_processors` - (Required, float) The number of vCPUs to assign to the VM (as visibile within the guest operating system).
* `pi_proc_type` - (Required, string) The type of processor mode in which the VM will run (shared/dedicated).
* `pi_memory` - (Required, float) The amount of memory (GB) to assign to the VM.
* `pi_sys_type` - (Required, string) The type of system on which to create the VM (s922/e880/any).
* `pi_volume_ids` - (Required, list(string)) The list of volume IDs to attach to the VM at creation time.
* `pi_network_ids` - (Required, list(string)) The list of network IDs assigned to the VM.
* `pi_migratable` - (Required, boolean) Dictates whether the VM can be migrated (e.g., for server maintenance).
* `pi_cloud_instance_id` - (Required, string) The cloud_instance_id for this account.
* `pi_user_data` - (Optional, string) The base64-encoded form of the user data (cloud-init) to pass to the VM at creation time.
* `pi_public_network` - (Optional, boolean) Dictates whether the VM will be attached to a public network (true/false); default is false.
* `pi_replicants` - (Optional, float) Specifies the number of VMs to deploy; default is 1.
* `pi_replication_policy` - (Optional, string) Specifies the replication policy (e.g., none).
* `pi_replication_scheme` - (Optional, string) Specifies the replicate scheme (prefix/suffix).

### Output parameters

The following attributes are exported:

* `pi_instance_id` - (string) The unique identifier of the instance.
* `pi_disk_size` - (int) The size (GB) of the root disk.
* `pi_instance_status` - (string) The status of the VM.
* `pi_minproc` - (float) The minimum number of processors the VM can have.
* `pi_addresses` - (Required, list(map[string])) A list of addresses assigned to the VM.
* `pi_instance_progress` - (float) Specifies the overall progress of the VM deployment process.

### Timeouts

ibm_pi_instance provides the following [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating an instance.
* `delete` - (Default 60 minutes) Used for deleting an instance.




## ibm_pi_key

Provides a SSH key resource. This allows SSH Keys to be created, updated, and cancelled in the Power Virtual Server Cloud.

### Sample Terraform code

In the following example, you can create a ssh key to be used during creation of a pvminstance:

```hcl
resource "ibm_pi_key" "testacc_sshkey" {
  pi_key_name          = "testkey"
  pi_ssh_key           = "ssh-rsa <value>"
  pi_cloud_instance_id = "<value of the cloud_instance_id>"
}
```



The following arguments are supported:

* `pi_key_name` - (Required, int) The key name.
* `pi_ssh_key` - (Required, string) The value of the ssh key.
* `pi_cloud_instance_id` - (Required, string) The cloud_instance_id for this account.

### Output parameters

The following attributes are exported:

None

### Timeouts

ibm_pi_key provides the following [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating a SSH key.
* `delete` - (Default 60 minutes) Used for deleting a SSH key.




## ibm_pi_network

Provides a network resource. This allows network to be created, updated and deleted.

### Sample Terraform code

In the following example, you can create a network:

```hcl
resource "ibm_pi_network" "power_networks" {
  count                = 1
  pi_network_name      = "power-network"
  pi_cloud_instance_id = "<value of the cloud_instance_id>"
}
```



The following arguments are supported:

* `pi_network_name` - (Required, string) The name of the network.
* `pi_network_dns` - (Required, list(strings)) List of DNS entries for the network.
* `pi_network_cidr` - (Required, string) The network CIDR.
* `pi_network_type` - (Required, string) The type of network (e.g., pub-vlan, vlan).
* `pi_network_gateway` - (Optional, string) The network gateway address.
* `pi_network_available_ip_count` - (Optional, float) The number of available IP addresses.
* `pi_network_used_ip_count` - (Optional, float) The number of used IP addresses.
* `pi_network_used_ip_percent` - (Optional, float) The percentage of IP addresses used.
* `pi_cloud_instance_id` - (Required, string) The cloud_instance_id for this account.

### Output parameters

The following attributes are exported:

* `networkid` - The unique identifier (string) of the network.
* `vlanid` - The unique identifier (int) of the network VLAN.

### Timeouts

ibm_pi_network provides the following [timeout](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating a network.
* `delete` - (Default 60 minutes) Used for deleting a network.




## ibm_pi_volume

Provides a volume resource. This allows volume to be created, updated, and cancelled in the Power Virtual Server Cloud.

### Sample Terraform code

In the following example, you can create a volume:

```hcl
resource "ibm_pi_volume" "testacc_volume"{
  pi_volume_size       = 20
  pi_volume_name       = test-volume
  pi_volume_type       = ssd
  pi_volume_shareable  = true
  pi_cloud_instance_id = "<value of the cloud_instance_id>"
}
```



The following arguments are supported:

* `pi_volume_size` - (Required, int) The size for this volume.
* `pi_volume_name` - (Required, string) The name of this volume.
* `pi_volume_type` - (Required, string) The volume type - Only two values are supported (ssd/standard).
* `pi_volume_shareable` - (Required, boolean) If the volume can be shared or not (true/false).
* `pi_cloud_instance_id` - (Required, string) The cloud_instance_id for this account.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the volume.
* `status` - The status of the volume.

### Timeouts

ibm_pi_volume provides the following [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for Creating Volume.
* `delete` - (Default 60 minutes) Used for Deleting Volume.

