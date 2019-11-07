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

# Container Resources
{: #container-resources}


## ibm_container_alb

Create, update or delete a application load balancer. 

### Sample Terraform code

In the following example, you can configure a alb:

```hcl
resource ibm_container_alb alb {
  alb_id = "public-cr083d810e501d4c73b42184eab5a7ad56-alb"
  enable = true
}

```



The following arguments are supported:

* `alb_id` - (Required, string) The ALB ID.
* `enable` - (Optional, bool)  Enable an ALB for the cluster.
* `disable_deployment` - (Optional, bool) Disable the ALB deployment only. If provided, the ALB deployment is deleted but the IBM-provided Ingress subdomain remains. 
**Note** - Must include either 'enable' or 'disable_deployment' in the configuration, but must not include both.
* `user_ip` - (Optional,string) For a private ALB only. The private ALB is deployed with an IP address from a user-provided private subnet. If no IP address is provided, the ALB is deployed with a random IP address from a private subnet in the IBM Cloud account.
* `region` - (Optional, string) The region of ALB.

### Output parameters

The following attributes are exported:

* `id` - The ALB ID.
* `alb_type` - The ALB type.
* `cluster` - The name of the cluster.
* `name` - The name of the ALB.
### Timeouts

ibm_container_alb provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 5 minutes) Used for creating Instance.




## ibm_container_alb_cert

Create, update or delete a Application load balancer certificate. 

### Sample Terraform code

In the following example, you can configure a alb:

```hcl
resource ibm_container_alb_cert cert {
  cert_crn    = "crn:v1:bluemix:public:cloudcerts:us-south:a/e9021a4dc47e3d:faadea8e-a7f4-408f-8b39-2175ed17ae62:certificate:3f2ab474fbbf9564582"
  secret_name = "test-sec"
  cluster     = "myCluster"
}

```



The following arguments are supported:

* `cert_crn` - (Required, string) The certificate CRN.
* `cluster_id` - (Required, string)  The cluster ID.
* `secret_name` - (Required, string) The name of the ALB certificate secret. 
* `region` - (Optional, string) The region of ALB certificate.

### Output parameters

The following attributes are exported:

* `id` - The ALB cert ID. The id is composed of \<cluster_name_id\>/\<secret_name\>.<br/>
* `domain_name` - The Domain name of the certificate.
* `expires_on` - The Expiry date of the certificate.
* `issuer_name` - The Issuer name of the certificate.
* `cluster_crn` - The cluster crn.
* `cloud_cert_instance_id` - Cloud Certificate instance ID from which certificate is downloaded.

### Import

ibm_container_alb_cert can be imported using cluster_id, secret_name eg

```
$ terraform import ibm_container_alb_cert.example 166179849c9a469581f28939874d0c82/mysecret
### Timeouts

ibm_container_alb_cert provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 5 minutes) Used for creating Instance.
* `update` - (Default 5 minutes) Used for updating Instance.




## ibm_container_bind_service

Bind an IBM service to a Kubernetes namespace. With this resource, you can attach an existing service to an existing Kubernetes cluster.

### Sample Terraform code

In the following example, you can bind a service to a cluster:

```hcl
resource "ibm_container_bind_service" "bind_service" {
  cluster_name_id             = "cluster_name"
  service_instance_name       = "service_name"
  namespace_id                = "default"
}
```

### Input parameters

The following arguments are supported:

* `cluster_name_id` - (Required, string) The name or ID of the cluster.
* `service_instance_space_guid` - (Removed) The space GUID associated with the service instance. This attribute is removed.
* `service_instance_name_id` - (Removed) The name or ID of the service that you want to bind to the cluster. This attribute is removed, use `service_instance_id` or `service_instance_name` instead.
* `namespace_id` - (Required, string) The Kubernetes namespace.
* `service_instance_name` - (Optional, string) The name of the service that you want to bind to the cluster. Conflicts with `service_instance_id`.
* `service_instance_id` - (Optional, string) The ID of the service that you want to bind to the cluster. Conflicts with `service_instance_name`.
* `key` - (Optional, string) Specify an existing service key to use for the service binding.
* `role` - (Optional, string) Specify the IAM role for the service key. This flag does not work if you specify an existing key to use or for services that are not IAM-enabled, such as Cloud Foundry services.
* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization associated with the cluster. You can retrieve the value from data source `ibm_org` or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space associated with the cluster. You can retrieve the value from data source `ibm_space` or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account associated with the cluster. You can retrieve the value from data source `ibm_account` or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.
* `tags` - (Optional, array of strings) Tags associated with the container bind service instance.  
  **NOTE**: `Tags` are managed locally and not stored on the IBM Cloud service endpoint at this moment.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the bind service resource. The id is composed of \<cluster_name_id\>/\<service_instance_name or service_instance_id\>/\<namespace_id/>
* `namespace_id` -  The Kubernetes namespace.
* `secret_name` - The secret name. This field is removed.

### Import

ibm_container_bind_service can be imported using cluster_name_id, service_instance_name or service_instance_id and namespace_id, eg

```
$ terraform import ibm_container_bind_service.example mycluster/myservice/default


