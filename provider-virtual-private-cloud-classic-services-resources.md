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

# Virtual Private Cloud Classic Services Resources
{: #virtual-private-cloud-classic-services-resources}


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




## ibm_is_ike_policy

Provides a ike policy resource. This allows ike policy to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a ike policy:

```hcl
resource "ibm_is_ike_policy" "example" {
	name = "test"
	authentication_algorithm = "md5"
	encryption_algorithm = "3des"
	dh_group = 2
	ike_version = 1
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the IKE policy.
* `authentication_algorithm` - (Required, string)  The authentication algorithm. Enumeration type: md5, sha1, sha256.
* `encryption_algorithm` - (Required, string) The encryption algorithm. Enumeration type: 3des, aes128, aes256.
* `dh_group` - (Required, int) The Diffie-Hellman group. Enumeration type: 2, 5, 14.
* `ike_version` - (Optional,int) The IKE protocol version. Enumeration type: 1, 2.
* `key_lifetime` - (Optional, int) The key lifetime in seconds. Maximum: 86400, Minimum: 300. Default is 28800.
* `resource_group` - (Optional, string) The resource group ID where the ike policy to be created.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the IKE Policy.
* `negotiation_mode` - The IKE negotiation mode. Only main is supported.
* `href` - The IKE policy's canonical URL.
* `vpn_connections` - Collection of references to VPN connections that use this IKE policy. Nested connections is
	* `name` - The name given to this VPN connection.
	* `id` -  The unique identifier of a VPN connection.
	* `href` - The VPN connection's canonical URL.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.



## ibm_is_ipsec_policy

Provides a ipsec policy resource. This allows ipsec policy to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a ip sec policy:

```hcl
resource "ibm_is_ipsec_policy" "example" {
	name = "test"
	authentication_algorithm = "md5"
	encryption_algorithm = "3des"
	pfs = "disabled"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the IPsec policy.
* `authentication_algorithm` - (Required, string)  The authentication algorithm. Enumeration type: md5, sha1, sha256.
* `encryption_algorithm` - (Required, string) The encryption algorithm. Enumeration type: 3des, aes128, aes256.
* `pfs` - (Required, string) Perfect Forward Secrecy. Enumeration type: disabled, group_2, group_5, group_14.
* `key_lifetime` - (Optional, int) The key lifetime in seconds. Maximum: 86400, Minimum: 300. Default is 3600.
* `resource_group` - (Optional, string) The resource group ID where the ip sec policy to be created.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the Ip Sec Policy.
* `encapsulation_mode` - The encapsulation mode used. Only tunnel is supported.
* `transform_protocol` - The transform protocol used. Only esp is supported.
* `vpn_connections` - Collection of references to VPN connections that use this IPsec policy. Nested connections is
	* `name` - The name given to this VPN connection.
	* `id` -  The unique identifier of a VPN connection.
	* `href` - The VPN connection's canonical URL.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_ipsec_policy can be imported using ID, eg

```
$ terraform import ibm_is_ipsec_policy.example d7bec597-4726-451f-8a63-e62e6f19c32c
```


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


## ibm_is_vpc_route

Provides a vpc route resource. This allows vpc route to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_vpc" "testacc_vpc" {
  name = "testvpc"
}

resource "ibm_is_vpc_route" "testacc_vpc_route" {
  name        = "routetest"
  vpc         = "${ibm_is_vpc.testacc_vpc.id}"
  zone        = "us-south-1"
  destination = "192.168.4.0/24"
  next_hop    = "10.0.0.4"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The route name.
* `vpc` - (Required, string) The vpc id. 
* `zone` - (Required, string) Name of the zone. 
* `destination` - (Required, string) The destination of the route. 
* `next_hop` - (Required, string) The next hop of the route. 

### Output parameters

The following attributes are exported:

* `id` - The id of the route. The id is composed of \<vpc_id\>/\<vpc_route_id\>
* `status` - The status of the VPC Route.

### Import

ibm_is_vpc_route can be imported using VPC ID and VPC Route ID, eg

```
$ terraform import ibm_is_vpc_route.example 56738c92-4631-4eb5-8938-8af9211a6ea4/fc2667e0-9e6f-4993-a0fd-cabab477c4d1
```


## ibm_is_network_acl

Provides a network ACL resource. This allows network ACL to be created, updated, and cancelled.


### Sample Terraform code

```hcl
resource "ibm_is_network_acl" "isExampleACL" {
			name = "is-example-acl"
			rules=[
			{
				name = "outbound"
				action = "allow"
				protocol = "icmp"
				source = "0.0.0.0/0"
				destination = "0.0.0.0/0"
				direction = "outbound"
				icmp=[
				{
					code = 1
					type = 1
				}]
			},
			{
				name = "inbound"
				action = "allow"
				protocol = "icmp"
				source = "0.0.0.0/0"
				destination = "0.0.0.0/0"
				direction = "inbound"
				icmp=[
				{
					code = 1
					type = 1
				}]
			}
			]
		}
```

### Input parameters

The following arguments are supported:



* `name` - (Required, string) The name of the network ACL.
* `rules` - (Optional, array)   The rules for a network ACL
Nested `rules` blocks have the following structure:
	* `name` - (Required, string) The user-defined name for this rule.
	* `action` - (Required, string) Whether to allow or deny matching traffic.
	* `source` - (Required, string) The source IP address or CIDR block.
	* `destination` - (Required, string) The destination IP address or CIDR block.
	* `direction` - (Required, string) Whether the traffic to be matched is inbound or outbound.
	* `icmp` - (Optional, array) The protocol ICMP
		* `code` - (Optional, int) The ICMP traffic code to allow. Valid values from 0 to 255. If unspecified, all codes are allowed. This can only be specified if type is also specified.
		* `type` - (Optional, int) The ICMP traffic type to allow. Valid values from 0 to 254. If unspecified, all types are allowed by this rule.
	* `tcp` - (Optional, array) TCP protocol.
		* `port_max` - (Optional, int) The highest port in the range of ports to be matched; if unspecified, 65535 is used.
		* `port_min` - (Optional, int) The lowest port in the range of ports to be matched; if unspecified, 1 is used.
	* `udp` - (Optional, array) UDP protocol
		* `port_max` - (Optional, int) The highest port in the range of ports to be matched; if unspecified, 65535 is used.
		* `port_min` - (Optional, int) The lowest port in the range of ports to be matched; if unspecified, 1 is used.
		

### Output parameters

The following attributes are exported:

* `id` - The id of the network ACL.
* `rules` - The rules for a network ACL.
Nested `rules` blocks have the following structure:
	* `id` - The rule id.
	* `ip_version` - The IP version of the rule.
	* `subnets` - The subnets for the ACL rule.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_network_acl can be imported using ID, eg

```
$ terraform import ibm_is_network_acl.example d7bec597-4726-451f-8a63-e62e6f19c32c
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



## ibm_is_vpn_gateway

Provides a VPN gateway resource. This allows VPN gateway to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a VPN gateway:

```hcl
resource "ibm_is_vpn_gateway" "testacc_vpn_gateway" {
    name = "test"
    subnet = "a4ce411d-e118-4802-95ad-525e6ea0cfc9"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) Name of the VPN gateway.
* `subnet` - (Required, string) The unique identifier for this subnet.
* `resource_group` - (Optional, string) The resource group where the VPN gateway to be created.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VPN gateway.
* `status` - The status of VPN gateway.
* `public_ip_address` -  The IP address assigned to this VPN gateway.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_vpn_gateway can be imported using ID, eg

```
$ terraform import ibm_is_vpn_gateway.example d7bec597-4726-451f-8a63-e62e6f19c32c
```


## ibm_is_vpn_gateway_connection

Provides a VPN gateway connection resource. This allows VPN gateway connection to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a VPN gateway:

```hcl
resource "ibm_is_vpn_gateway_connection" "VPNGatewayConnection" {
  name          = "test2"
  vpn_gateway   = "${ibm_is_vpn_gateway.testacc_VPNGateway2.id}"
  peer_address  = "${ibm_is_vpn_gateway.testacc_VPNGateway2.public_ip_address}"
  preshared_key = "VPNDemoPassword"
  local_cidrs   = ["${ibm_is_subnet.testacc_subnet2.ipv4_cidr_block}"]
  peer_cidrs    = ["${ibm_is_subnet.testacc_subnet1.ipv4_cidr_block}"]
}

```



The following arguments are supported:

* `name` - (Required, string) Name of the VPN gateway connection.
* `vpn_gateway` - (Required, string) The unique identifier of VPN gateway.
* `peer_address` - (Required, string) The IP address of the peer VPN gateway.
* `preshared_key`: The preshared key.
* `local_cidrs` - (Optional, list) List of CIDRs for this resource.
* `peer_cidrs` - (Optional, list) List of CIDRs for this resource.
* `admin_state_up` - (Optional, bool) VPN gateway connection status. Default false. If set to false, the VPN gateway connection is shut down
* `action` - (Optional, string) Dead Peer Detection actions. Supported values are restart, clear, hold, none. Default `none`
* `interval` - (Optional, int) Dead Peer Detection interval in seconds. Default 30.
* `timeout` - (Optional, int) Dead Peer Detection timeout in seconds. Default 120.
* `ike_policy` - (Optional, string) ID of the IKE policy.
* `ipsec_policy` - (Optional, string) ID of the IPSec policy.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the VPN gateway connection. The id is composed of \<vpn_gateway_id\>/\<vpn_gateway_connection_id\>.
* `status` - The status of VPN gateway connection.

### Import

ibm_is_vpn_gateway_connection can be imported using vpn gateway ID and vpn gateway connection ID, eg

```
$ terraform import ibm_is_vpn_gateway_connection.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```

### Timeouts

ibm_is_vpn_gateway_connection provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `delete` - (Default 60 minutes) Used for deleting Instance.




## ibm_is_lb

Provides a load balancer resource. This allows load balancer to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a load balancer:

```hcl
resource "ibm_is_lb" "lb" {
    name = "loadbalancer1"
    subnets = ["04813493-15d6-4150-9948-6cc646cb67f2"]
}

```



The following arguments are supported:

* `name` - (Required, string) Name of the loadbalancer.
* `subnets` - (Required, list) ID of the subnets to provision this load balancer.
* `type` - (Optional, string) The type of the load balancer. Default value `public`. Supported values `public` and  `private`.
* `resource_group` - (Optional, string) The resource group where the load balancer to be created.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the load balancer.
* `public_ips` - The public IP addresses assigned to this load balancer.
* `private_ips` - The private IP addresses assigned to this load balancer.
* `status` - The status of load balancer.
* `operating_status` - The operating status of this load balancer.
* `hostname` - Fully qualified domain name assigned to this load balancer.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this instance.


### Import

ibm_is_lb can be imported using lbID, eg

```
$ terraform import ibm_is_lb.example d7bec597-4726-451f-8a63-e62e6f19c32c
```

### Timeouts

ibm_is_lb provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating Instance.
* `delete` - (Default 60 minutes) Used for deleting Instance.




## ibm_is_lb_listener

Provides a load balancer listener resource. This allows load balancer listener to be created, updated, and cancelled.

**Note**: When provisioning the load balancer listener along with load balancer pool or pool member, Use explicit depends on the resources or perform the terraform apply with parallelism 1. For more information on explicit dependencies refer [here](https://learn.hashicorp.com/terraform/getting-started/dependencies#implicit-and-explicit-dependencies)

### Sample Terraform code

In the following example, you can create a load balancer listener along with pool and pool member:

```hcl
resource "ibm_is_lb_listener" "testacc_lb_listener" {
  lb       = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  port     = "9080"
  protocol = "http"
}
resource "ibm_is_lb_pool" "webapptier-lb-pool" {
  lb                 = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  name               = "a-webapptier-lb-pool"
  protocol           = "http"
  algorithm          = "round_robin"
  health_delay       = "5"
  health_retries     = "2"
  health_timeout     = "2"
  health_type        = "http"
  health_monitor_url = "/"
  depends_on = ["ibm_is_lb_listener.testacc_lb_listener"]
}

resource "ibm_is_lb_pool_member" "webapptier-lb-pool-member-zone1" {
  count = "2"
  lb    = "8898e627-f61f-4ac8-be85-9db9d8bfd345"
  pool  = "${element(split("/",ibm_is_lb_pool.webapptier-lb-pool.id),1)}"
  port  = "80"
  target_address = "192.168.0.1"
  depends_on = ["ibm_is_lb_listener.testacc_lb_listener"]
}


```



The following arguments are supported:

* `lb` - (Required, string) The load balancer unique identifier.
* `port` - (Required, int) The listener port number. Valid range 1 to 65535.
* `protocol` - (Required, string) The listener protocol. Enumeration type: http, tcp, https.
* `default_pool` - (Optional, string) The load balancer pool unique identifier.
* `certificate_instance` - (Optional, string) CRN of the certificate instance.
* `connection_limit` - (Optional, int) The connection limit of the listener. Valid range  1 to 15000.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the load balancer listener.
* `status` - The status of load balancer listener.

### Import

ibm_is_lb_listener can be imported using lbID and listenerID, eg

```
$ terraform import ibm_is_lb_listener.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```

### Timeouts

ibm_is_lb_listener provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating Instance.
* `update` - (Default 60 minutes) Used for updating Instance.
* `delete` - (Default 60 minutes) Used for deleting Instance.





## ibm_is_lb_pool

Provides a load balancer pool resource. This allows load balancer pool to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a load balancer pool:

```hcl
resource "ibm_is_lb_pool" "testacc_pool" {
  name           = "test_pool"
  lb             = "addfd-gg4r4-12345"
  algorithm      = "round_robin"
  protocol       = "http"
  health_delay   = 60
  health_retries = 5
  health_timeout = 30
  health_type    = "http"
}

```



The following arguments are supported:

* `name` - (Required, string) The name of the pool
* `lb` - (Required, string)  The load balancer unique identifier.
* `algorithm` - (Required, string) The load balancing algorithm. Enumeration type: round_robin, weighted_round_robin, least_connections
* `protocol` - (Required, string) The pool protocol. Enumeration type: http, tcp
* `health_delay` - (Required, int) The health check interval in seconds. Interval must be greater than timeout value
* `health_retries` - (Required, int) The health check max retries
* `health_timeout` - (Required, int) The health check timeout in seconds
* `health_type` - (Required, string) The pool protocol. Enumeration type: http, tcp
* `health_monitor_url` - (Optional, string) The health check url. This option is applicable only to http type of --health-type
* `health_monitor_port` - (Optional, int) The health check port number
* `session_persistence_type` - (Optional, string) The session persistence type, Enumeration type: source_ip, http_cookie, app_cookie
* `session_persistence_cookie_name` - (Optional, string) Session persistence cookie name. This option is applicable only to --session-persistence-type

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the load balancer pool.The id is composed of \<lb_id\>/\<pool_id\>.
* `provisioning_status` - The status of load balancer pool.

### Import

ibm_is_lb_pool can be imported using lbID and poolID, eg

```
$ terraform import ibm_is_lb_pool.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb
```

### Timeouts

ibm_is_lb_pool provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating Instance.
* `update` - (Default 60 minutes) Used for updating Instance.
* `delete` - (Default 60 minutes) Used for deleting Instance.




## ibm_is_lb_pool_member

Provides a load balancer pool member resource. This allows load balancer pool member to be created, updated, and cancelled.


### Sample Terraform code

In the following example, you can create a load balancer pool member:

```hcl
resource "ibm_is_lb_pool_member" "testacc_lb_mem" {
  lb             = "daac2b08-fe8a-443b-9b06-1cef79922dce"
  pool           = "f087d3bd-3da8-452d-9ce4-c1010c9fec04"
  port           = 8080
  target_address = "127.0.0.1"
  weight		 = 60
}

```



The following arguments are supported:

* `pool` - (Required, string) The load balancer pool unique identifier.
* `lb` - (Required, string)  The load balancer unique identifier.
* `port` - (Required, int) The port number of the application running in the server member.
* `target_address` - (Required, string) The IP address of the pool member.
* `weight` - (Optional, int) Weight of the server member. This option takes effect only when the load balancing algorithm of its belonging pool is weighted_round_robin

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the load balancer pool member.
* `href` - The member's canonical URL.
* `health` - Health of the server member in the pool.

### Import

ibm_is_lb_pool_member can be imported using lbID, poolID and poolmemebrID, eg

```
$ terraform import ibm_is_lb_pool_member.example d7bec597-4726-451f-8a63-e62e6f19c32c/cea6651a-bc0a-4438-9f8a-a0770bbf3ebb/gfe6651a-bc0a-5538-8h8a-b0770bbf32cc
```

### Timeouts

ibm_is_lb_pool_member provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 60 minutes) Used for creating Instance.
* `update` - (Default 60 minutes) Used for updating Instance.
* `delete` - (Default 60 minutes) Used for deleting Instance.




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


