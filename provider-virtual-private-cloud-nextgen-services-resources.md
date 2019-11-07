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

# Virtual Private Cloud NextGen Services Resources
{: #virtual-private-cloud-nextgen-services-resources}


## ibm_is_floating_ip

Provides a floatingip resource. This allows floatingip to be created, updated, and cancelled.


### Sample Terraform code

```hcl

resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
  profile = "b-2x8"

  primary_network_interface = {
    port_speed = "1000"
    subnet     = "70be8eae-134c-436e-a86e-04849f84cb34"
  }

  vpc  = "01eda778-b822-43a2-816d-d30713df5e13"
  zone = "us-south-1"
  keys = ["eac87f33-0c00-4da7-aa66-dc2d972148bd"]
}

resource "ibm_is_floating_ip" "testacc_floatingip" {
  name   = "testfip1"
  target = "${ibm_is_instance.testacc_instance.primary_network_interface.0.id}"
}

```



The following arguments are supported:

* `name` - (Required, string) The floating ip name.
* `target` - (Optional, string) ID of the target network interface.
    **NOTE**: Conflicts with `zone`.
* `zone` - (Optional, string) Name of the target zone. 
    **NOTE**: Conflicts with `target`.

### Output parameters

The following attributes are exported:

* `id` - The id of the floating ip.
* `status` - The status of the floating ip.
* `address` - The floating ip address. 
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.

### Timeouts

ibm_is_instance provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for creating floating IP.
* `delete` - (Default 10 minutes) Used for deleting floating IP.




## ibm_is_instance

Provides a instance resource. This allows instance to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
  name = "testvpc"
}

resource "ibm_is_subnet" "testacc_subnet" {
  name            = "testsubnet"
  vpc             = "${ibm_is_vpc.testacc_vpc.id}"
  zone            = "us-south-1"
  ipv4_cidr_block = "10.240.0.0/24"
}

resource "ibm_is_ssh_key" "testacc_sshkey" {
  name       = "testssh"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
}

resource "ibm_is_instance" "testacc_instance" {
  name    = "testinstance"
  image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
  profile = "b-2x8"

  primary_network_interface = {
    subnet     = "${ibm_is_subnet.testacc_subnet.id}"
  }

  network_interfaces = {
    name       = "eth1"
    subnet     = "${ibm_is_subnet.testacc_subnet.id}"
  }


  vpc  = "${ibm_is_vpc.testacc_vpc.id}"
  zone = "us-south-1"
  keys = ["${ibm_is_ssh_key.testacc_sshkey.id}"]

  //User can configure timeouts
  	timeouts {
      	create = "90m"
      	delete = "30m"
    }
}

```



The following arguments are supported:

* `name` - (Optional, string) The instance name.
* `vpc` - (Required, string) The vpc id. 
* `zone` - (Required, string) Name of the zone. 
* `profile` - (Required, string) The profile name. 
* `image` - (Required, string) ID of the image. 
* `boot_volume` - (Optional, list) A block describing the boot volume of this instance.  
`boot_volume` block have the following structure:
  * `name` - (Optional, string) The name of the boot volume.
  * `encryption` -(Optional, string) The encryption of the boot volume.
* `keys` - (Required, list) Comma separated IDs of ssh keys.  
* `primary_network_interface` - (Required, list) A nested block describing the primary network interface of this instance. We can have only one primary network interface.
Nested `primary_network_interface` block have the following structure:
  * `name` - (Optional, string) The name of the network interface.
  * `port_speed` - (Deprecated, int) Speed of the network interface.
  * `subnet` -  (Required, string) ID of the subnet.
  * `security_groups` - (Optional, list) Comma separated IDs of security groups.
* `network_interfaces` - (Optional, list) A nested block describing the additional network interface of this instance.
Nested `network_interfaces` block have the following structure:
  * `name` - (Optional, string) The name of the network interface.
  * `subnet` -  (Required, string) ID of the subnet.
  * `security_groups` - (Optional, list) Comma separated IDs of security groups.

* `volumes` - (Optional, list) Comma separated IDs of volumes. 
* `user_data` - (Optional, string) User data to transfer to the server instance.
* `resource_group` - (Optional, string) The resource group ID for this instance.

### Output parameters

The following attributes are exported:

* `id` - The id of the instance.
* `memory` - Memory of the instance.
* `status` - Status of the instance.
* `vcpu` - A nested block describing the VCPU configuration of this instance.
Nested `vcpu` blocks have the following structure:
  * `architecture` - The architecture of the instance.
  * `count` - The number of VCPUs assigned to the instance.
* `gpu` - A nested block describing the gpu of this instance.
Nested `gpu` blocks have the following structure:
  * `cores` - The cores of the gpu.
  * `count` - Count of the gpu.
  * `manufacture` - Manufacture of the gpu.
  * `memory` - Memory of the gpu.
  * `model` - Model of the gpu.
* `primary_network_interface` - A nested block describing the primary network interface of this instance.
Nested `primary_network_interface` blocks have the following structure:
  * `id` - The id of the network interface.
  * `name` - The name of the network interface.
  * `subnet` -  ID of the subnet.
  * `security_groups` -  List of security groups.
  * `primary_ipv4_address` - The primary IPv4 address.
* `network_interfaces` - A nested block describing the additional network interface of this instance.
Nested `network_interfaces` blocks have the following structure:
  * `id` - The id of the network interface.
  * `name` - The name of the network interface.
  * `subnet` -  ID of the subnet.
  * `security_groups` -  List of security groups.
  * `primary_ipv4_address` - The primary IPv4 address.
* `boot_volume` - A nested block describing the boot volume.
Nested `boot_volume` blocks have the following structure:
  * `name` - The name of the boot volume.
  * `size` -  Capacity of the volume in GB.
  * `iops` -  Input/Output Operations Per Second for the volume.
  * `profile` - The profile of the volume.
  * `encryption` - The encryption of the boot volume.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_instance can be imported using instanceID, eg

```
$ terraform import ibm_is_instance.example d7bec597-4726-451f-8a63-e62e6f19c32c
```

### Timeouts

ibm_is_instance provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating Instance.
* `delete` - (Default 60 minutes) Used for deleting Instance.




## ibm_is_public_gateway

Provides a public gateway resource. This allows gateway to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_public_gateway" "testacc_gateway" {
	name = "test_gateway"
	vpc = "${ibm_is_vpc.testacc_vpc.id}"
	zone = "us-south-1"

	//User can configure timeouts
  	timeouts {
      	create = "90m"
    }
}
```