## ibm_container_cluster

Create, update, or delete a Kubernetes cluster. An existing subnet can be attached to the cluster by passing the subnet ID. A webhook can be registered to a cluster. By default, your single zone cluster is set up with a worker pool that is named default.
During the creation of cluster the workers are created with the kube version by default. 

**Before updating the version of cluster and workers via terraform get the list of available updates and their pre and post update instructions at https://cloud.ibm.com/docs/containers/cs_versions.html#version_types. Also please go through the instructions at https://cloud.ibm.com/docs/containers/cs_cluster_update.html#update.
_Users must read these docs carefully before updating the version via terraform_.**

Note: The previous cluster setup of stand-alone worker nodes is supported, but deprecated. Clusters now have a feature called a worker pool, which is a collection of worker nodes with the same flavor, such as machine type, CPU, and memory. Use ibm_container_worker_pool and ibm_container_worker_pool_zone attachment resources to make changes to your cluster, such as adding zones, adding worker nodes, or updating worker nodes.

### Sample Terraform code

In the following example, you can create a Kubernetes cluster with a default worker pool with one worker:

```hcl
resource "ibm_container_cluster" "testacc_cluster" {
  name            = "test"
  datacenter      = "dal10"
  machine_type    = "free"
  hardware        = "shared"
  public_vlan_id  = "vlan"
  private_vlan_id = "vlan"
  subnet_id       = ["1154643"]

  default_pool_size      = 1

  webhook = [{
    level = "Normal"
    type = "slack"
    url = "https://hooks.slack.com/services/yt7rebjhgh2r4rd44fjk"
  }]

}
```

Create the Kubernetes cluster with a default worker pool with 2 workers and one standalone worker as mentioned by worker_num:

```hcl
resource "ibm_container_cluster" "testacc_cluster" {
  name            = "test"
  datacenter      = "dal10"
  machine_type    = "free"
  hardware        = "shared"
  public_vlan_id  = "vlan"
  private_vlan_id = "vlan"
  subnet_id       = ["1154643"]

  default_pool_size = 2
  worker_num = 1
  webhook = [{
    level = "Normal"
    type = "slack"
    url = "https://hooks.slack.com/services/yt7rebjhgh2r4rd44fjk"
  }]

}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the cluster.
* `datacenter` - (Required, string)  The datacenter of the worker nodes. You can retrieve the value by running the `bluemix cs locations` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `kube_version` - (Optional, string) The desired Kubernetes version of the created cluster. If present, at least major.minor must be specified.
* `update_all_workers` - (Optional, bool)  Set to `true` if you want to update workers kube version along with the cluster kube_version
* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization associated with the cluster. You can retrieve the value from data source `ibm_org` or by running the `ibmcloud iam orgs --guid` command in the IBM Cloud CLI.
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space associated with the cluster. You can retrieve the value from data source `ibm_space` or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account associated with the cluster. You can retrieve the value from data source `ibm_account` or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.
* `workers` - (Removed) The worker nodes that you want to add to the cluster. Nested `workers` blocks have the following structure:
	* `action` - valid actions are add, reboot and reload.
	* `name` - Name of the worker.
	* `version` - worker version.
  
	**NOTE**: Conflicts with `worker_num`. 
* `worker_num` - (Optional, int)  The number of cluster worker nodes. This creates the stand-alone workers which are not associated to any pool.  
	**NOTE**: Conflicts with `workers`. 
* `workers_info` - (Optional, array) The worker nodes attached to this cluster. Use this attribute to update the worker version. Nested `workers_info` blocks have the following structure:
	* `id` - ID of the worker.
	* `version` - worker version. 
* `default_pool_size` - (Optional,int) The number of workers created under the default worker pool which support Multi-AZ. 
* `machine_type` - (Optional, string) The machine type of the worker nodes. You can retrieve the value by running the `ibmcloud ks machine-types <data-center>` command in the IBM Cloud CLI.
* `billing` - (Optional, string) The billing type for the instance. Accepted values are `hourly` or `monthly`.
* `isolation` - (Removed) Accepted values are `public` or `private`. Use `private` if you want to have available physical resources dedicated to you only or `public` to allow physical resources to be shared with other IBM customers. Use hardware instead.
* `hardware` - (Optional, string) The level of hardware isolation for your worker node. Use `dedicated` to have available physical resources dedicated to you only, or `shared` to allow physical resources to be shared with other IBM customers. For IBM Cloud Public accounts, it can be shared or dedicated. For IBM Cloud Dedicated accounts, dedicated is the only available option.
* `public_vlan_id`- (Optional, string) The public VLAN ID for the worker node. You can retrieve the value by running the ibmcloud ks vlans <data-center> command in the IBM Cloud CLI.
  * Free clusters: You must not specify any public VLAN. Your free cluster is automatically connected to a public VLAN that is owned by IBM.
  * Standard clusters:  
    (a) If you already have a public VLAN set up in your IBM Cloud Classic Infrastructure (SoftLayer) account for that zone, enter the ID of the public VLAN.<br/>
    (b) If you want to connect your worker nodes to a private VLAN only, do not specify this option.

* `private_vlan_id` - (Optional, string) The private VLAN of the worker node. You can retrieve the value by running the ibmcloud ks vlans <data-center> command in the IBM Cloud CLI.
  * Free clusters: You must not specify any private VLAN. Your free cluster is automatically connected to a private VLAN that is owned by IBM.
  * Standard clusters:<br/>
    (a) If you already have a private VLAN set up in your IBM Cloud Classic Infrastructure (SoftLayer) account for that zone, enter the ID of the private VLAN.<br/>
    (b) If you do not have a private VLAN in your account, do not specify this option. IBM Cloud Kubernetes Service will automatically create a private VLAN for you.
* `subnet_id` - (Optional, string) The existing subnet ID that you want to add to the cluster. You can retrieve the value by running the `ibmcloud ks subnets` command in the IBM Cloud CLI.
* `no_subnet` - (Optional, boolean) Set to `true` if you do not want to automatically create a portable subnet.
* `is_trusted` - (Optional, boolean) Set to `true` to  enable trusted cluster feature. Default is false.
* `disk_encryption` - (Optional, boolean) Set to `false` to disable encryption on a worker.
* `webhook` - (Optional, string) The webhook that you want to add to the cluster.
* `public_service_endpoint` - (Optional,bool) Enable the public service endpoint to make the master publicly accessible.
* `private_service_endpoint` - (Optional,bool) Enable the private service endpoint to make the master privately accessible. Once enabled this feature cannot be disabled later.
  **NOTE**: As a prerequisite for using Service Endpoints, Account must be enabled for Virtual Routing and Forwarding (VRF). Learn more about VRF on IBM Cloud [here](https://cloud.ibm.com/docs/infrastructure/direct-link/vrf-on-ibm-cloud.html#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud). Account must be enabled for connectivity to Service Endpoints. Use the resource `ibm_container_cluster_feature` to update the `public_service_endpoint` and `private_service_endpoint`. 
* `wait_time_minutes` - (Optional, integer) The duration, expressed in minutes, to wait for the cluster to become available before declaring it as created. It is also the same amount of time waited for no active transactions before proceeding with an update or deletion. The default value is `90`.
* `tags` - (Optional, array of strings) Tags associated with the container cluster instance.  
  **NOTE**: For users on account to add tags to a resource, they must be assigned the appropriate access. Learn more about tags permission [here](https://cloud.ibm.com/docs/resources?topic=resources-access)

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the cluster.
* `name` - The name of the cluster.
* `server_url` - The server URL.
* `ingress_hostname` - The Ingress hostname.
* `ingress_secret` - The Ingress secret.
* `subnet_id` - The subnets attached to this cluster.
* `workers` -  Exported attributes are:
	* `id` - The id of the worker
* `worker_pools` - Worker pools attached to the cluster
  * `name` - The name of the worker pool.
  * `machine_type` - The machine type of the worker node.
  * `size_per_zone` - Number of workers per zone in this pool.
  * `hardware` - The level of hardware isolation for your worker node.
  * `id` - Worker pool id.
  * `state` - Worker pool state.
  * `labels` - Labels on all the workers in the worker pool.
	* `zones` - List of zones attached to the worker_pool.
		* `zone` - Zone name.
		* `private_vlan` - The ID of the private VLAN. 
		* `public_vlan` - The ID of the public VLAN.
		* `worker_count` - Number of workers attached to this zone.
* `workers_info` - The worker nodes attached to this cluster. Nested `workers_info` blocks have the following structure:
	* `pool_name` - Name of the worker pool to which the worker belongs to.
* `albs` - Application load balancer (ALB)'s attached to the cluster
  * `id` - The application load balancer (ALB) id.
  * `name` - The name of the application load balancer (ALB).
  * `alb_type` - The application load balancer (ALB) type public or private.
  * `enable` -  Enable (true) or disable(false) application load balancer (ALB).
  * `state` - The status of the application load balancer (ALB)(enabled or disabled).
  * `num_of_instances` - Desired number of application load balancer (ALB) replicas.
  * `alb_ip` - BYOIP VIP to use for application load balancer (ALB). Currently supported only for private application load balancer (ALB).
  * `resize` - Indicate whether resizing should be done.
  * `disable_deployment` - Indicate whether to disable deployment only on disable application load balancer (ALB).
* `private_service_endpoint_url` - Private service endpoint url.
* `public_service_endpoint_url` - Public service endpoint url.
* `crn` - CRN of the instance.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this cluster.


## ibm_container_cluster_feature

Enables or disables a container cluster feature. 

### Sample Terraform code

In the following example, you can enable a private service endpoint:

```hcl
resource ibm_container_cluster_feature feature {
  cluster                 = "test1"
  private_service_endpoint = "true"
}

