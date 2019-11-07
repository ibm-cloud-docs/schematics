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

# Container Data Sources
{: #container-data-sources}


## ibm_container_cluster


Import the details of a Kubernetes cluster on IBM Cloud as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
data "ibm_container_cluster" "cluster_foo" {
  cluster_name_id = "FOO"
}
```

### Input parameters

The following arguments are supported:

* `cluster_name_id` - (Required, string) The name or ID of the cluster.
* `alb_type` - (Optional, string) Used to filter the albs based on type. Valid values are `private`, `public` and `all`. The default value is `all`.
* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization associated with the cluster. You can retrieve the value from the `ibm_org` data source or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space associated with the cluster. You can retrieve the value from the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account associated with the cluster. You can retrieve the value from the `ibm_account` data source or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the cluster.
* `worker_count` - The number of workers that are attached to the cluster.
* `workers` - The IDs of the workers that are attached to the cluster.
* `bounded_services` - The services that are bounded to the cluster.
* `is_trusted` - Is trusted cluster feature enabled.
* `vlans` - The VLAN'S that are attached to the cluster. Nested `vlans` blocks have the following structure:
	* `id` - The VLAN id.
	* `subnets` - The list of subnets attached to VLAN belonging to the cluster. Nested `subnets` blocks have the following structure:
		* `id` - The Subnet Id.
		* `cidr` - The cidr range.
		* `ips` - The list of ip's in the subnet.
		* `is_byoip` - `true` if user provides a ip range else `false`.
		* `is_public` - `true` if VLAN is public else `false`.
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
* `public_service_endpoint` - Is public service endpoint enabled to make the master publicly accessible.
* `private_service_endpoint` - Is private service endpoint enabled to make the master privately accessible.
* `crn` - CRN of the instance.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this cluster.


## ibm_container_cluster_config


Download a configuration for Kubernetes clusters on IBM Cloud. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
data "ibm_container_cluster_config" "cluster_foo" {
  cluster_name_id = "FOO"
  config_dir      = "/home/foo_config"
}
```

### Input parameters

The following arguments are supported:

* `cluster_name_id` - (Required, string) The name or ID of the cluster.
* `config_dir` - (Required, string) The directory where you want the cluster configuration to download.
* `admin` - (Optional, boolean) Set the value to `true` to download the configuration for the administrator. The default value is `false`.
* `download` - (Optional, boolean) Set the value to `false` to skip downloading the configuration for the administrator. The default value is `true`. Because it is part of a data source, by default the configuration is downloaded for every Terraform call. For a particular cluster name or ID, the configuration is guaranteed to be downloaded to the same path for a given `config_dir`.
* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization associated with the cluster. You can retrieve the value from the `ibm_org` data source or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space associated with the cluster. You can retrieve the value from the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account associated with the cluster. You can retrieve the value from the `ibm_account` data source or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region where the cluster is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `network` - (Optional, boolean) Set the value to `true` to download the configuration for the Calico network config with the Admin config. The default value is `false`.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the cluster configuration.
* `config_file_path` - The path to the cluster configuration file. This is typically the Kubernetes YAML configuration file.
* `calico_config_file_path` - The path to the cluster calico configuration file.


## ibm_container_cluster_worker


Import details of a worker node of a Kubernetes cluster as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
data "ibm_container_cluster_worker" "cluster_foo" {
  worker_id    = "dev-mex10-pa70c4414695c041518603bfd0cd6e333a-w1"
}
```

### Input parameters

The following arguments are supported:

* `worker_id` - (Required, string) The ID of the worker node attached to the cluster.
* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization that the cluster is associated with. You can retrieve the value from the `ibm_org` data source or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space that the cluster is associated with. You can retrieve the value from the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account that the cluster is associated with. You can retrieve the value from the `ibm_account` data source or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region where the worker is provisioned. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.

### Output parameters

The following attributes are exported:

* `state` - The unique identifier of the cluster.
* `status` - The number of workers nodes attached to the cluster.
* `private_vlan` - The private VLAN of the worker node.
* `public_vlan` -  The public VLAN of the worker node.
* `private_ip` - The private IP of the worker node.
* `public_ip` -  The public IP of the worker node.
* `resource_controller_url` - The URL of the IBM Cloud dashboard that can be used to explore and view details about this cluster.



## ibm_container_cluster_versions

Get the list of supported kubernetes versions on IBM Cloud. Please refer to https://cloud.ibm.com/docs/containers/cs_versions.html#cs_versions for detail instructions.

### Sample Terraform code

```hcl
data "ibm_container_cluster_versions" "cluster_versions" {
  region          = "eu-de"
}
```

### Input parameters

The following arguments are supported:

* `org_guid` - (Deprecated, string) The GUID for the IBM Cloud organization associated with the cluster. You can retrieve the value from the `ibm_org` data source or by running the `ibmcloud iam orgs --guid` command in the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started).
* `space_guid` - (Deprecated, string) The GUID for the IBM Cloud space associated with the cluster. You can retrieve the value from the `ibm_space` data source or by running the `ibmcloud iam space <space-name> --guid` command in the IBM Cloud CLI.
* `account_guid` - (Deprecated, string) The GUID for the IBM Cloud account associated with the cluster. You can retrieve the value from the `ibm_account` data source or by running the `ibmcloud iam accounts` command in the IBM Cloud CLI.
* `region` - (Deprecated, string) The region to target. If the region is not specified it will be defaulted to provider region(IC_REGION/IBMCLOUD_REGION). To get the list of supported regions please access this [link](https://containers.bluemix.net/v1/regions) and use the alias.
* `resource_group_id` - (Optional, string) The ID of the resource group.  You can retrieve the value from data source `ibm_resource_group`. If not provided defaults to default resource group.

### Output parameters

The following attributes are exported:

* `id` - The unique identifier of the cluster versions.
* `valid_kube_versions` - The supported kubernetes versions on IBM Cloud.
* `valid_openshift_versions` - The supported openshift versions on IBM Cloud.