The following arguments are supported:

* `name` - (Required, string) The name of the gateway.
* `vpc` - (Required, string) The vpc id.
* `zone` - (Required, string) The gateway zone name.
* `floating_ip` - (Optional, string) A nested block describing the floating IP of this gateway.
Nested `floating_ip` blocks have the following structure:
  * `id` - (Optional, string) ID of the floating ip bound to the public gateway.
  * `address` - (Optional, string) IP address of the floating ip bound to the public gateway. 
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Output parameters

The following attributes are exported:

* `id` - The id of the gateway.
* `status` - The status of the gateway.

### Import

ibm_is_public_gateway can be imported using ID, eg

```
$ terraform import ibm_is_public_gateway.example d7bec597-4726-451f-8a63-e62e6f19c32c
```
### Timeouts

ibm_is_public_gateway provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating public gateway.
* `delete` - (Default 60 minutes) Used for deleting public gateway.




## ibm_is_vpc

Provides a vpc resource. This allows VPC to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a VPC:

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
    name = "test"
}

```

### Input parameters

The following arguments are supported:

* `default_network_acl` - (Optional, string) ID of the default network ACL.
* `is_default` - (Removed, bool) This field is removed.
* `classic_access` -(Optional, bool) Indicates whether this VPC should be connected to Classic Infrastructure. If true, This VPC's resources will have private network connectivity to the account's Classic Infrastructure resources. Only one VPC on an account may be connected in this way. 
* `name` - (Required, string) The name of the VPC.
* `resource_group` - (Optional, string) The resource group ID where the VPC to be created
* `tags` - (Optional, array of strings) Tags associated with the instance.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VPC.
* `default_security_group` - The unique identifier of the VPC default security group.
* `status` - The status of VPC.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_vpc can be imported using ID, eg

```
$ terraform import ibm_is_vpc.example d7bec597-4726-451f-8a63-e62e6f19c32c
```


## ibm_is_vpc_address_prefix

Provides a vpc address prefix resource. This allows vpc address prefix to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
  name = "testvpc"
}

resource "ibm_is_vpc_address_prefix" "testacc_vpc_address_prefix" {
  name = "test"
  zone   = "us-south-1"
  vpc         = "${ibm_is_vpc.testacc_vpc.id}"
  cidr        = "10.240.0.0/24"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The address prefix name.
* `vpc` - (Required, string) The vpc id. 
* `zone` - (Required, string) Name of the zone. 
* `cidr` - (Required, string) The CIDR block for the address prefix. 

### Output parameters

The following attributes are exported:

* `id` - The id of the address prefix.
* `has_subnets` - Indicates whether subnets exist with addresses from this prefix.

### Import

ibm_is_vpc_address_prefix can be imported using ID, eg

```
$ terraform import ibm_is_vpc_address_prefix.example d7bec597-4726-451f-8a63-e62e6f19c32c
```


## ibm_is_security_group

Provides a security group resource. This allows security group to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
	name = "test"
	vpc = "${ibm_is_vpc.testacc_vpc.id}"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Optional, string) The security group name.
* `vpc` - (Required, string) The vpc id. 
* `resource_group` - (Optional, string) The resource group ID where the security group to be created.

### Output parameters

The following attributes are exported:

* `id` - The id of the security group.
* `rules` - A nested block describing the rules of this security group.
Nested `rules` blocks have the following structure:
  * `direction` -  The direction of the traffic either `inbound` or `outbound`.
  * `ip_version` - IP version either `ipv4` or `ipv6`.
  * `remote` - Security group id - an IP address, a CIDR block, or a single security group identifier.
  * `protocol` - The type of the protocol `all`, `icmp`, `tcp`, `udp`. 
  * `type` - The ICMP traffic type to allow.
  * `code` - The ICMP traffic code to allow.
  * `port_max` - The inclusive upper bound of TCP/UDP port range.
  * `port_min` - The inclusive lower bound of TCP/UDP port range. 
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.

   
### Import

ibm_is_security_group can be imported using lbID, eg

```
$ terraform import ibm_is_security_group.example d7bec597-4726-451f-8a63-e62e6f19c32c
```



## ibm_is_security_group_rule

Provides a security group rule resource. This allows security group rule to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a different types of protocol rules `ALL`, `ICMP`, `UDP` and `TCP`.

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
	name = "test"
}

resource "ibm_is_security_group" "testacc_security_group" {
	name = "test"
	vpc = "${ibm_is_vpc.testacc_vpc.id}"
}

resource "ibm_is_security_group_rule" "testacc_security_group_rule_all" {
	group = "${ibm_is_security_group.testacc_security_group.id}"
	direction = "inbound"
	remote = "127.0.0.1"
 }
 
 resource "ibm_is_security_group_rule" "testacc_security_group_rule_icmp" {
	group = "${ibm_is_security_group.testacc_security_group.id}"
	direction = "inbound"
	remote = "127.0.0.1"
	icmp = {
		code = 20
		type = 30
	}

 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_udp" {
	group = "${ibm_is_security_group.testacc_security_group.id}"
	direction = "inbound"
	remote = "127.0.0.1"
	udp = {
		port_min = 805
		port_max = 807
	}
 }

 resource "ibm_is_security_group_rule" "testacc_security_group_rule_tcp" {
	group = "${ibm_is_security_group.testacc_security_group.id}"
	direction = "egress"
	remote = "127.0.0.1"
	tcp = {
		port_min = 8080
		port_max = 8080
	}
 }
```