```



The following arguments are supported:

* `cluster` - (Required, string) The cluster name or id.
* `public_service_endpoint` - (Optional, bool)  Enable or disable the public service endpoint.
* `private_service_endpoint` - (Optional, bool) Enable the private service endpoint to make the master privately accessible. Once enabled this feature cannot be disabled later.
  **NOTE**: As a prerequisite for using Service Endpoints, Account must be enabled for Virtual Routing and Forwarding (VRF). Learn more about VRF on IBM Cloud [here](https://cloud.ibm.com/docs/infrastructure/direct-link/vrf-on-ibm-cloud.html#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud). Account must be enabled for connectivity to Service Endpoints.
* `refresh_api_servers` - (Optional, bool) To apply these changes, refresh the cluster's API server. Default value is true.
* `reload_workers` - (Optional, bool) To apply these changes, reload workers. Default value is true.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.

### Output parameters

The following attributes are exported:

* `id` - The cluster feature ID.
* `public_service_endpoint_url` - Public service endpoint url.
* `private_service_endpoint_url` - Private service endpoint url.
### Timeouts

ibm_container_cluster_feature provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 90 minutes) Used for creating Instance.
* `update` - (Default 90 minutes) Used for updating Instance.




## ibm_container_worker_pool

Create, update, or delete a worker pool. The worker pool will be attached to the specified cluster.


### Sample Terraform code

In the following example, you can create a worker pool:

```hcl
resource "ibm_container_worker_pool" "testacc_workerpool" {
  worker_pool_name = "terraform_test_pool"
  machine_type     = "u2c.2x4"
  cluster          = "my_cluster"
  size_per_zone    = 1
  hardware         = "shared"
  disk_encryption  = "true"
  region           = "eu-de"

  labels = {
    "test" = "test-pool"
  }

  //User can increase timeouts 
    timeouts {
      update = "180m"
    }
}
```



The following arguments are supported:

* `name` - (Required, string) The name of the worker pool.
* `cluster` - (Required, string) The name or id of the cluster.
* `machine_type` - (Required, string) The machine type of the worker node.
* `size_per_zone` - (Required, int) Number of workers per zone in this pool.
* `hardware` - (Optional, string) The level of hardware isolation for your worker node. Use `dedicated` to have available physical resources dedicated to you only, or `shared` to allow physical resources to be shared with other IBM customers. For IBM Cloud Public accounts, the default value is shared. For IBM Cloud Dedicated accounts, dedicated is the only available option.
* `disk_encryption` - (Optional, boolean) Set to `false` to disable encryption on a worker. Default is true.
* `labels` - (Optional, map) Labels on all the workers in the worker pool.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.
 
### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the worker pool resource. The id is composed of \<cluster_name_id\>/\<worker_pool_id\>.<br/>
**Note**:To reference the worker pool id in other resources use below interpolation syntax.<br/>
`Ex: ${element(split("/",ibm_container_worker_pool.testacc_workerpool.id),1)}`
* `state` - Worker pool state.
* `zones` - List of zones attached to the worker_pool.
   * `zone` - Zone name.
   * `private_vlan` - The ID of the private VLAN.
   * `public_vlan` - The ID of the public VLAN.
   * `worker_count` - Number of workers attached to this zone.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this cluster.

### Import

ibm_container_worker_pool can be imported using cluster_name_id, worker_pool_id eg

```
$ terraform import ibm_container_worker_pool.example mycluster/5c4f4d06e0dc402084922dea70850e3b-7cafe35

