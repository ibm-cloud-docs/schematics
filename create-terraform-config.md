---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-23"

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
{:external: target="_blank" .external}

# Creating Terraform configurations
{: #create-tf-config}

Learn how to create Terraform configuration files that are well-structured, reusable, and comprehensive.
{: shortdesc}

**How is a Terraform configuration structured?** </br>
A Terraform configuration consists of one or more Terraform files that declare the state that you want to achieve for your {{site.data.keyword.cloud_notm}} resources. To successfully work with your resources, you must [configure IBM as your cloud provider](#configure-provider) and [add resources to your Terraform configuration file](#configure-resources). Optionally, you can use [input variables](#configure-variables) to customize your resources.

**What language do I use to develop my infrastructure code?** </br>
You can write your Terraform configuration by using HashiCorp Configuration Language (HCL) or JSON syntax. For more information, see [Configuration language](https://www.terraform.io/docs/configuration/index.html){: external}.  

**Where do I store my Terraform configuration files?** </br>
A Terraform configuration is infrastructure code that you must treat as regular code. To support collaboration, source and version control, store your files in a GitHub or GitLab repository. With version control, you can revert to previous configurations, audit changes to configurations, and share code with multiple teams. You can also set up your own continuous integration pipeline to automatically apply your configuration changes in {{site.data.keyword.cloud_notm}}. 

The following image shows an example of how your Terraform configuration files could look like in a GitHub repository. 

<img src="images/gh-repo-structure.png" alt="Sample GitHub setup for a Terraform configuration" width="800" style="width: 800px; border-style: none"/>

**What do I do with my `terraform.tfvars` file?** </br>
The `terraform.tfvars` file is a local variables file that you use to store sensitive information, such as your {{site.data.keyword.cloud_notm}} API key or classic infrastructure user name when you use native Terraform. This file must be present on your local machine so that Terraform can load the values for your credentials when you initialize the Terraform CLI. With {{site.data.keyword.cloud_notm}} Schematics, you do not use a local `terraform.tfvars` file. Instead, you [declare your variables](#configure-variables) in the Terraform configuration files, and enter the values for your variables in the {{site.data.keyword.cloud_notm}} Schematics console when you create a workspace. 

**What if I have an existing `terraform.tfstate` file?** </br>
If you used native Terraform before to provision and manage {{site.data.keyword.cloud_notm}} resources, you might have a `terraform.tfstate` file in your GitHub repository that stores the current state of your Terraform-deployed {{site.data.keyword.cloud_notm}} resources. During the {{site.data.keyword.cloud_notm}} Schematics beta, `terraform.tfstate` files are not imported when you create a Schematics workspace. Because the `terraform-tfstate` file is not available to {{site.data.keyword.cloud_notm}} Schematics, you cannot use the service to manage {{site.data.keyword.cloud_notm}} resources that you already started managing with native Terraform. 

## Configuring IBM as your cloud provider 
{: #configure-provider}

Specify the cloud provider that you want to use to provision your resources in the `provider` block of your Terraform configuration file. The `provider` block includes all the input variables the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform requires to provision your resources.
{: shortdesc}

1. Choose how you want to configure the `provider` block. 
   - **Option 1: Create a separate `provider.tf` file.** The information in this file is loaded by Terraform and {{site.data.keyword.cloud_notm}} Schematics, and applied to all Terraform configuration files that exist in the same directory. This approach is useful if you split out your infrastructure code across multiple files. 
   - **Option 2: Add a `provider` block to your Terraform configuration file.** You might choose this option if you prefer to specify the provider alongside with your variables and resources in one Terraform configuration file. 

2. Review what credentials and information you must provide in the `provider` block to work with your resources. For some resources, {{site.data.keyword.cloud_notm}} Schematics automatically retrieves the {{site.data.keyword.cloud_notm}} API key so that you do not have to include this information in your `provider` block.
   
   <table>
   <thead>
     <th>Resource</th>
     <th>Required input parameters</th>
  </thead>
  <tbody>
    <tr>
      <td>VPC infrastructure resources</td>
      <td>Generation of {{site.data.keyword.cloud_notm}} VPC infrastructure</td>
    </tr>
    <tr>
      <td>Classic infrastructure resources</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} classic infrastructure user name</li><li>{{site.data.keyword.cloud_notm}} classic infrastructure API key</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
    </tr>
    <tr>
      <td>IBM Cloud Kubernetes Service resources</td>
      <td>n/a</td>
    </tr>
    <tr>
      <td>Cloud Foundry resources</td>
      <td><ul><li>{{site.data.keyword.cloud_notm}} API key</li><li>{{site.data.keyword.cloud_notm}} region</li></ul></td>
    </tr>
    <tr>
      <td>All other resources</td>
      <td>{{site.data.keyword.cloud_notm}} region</td>
    </tr>
  </tbody>
  </table>
   
2. Create a `provider.tf` file or add the following code to your Terraform configuration file. 

   Example for VPC infrastructure resources: 
   ```
   provider "ibm" {
     generation = 1
   }
   ```
   {: codeblock}
   
   Example for classic infrastructure resources: 
   ```
   variable "softlayer_username" {}
   variable "softlayer_api_key" {}

   provider "ibm" {
     region = "<region_name>"
     softlayer_username = "${var.softlayer_username}"
     softlayer_api_key  = "${var.softlayer_api_key}"
   }
   ```
   {: codeblock}
   
   Example for all {{site.data.keyword.containerlong_notm}} resources:
   ```
   provider "ibm" {
   }
   ```
   {: codeblock}
   
   Example for Cloud Foundry resources:
   ```
   variable "ibmcloud_api_key" {}
   
   provider "ibm" {
     ibmcloud_api_key = "${var.ibmcloud_api_key}"
     region = "<region_name>"
   }
   ```
   {: codeblock}
   
   Example for all other resources:
   ```
   provider "ibm" {
     region = "<region_name>"
   }
   ```
   {: codeblock}

## Adding {{site.data.keyword.cloud_notm}} resources to your Terraform configuration file
{: #configure-resources}

Use `resource` blocks to define the {{site.data.keyword.cloud_notm}} resources that you want to manage with {{site.data.keyword.cloud_notm}} Schematics. 
{: shortdesc}

To support a multi-cloud approach, Terraform works with multiple cloud providers. A cloud provider is responsible for understanding the resources that you can provision, their API, and the methods to expose these resources in the cloud. To make this knowledge available to users, every supported cloud provider must provide a CLI plug-in for Terraform that users can use to work with the resources. To find an overview of the resources that you can provision in {{site.data.keyword.cloud_notm}}, see the [{{site.data.keyword.cloud_notm}} Provider plug-in for Terraform reference](https://ibm-cloud.github.io/tf-ibm-docs/){: external}. 

Example infrastructure code for provisioning a VPC: 
```
resource ibm_is_vpc "vpc" {
  name = "myvpc"
}
```
{: codeblock}



### Referencing resources in other resource blocks
{: #reference-resource-info}

Review the options that you have to reference existing resources in other resource blocks of your Terraform configuration file. 
{: shortdesc}

The {{site.data.keyword.cloud_notm}} Provider plug-in reference includes two types of objects, data sources and resources. You can use both objects to reference resources in other resource blocks.  

- **Resources**: To create a resource, you use the resource definitions in the {{site.data.keyword.cloud_notm}} Provider plug-in reference. A resource definition includes the syntax for configuring your {{site.data.keyword.cloud_notm}} resources and an **Attributes reference** that shows the properties that you can reference as input parameters in other resource blocks. For example, when you create a VPC, the ID of the VPC is made available after the creation. You can use the ID as an input parameter when you create a subnet for your VPC. Use this option if you combine multiple resources in one Terraform configuration file.  </br>

  Example infrastructure code: 
  ```
  resource ibm_is_vpc "vpc" {
    name = "myvpc"
  }

  resource ibm_is_security_group "sg1" {
    name = "mysecuritygroup"
    vpc  = "${ibm_is_vpc.vpc.id}"
  }
  ```
  {: codeblock}

- **Data sources**: You can also use the data sources from the {{site.data.keyword.cloud_notm}} Provider plug-in reference to retrieve information about an existing {{site.data.keyword.cloud_notm}} resource. Review the **Argument reference** section in the {{site.data.keyword.cloud_notm}} Provider plug-in reference to see what input parameters you must provide to retrieve an existing resource. Then, review the **Attributes reference** section to find an overview of parameters that are made available to you and that you can reference in your `resource` blocks. Use this option if you want to access the details of a resource that is configured in another Terraform configuration file. 
  
  Example infrastructure code: 
  ```
  data ibm_is_image "ubuntu" {
    name = "ubuntu-18.04-amd64"
  }
  
  resource ibm_is_instance "vsi1" {
    name    = "$mysi"
    vpc     = "${ibm_is_vpc.vpc.id}"
    zone    = "us-south1"
    keys    = ["${data.ibm_is_ssh_key.ssh_key_id.id}"]
    image   = "${data.ibm_is_image.ubuntu.id}"
    profile = "cc1-2x4"

    primary_network_interface = {
      subnet          = "${ibm_is_subnet.subnet1.id}"
      security_groups = ["${ibm_is_security_group.sg1.id}"]
    }
  }
  ```
  {: codeblock}


## Using input variables customize resources
{: #configure-variables}

You can use `variable` blocks to templatize your infrastructure code. For example, instead of creating multiple Terraform configuration files for a resource that you want to deploy in multiple data centers, simply reuse the same configuration and use an input variable to define the data center. 
{: shortdesc}

**Where do I store my variable declarations?** </br>
You can decide to declare your variables within the same Terraform configuration file where you specify the resources that you want to provision, or to create a separate `variables.tf` file that includes all your variable declarations. When you create a workspace, {{site.data.keyword.cloud_notm}} Schematics automatically parses through your Terraform configuration files to find variable declarations. 

Example variable declaration without details: 
```
variable "datacenter" {}
```
{: codeblock}

Example variable declaration without a default value: 
```
variable "datacenter" {
  type        = "string"
  description = "The data center that you want to deploy your Kubernetes cluster in."
}
```
{: codeblock}

Example variable declaration with a default value: 
```
variable "datacenter" {
  type        = "string"
  description = "The data center that you want to deploy your Kubernetes cluster in."
  default = "dal10"

}
```
{: codeblock}

### Referencing variables
{: #reference-variables}

You can reference the value of the variable in other blocks of your Terraform configuration files by using the `"${var.<variable_name>}"` syntax. 

Example for referencing a `datacenter` variable: 

```
resource ibm_container_cluster "test_cluster" {
  name         = "test"
  datacenter   = "${var.datacenter}"
}
```
{: codeblock}

For more information about variable configurations, see the [Terraform documentation](https://www.terraform.io/docs/configuration/variables.html){: external}.