### Input parameters

The following arguments are supported:

* `group` - (Required, string) The security group id.
* `direction` - (Required, string)  The direction of the traffic either `inbound` or `outbound`.
* `remote` - (Optional, string) Security group id - an IP address, a CIDR block, or a single security group identifier.
* `ip_version` - (Optional, string) IP version either `IPv4` or `IPv6`. Default `IPv4`.
* `icmp` - (Optional, list) A nested block describing the `icmp` protocol of this security group rule.
  * `type` - (Required, int) The ICMP traffic type to allow. Valid values from 0 to 254.
  * `code` - (Optional, int) The ICMP traffic code to allow. Valid values from 0 to 255.
* `tcp` - (Optional, list) A nested block describing the `tcp` protocol of this security group rule.
  * `port_min` - (Required, int) The inclusive lower bound of TCP port range. Valid values are from 1 to 65535.
  * `port_max` - (Required, int) The inclusive upper bound of TCP port range. Valid values are from 1 to 65535.
* `udp` - (Optional, list) A nested block describing the `udp` protocol of this security group rule.
  * `port_min` - (Required, int) The inclusive lower bound of UDP port range. Valid values are from 1 to 65535.
  * `port_max` - (Required, int) The inclusive upper bound of UDP port range. Valid values are from 1 to 65535.

**NOTE**: If any of the `icmp` , `tcp` or `udp` is not specified it creates a rule with protocol `ALL`. 


### Output parameters

The following attributes are exported:

* `id` - The id of the security group rule. The id is composed of \<security_group_id\>/\<security_group_rule_id\>.
* `rule_id` - The unique identifier of the rule.

### Import

ibm_is_security_group_rule can be imported using security group ID and security group rule ID, eg

```
$ terraform import ibm_is_security_group_rule.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```



## ibm_is_security_group_network_interface_attachment