### Timeouts

ibm_container_worker_pool provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `update` - (Default 90 minutes) Used for updating Instance.




## ibm_container_worker_pool_zone_attachment

Create, update, or delete a zone. This resource creates the zone and attaches it to the specified worker pool.

### Sample Terraform code

In the following example, you can create a zone:

```hcl
resource "ibm_container_worker_pool" "test_pool" {
  worker_pool_name = "my_pool"
  machine_type     = "u2c.2x4"
  cluster          = "my_cluster"
  size_per_zone    = 2
  hardware         = "shared"
  disk_encryption  = "true"
  labels = {
    "test" = "test-pool"

    "test1" = "test-pool1"
  }
}
resource "ibm_container_worker_pool_zone_attachment" "test_zone" {
  cluster         = "my_cluster"
  worker_pool     = "${element(split("/",ibm_container_worker_pool.test_pool.id),1)}"
  zone            = "dal12"
  private_vlan_id = "2320267"
  public_vlan_id  = "2320265"

  //User can increase timeouts
  timeouts {
      create = "90m"
      update = "3h"
      delete = "30m"
    }
}

```



The following arguments are supported:

* `zone` - (Required, string) The name of the zone. To list available zones, run `ibmcloud ks zones`
* `cluster` - (Required, string) The name or id of the cluster.
* `worker_pool` - (Required, string) The name or id of the worker pool.
* `private_vlan_id` - (Optional, string) The private VLAN of the worker node. You can retrieve the value by running the `ibmcloud ks vlans <zone>` command in the IBM Cloud CLI.
* `public_vlan_id` - (Optional, string) The public VLAN of the worker node. The public vlan id cannot be specified alone, it should be specified along with the private vlan id. You can retrieve the value by running the `ibmcloud ks vlans <zone>` command in the IBM Cloud CLI.
**Note**: If you do not have a private or public VLAN in that zone, do not specify `private_vlan_id` and `public_vlan_id`. A private and a public VLAN are automatically created for you when you initially add a new zone to your worker pool.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the worker pool zone attachment resource. The id is composed of \<cluster_name_id\>/\< worker_pool_name_id\>/\<zone/>
* `worker_count` - Number of workers attached to this zone.

### Import

ibm_container_worker_pool_zone_attachment can be imported using cluster_name_id, worker_pool_name_id and zone, eg

```
$ terraform import ibm_container_worker_pool_zone_attachment.example mycluster/5c4f4d06e0dc402084922dea70850e3b-7cafe35/dal10

### Timeouts

ibm_container_worker_pool_zone_attachment provides the following [Timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) configuration options:

* `create` - (Default 90 minutes) Used for creating Instance.
* `update` - (Default 90 minutes) Used for updating Instance.
* `delete` - (Default 90 minutes) Used for deleting Instance.

