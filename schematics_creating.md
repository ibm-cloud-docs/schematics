---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

keywords: schematics, automation, terraform

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

# Creating a Terraform configuration
{: #configuration}

<div class="p"><div class="note attention"><span class="attentiontitle">Attention: This service is deprecated.</span> All instances of this service are deprecated. Existing instances can be used until they are no longer supported on 01 May 2018. For more information, see the [deprecation announcement blog](https://www.ibm.com/blogs/bluemix/2018/03/retirement-ibm-cloud-schematics/). For migration information, see [Migrating to the {{site.data.keyword.Bluemix_notm}} Terraform Provider](index.html#migrating).</div>
</div>

{:deprecated}

When you create a Terraform configuration, you are codifying the cloud resources that make up your environment.
{:shortdesc}

When you work with {{site.data.keyword.bpshort}}, you write Terraform configurations for your environment in declarative syntax. You state how you want your environment to look, such as scaling an [{{site.data.keyword.containershort}}](../../containers/container_index.html) cluster to have 10 worker nodes in production. The service compares your configuration to the number of worker nodes that Terraform previously created and adds or removes worker nodes as necessary.

Configurations can be written in HashiCorp Configuration Language (HCL) or JSON syntax. See the <a href="https://www.terraform.io/docs/configuration/index.html">Terraform configuration docs <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> for HCL syntax and guidelines on how to write configurations.


## Example
{: #example}

<img src="images/config_clusters.png" width="400" alt="The following configuration file deploys a lite cluster with one worker node." style="width:400px; border-style: none"/>

_Figure 1. Use a Terraform configuration to deploy a lite cluster with one worker node._

The following configuration can be used to provision a single Kubernetes cluster with one worker node in the {{site.data.keyword.containershort}}. To use the configuration, save the code in a source control repository (GitHub or GitLab) with the Terraform file extension `.tf` .

```
provider "ibm" {
  platform_api_key    = "${var.platform_api_key}"
}

resource "ibm_container_cluster" "test_cluster" {
  name         = "test"
  datacenter   = "${var.datacenter}"
  org_guid     = "${var.org_guid}"
  space_guid   = "${var.space_guid}"
  account_guid = "${var.account_guid}"
  machine_type = "free"

  workers = [{
    name   = "worker1"
    action = "add"
  }]
}

variable "platform_api_key" {
  type        = "string"
  description = "Your platform API key. You can run bx iam api-key-create <key name> to create a key."
}

variable "datacenter" {
  type        = "string"
  description = "The data center that you want to deploy your Kubernetes cluster in."
}

variable "org_guid" {
  type        = "string"
  description = "Your {{site.data.keyword.cloud_notm}} org GUID. Run bx iam org <org name> --guid to get the value."
}

variable "space_guid" {
  type        = "string"
  description = "Your {{site.data.keyword.cloud_notm}} space GUID. Run bx iam space <space name> --guid to get the value."
}

variable "account_guid" {
  type        = "string"
  description = "Your {{site.data.keyword.cloud_notm}} account GUID. Run bx iam accounts to get the value."
}
```
{:codeblock}

## Provider
{: #provider}

The `provider` block identifies which cloud provider you want to use with Terraform. You can set the provider to work with {{site.data.keyword.cloud_notm}} resources by providing your platform API key.

In the following sample, the value for the platform API key is obscured as a variable so that you don't check it into source control. The `${var.platform_api_key}` code acts as a placeholder, which is later defined in the `variable "platform_api_key"` block. You can then pass the value for the platform API key when you deploy resources to your environment.

```
provider "ibm" {
  platform_api_key = "${var.platform_api_key}"
}
```
{:screen}

To provision {{site.data.keyword.cloud_notm}} infrastructure (SoftLayer) resources, you can add the `infrastructure_username` and `infrastructure_api_key` attributes to the provider block.

```
provider "ibm" {
  platform_api_key = "${var.platform_api_key}"
  infrastructure_username = "${var.infrastructure_username}"
  infrastructure_api_key = "${var.infrastructure_api_key}"
}
```
{:codeblock}

## Resources
{: #resources}

The `resource` block defines a component of your infrastructure. A single Kubernetes cluster is used in the example, but you can use Terraform to provision resources collectively. For more information about which resources are available, see the <a href="https://ibm-cloud.github.io/tf-ibm-docs/">{{site.data.keyword.cloud_notm}} provider for Terraform <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.

```
resource "ibm_container_cluster" "test_cluster" {
  name         = "test"
  datacenter   = "${var.datacenter}"
  org_guid     = "${var.org_guid}"
  space_guid   = "${var.space_guid}"
  account_guid = "${var.account_guid}"
  machine_type = "free"

  workers = [{
    name   = "worker1"
    action = "add"
  }]
}

```
{:screen}

## Variables
{: #variables}

You can use `variable` blocks to identify dynamic values. For example, if you want to use a configuration to deploy Kubernetes clusters in multiple data centers, you can create one configuration to use as a template. You can then turn any values that would vary by deployments into variable blocks.

Example:

```
variable "datacenter" {
  type        = "string"
  description = "The data center that you want to deploy your Kubernetes cluster in."
}
```
{:codeblock}

You can then call the variable with the syntax `${var.<variable_name>}` in other blocks. The `type` argument defines the variable as a string so that you can pass the value in a simple key-value pair.

In the following example, the Kubernetes resource block is referencing the data center variable with `${var.datacenter}`.

```
resource "ibm_container_cluster" "test_cluster" {
  name         = "test"
  datacenter   = "${var.datacenter}"
}
```
{:screen}

See the <a href="https://www.terraform.io/docs/configuration/variables.html">Terraform docs <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> for more information about variable configuration.

### Other types of variables
{: #variables_other}

In addition to strings, you can pass lists and maps in the `type` argument.

* Lists define multiple values as an array. For example, you can set the data center variable to deploy the cluster to a selection of data centers in the US south region.

  Example:

  ```
  variable "datacenter" {
    type        = "list"
    default     = ["dal10", "dal12", "dal13"]
  }
  ```
  {:codeblock}

  You can then call the variable with the syntax `${var.list_name[list_item]}`. For the example, you can call the dal10 data center with `${var.datacenter[0]}`.

* Maps define nested values from string to string. For example, you can create a variable to set the name of multiple worker nodes in your cluster.

  Example:

  ```
  variable "workers" {
    type        = "map"
    default     = {
      "worker1  = "HTTP server",
      "worker2" = "API server",
      "worker3" = "Firewall"
    }
  }
  ```
  {:codeblock}

  You can then call the variable with the syntax `${var.map_name[key]}`. For the example, calling `${var.workers["worker1]}` would return "HTTP server".

### Custom variables
{: #variables_custom}

In addition to the variables available for each resource, the following table describes custom variables and values that you can use in the {{site.data.keyword.bpshort}} service.

| Variable | Value | Description |
| :------------- | :------------- | :------------- |
| `SCHEMATICSGITTOKEN` | Your Git personal access token. | This variable is required for {{site.data.keyword.bpshort}} to scan GitHub and GitLab Enterprise repositories or a private repository. |
| User defined | `$SCHEMATICS.SSHKEYPUBLIC` | Returns an environment-specific public SSH key that is created by {{site.data.keyword.bpshort}}. For example, if you wanted to create an SSH key pair to run code on a VM and copy files to a VM. {{site.data.keyword.bpshort}} can create the key pair with this variable and `$SCHEMATICS.SSHKEYPRIVATE`. |
| User defined | `$SCHEMATICS.SSHKEYPRIVATE` | Returns an environment-specific private SSH key that is created by {{site.data.keyword.bpshort}}. This value is always encrypted and not shown in the activity log. |
| User defined |`$SCHEMATICS.USER` | Returns the name of the user who requests the Terraform action. |
| User defined | `$SCHEMATICS.ENV` | Returns the name of the environment. |
| User defined | `$SCHEMATICS.ENVID` | Returns the unique ID assigned to the environment. |

_Table 1. Custom variables and values for {{site.data.keyword.bpshort}}._

### Advanced variable attributes in the {{site.data.keyword.bpshort}} GUI
{: #variables_advanced}

You can add annotations to your Terraform configuration files to handle variables in different ways, such as defining the range of numbers that are accepted as valid inputs or sorting the variables into a second tier in the {{site.data.keyword.bpshort}} GUI.

Variable annotations can be written in HCL files as single or multi-line comments, or as a dedicated JSON file in your repository with the name `varinfo.json`.

**NOTE:** The `varinfo.json` file takes higher precedence over HCL files with inline comments. If you have both the JSON file and HCL inline comments, the comments in the HCL files are ignored.

The following table lists advanced variable attributes and example code snippets.

<table summary="Advanced variable attributes with HCL and JSON code examples.">
<caption>Table 2. Advanced variable attributes with HCL and JSON code examples.
</caption>
<thead>
<th colspan="1">Attribute</th>
<th colspan="1">Code example</th>
</thead>
<tbody>
<tr>
<td>
Define acceptable numerical values with the `range` attributes.
</td>
<td>
HCL:
<pre class="codeblock">
<code>
variable "number_of_servers"
/&#42;
  {
     "range" : {
         "start" : 1,
         "end" : 5
      }
  }
&#42;/
{
  description = "Number of servers to deploy"
  default     = 1
}
</code>
</pre>
JSON:
<pre class="codeblock">
<code>
[
  {
     "name" : "number_of_servers",
     "range" : {
         "start" : 1,
         "end" : 5
      }
  }
]
</code>
</pre>
</td>
</tr>
<tr>
<td>
Control display of variables with the `visible` attribute. If set to `false`, this attribute separates variables into a second tier, which appears as an expanded variable section in the GUI. Default value: `false`.
</td>
<td>
HCL single comment:
<pre class="codeblock">
<code>
variable "account_number"
&#35; { "hidden: true }
{
  description = "Account number for invoicing"
}
</code>
</pre>
HCL multi-line comment:
<pre class="codeblock">
<code>
variable "account_number"
/&#42;
   {
      "hidden: true
   }
&#42;/
{
  description = "Account number for invoicing"
}
</code>
</pre>
JSON:
<pre class="codeblock">
<code>
[
   {
      "name" : "account_number",
      "hidden" : true
   }
]
</code>
</pre>
</td>
</tr>
<tr>
<td>
Provide a drop-down menu of predefined value options for the user to choose from in the GUI. You can specify a default value either in the variable definition (higher precedence) or as part of the annotation inside the option itself.
</td>
<td>
HCL with a default specified in the variable definition:
<pre class="codeblock">
<code>
variable "disksize"
/&#42;
   {
      "options" : [
          {
             "value" : 250,
             "label" : "250 GB"     
          },
          {
             "value" : 500,
             "label" : "500 GB"     
          }
      ]
   }
&#42;/
{
  description = "Disk size in GB"
  default     = 250
}
</code>
</pre>
JSON with a default specified in the annotation for an option:
<pre class="codeblock">
<code>
[
   {
      "name"    : "disksize"
      "options" : [
          {
             "value"   : 250,
             "label"   : "250 GB",
             "default" : true
          },
          {
             "value" : 500,
             "label" : "500 GB"     
          }
      ]
   }
]
</code>
</pre>
</td>
</tr>
</tbody></table>

### What's next
{: #next}

* After you store your Terraform configuration in source control, you can create an environment in {{site.data.keyword.bpshort}} and [deploy your resources](schematics_deploying.html).
* Try building up the sample Kubernetes configuration to work with other <a href="https://ibm-cloud.github.io/tf-ibm-docs/">{{site.data.keyword.cloud_notm}} resources <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.



Writing configurations
If you bring your own Terraform configuration to Schematics, the following recommendations can help you better write configurations that are well-structured, reusable, and comprehensive. For more information about configuration anatomy, see Creating a configuration.

Use input variables to pass credentials instead of embedding credentials in configurations
Because configurations are reusable, do not embed your personal credentials into configurations. Instead, use input variables to pass credentials into configurations. When you use the GUI to pass these variable values, you can mask these sensitive values from other users in the organization by clicking the eye icon.

Provide a default value to make a variable optional
You can use the default parameter to set a default value for a variable. However, providing a default value automatically makes the variable optional. If no default value is provided, then the variable is required.

For example, the following API key variable declaration has no default value, requiring a user-input value.

"softlayer-api-key" {
  description = "Your IBM Cloud Infrastructure (SoftLayer) API key."
}
However, the following variable declaration for a load balancer service group port has a default value of 80.

variable "lb-service-group-port" {
default = 80
description = "The port for the local load balancer service group."
}
Use a dedicated file to store output declarations, and if your configuration is highly modularized, a dedicated file to store variable declarations
It is common practice to use a dedicated file, often called outputs.tf, to store your output variable declarations. It is common practice to also use a dedicated file to store variable declarations, especially if your configuration is simple and your components are highly modularized. For example, the NGINX Auto Scale Group template External link icon is fairly modularized, and declares variables in output.tf and variables.tf files. But for more complex configurations, store your variable declarations with the resource file that calls it so that it's easier for anyone who reads your configuration to map the variables to the resources.