Provides a security group network interface attachment resource. This allows security group network interface attachment to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_security_group_network_interface_attachment" "sgnic" {
  security_group    = "2d364f0a-a870-42c3-a554-000001352417"
  network_interface = "6d6128aa-badc-45c4-bb0e-7c2c1c47be55"
}
```

### Input parameters

The following arguments are supported:

* `security_group` - (Required, string) The security group id.
* `network_interface` - (Required, string) The network interface id. 

### Output parameters

The following attributes are exported:

* `id` - The id of the security group network interface. The id is composed of \<security_group_id\>/\<network_interface_id\>.
* `instance_network_interface` - The instance network interface id.
* `name` - The user-defined name for this network interface.
* `port_speed` - The network interface port speed in Mbp.
* `primary_ipv4_address` - The network interface port speed in Mbp.
* `primary_ipv6_address` - The primary IPv6 address in compressed notation as specified by RFC 5952.
* `secondary_address` - Collection seconary IP addresses.
* `status` - The status of the volume.
* `subnet` - The Subnet id.
* `type` - The type of this network interface as it relates to a instance.
* `security_groups` -  A nested block describing the security groups of this network interface.
Nested `security_groups` blocks have the following structure:
	* `id` - The id of this security group.
	* `crn` - The CRN of this security group.
	* `name` - The name of this security group.

* `floating_ips` - A nested block describing the floating ip's of this network interface.
Nested `floating_ips` blocks have the following structure:
  * `id` - The id of this floating Ip.
  * `crn` - The CRN of this floating Ip.
  * `name` - The name of this floating Ip.
  * `address` - The globally unique IP address

### Import

ibm_is_security_group_network_interface_attachment can be imported using security group ID and network interface ID, eg

```
$ terraform import ibm_is_security_group_network_interface_attachment.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```



## ibm_is_subnet

Provides a subnet resource. This allows subnet to be created, updated, and cancelled.


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

	//User can configure timeouts
  	timeouts {
      	create = "90m"
      	delete = "30m"
    }
}
```



The following arguments are supported:


* `ipv4_cidr_block` - (Optional, string)   The IPv4 range of the subnet.
* `total_ipv4_address_count` - (Optional, string) The total number of IPv4 addresses.
* `ip_version` - (Optional, string) The Ip Version. The default is `ipv4`.
* `name` - (Required, string) The name of the subnet.
* `network_acl` - (Optional, string) The ID of the network ACL for the subnet.
* `public_gateway` - (Optional, string) The ID of the public-gateway for the subnet.
* `vpc` - (Required, string) The vpc id.
* `zone` - (Required, string) The subnet zone name.

### Output parameters

The following attributes are exported:

* `id` - The id of the subnet.
* `ipv6_cidr_block` - The IPv6 range of the subnet.
* `status` - The status of the subnet.
* `available_ipv4_address_count` - The total number of available IPv4 addresses.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.

### Import

ibm_is_subnet can be imported using ID, eg

```
$ terraform import ibm_is_subnet.example d7bec597-4726-451f-8a63-e62e6f19c32c
```
### Timeouts

ibm_is_subnet provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 10 minutes) Used for creating Instance.
* `update` - (Default 10 minutes) Used for creating Instance.
* `delete` - (Default 10 minutes) Used for deleting Instance.




## ibm_is_ssh_key

Provides a ssh key resource. This allows ssh key to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_ssh_key" "isExampleKey" {
	name = "test_key"
	public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The user-defined name for this key.
* `public_key` - (Required, string) The public SSH key.

### Output parameters

The following attributes are exported:

* `id` - The id of the ssh key.
* `fingerprint` -  The SHA256 fingerprint of the public key.
* `length` - The length of this key.
* `type` - The cryptosystem used by this key.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_ssh_key can be imported using ID, eg

```
$ terraform import ibm_is_ssh_key.example d7bec597-4726-451f-8a63-e62e6f19c32c
```



## ibm_is_volume

Provides a volume resource. This allows volume to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a volume:

```hcl
resource "ibm_is_volume" "testacc_volume" {
  name     = "test_volume"
  profile  = "10iops-tier"
  zone     = "us-south-1"
  iops     = 10000
  capacity = 100
}

```



The following arguments are supported:

* `name` - (Required, string) The user-defined name for this volume.
* `profile` - (Required, string) The profile to use for this volume.
* `zone` - (Required, string) The location of the volume.
* `iops` - (Optional, int) The bandwidth for the volume.
* `capacity` - (Optional, int) The capacity of the volume in gigabytes. This defaults to `100`.
* `encryption_key` - (Optional, string) The key to use for encrypting this volume.
* `resource_group` - (Optional, string) The resource group ID for this volume.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the volume.
* `status` - The status of volume.
* `crn` - The CRN for the volume.
### Timeouts

ibm_is_volume provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for Creating Instance.
* `delete` - (Default 60 minutes) Used for Deleting Instance.


