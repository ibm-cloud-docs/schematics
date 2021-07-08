---

copyright:
  years: 2021
lastupdated: "2021-05-19"

subcollection: schematics

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}


# Setting up Terraform for {{site.data.keyword.bplong_notm}} 
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the provisioning, update, and deletion of your {{site.data.keyword.bplong}} services instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud_notm}} solution? Try out [{{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud_notm}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud_notm}} catalog.
{: tip}

Before you begin, make sure that you have the [required access](/docs/schematics?topic=schematics-access) to create and work with {{site.data.keyword.bplong_notm}} workspace.

## Example: Creating the {{site.data.keyword.bpshort}} workspaces by using Terraform 
{: #workspace-resource}

Complete the following steps to create the {{site.data.keyword.bpshort}} workspaces by using Terraform:

1. Follow the [Terraform on {{site.data.keyword.cloud_notm}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform) to install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to provision, update, or delete {{site.data.keyword.bpshort}} resources. 

2. Create a Terraform configuration file that is named `main.tf` and `terraform.tfvars` in the same folder as `versions.tf`. In this file, you add the configuration to create a {{site.data.keyword.bpshort}} workspace by using HashiCorp Configuration Language (HCL).

   You can use Terraform example Git URL `https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway`. This example uses service instance to set up an API for an {{site.data.keyword.cloud_notm}} service of your choice. You can specify the API endpoint that you want to use to access your service, and define subscription keys so that you can securely consume your API.
   Then creates the {{site.data.keyword.bplong_notm}} workspace `tf-testwks-apigwy` in the `default` resource group of your region. This workspace points to a Terraform template of your choice that requires the Terraform version `terraform_v0.12.20`. 

   **version.tf**
   The sample `versions.tf` file to specify the provider version that you need to create the workspace.

   ```
      terraform {
      required_providers {
         ibm = {
            source = "IBM-Cloud/ibm"
            version = "1.27.1"
            }
      }
   }
   ```
   {: codeblock}

   **terraform.tfvars**
   The sample `terraform.tfvars` file to store sensitive information, such as credentials. For more information, see [Referencing credentials from a terraform.tfvars file](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#tf-variables). To create API keys, see [Creating and API Keys](/docs/account?topic=account-userapikey#create_user_key).

   ```
   schematics_workspace_name="tf-testwks-apigwy"
   schematics_workspace_description="Sample workspace created with terraform with URL"
   schematics_workspace_type="terraform_v0.12.20"
   schematics_workspace_location="us-south"
   schematics_workspace_resource_group="default"
   ibmcloud_api_key="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

   ```
   {: codeblock}

   **main.tf**
   The sample `main.tf` file to invoke the variables from terraform.tfvars, by using the Git URL and your {{site.data.keyword.cloud_notm}} API key to create the 

   ```terraform
   variable "schematics_workspace_name" {}
   variable "schematics_workspace_description" {}
   variable "schematics_workspace_type" {}
   variable "schematics_workspace_location" {}
   variable "schematics_workspace_resource_group" {}
   variable "ibmcloud_api_key" {}

   resource "ibm_schematics_workspace" "schematics_workspace_instance" {
   name = var.schematics_workspace_name
   description = var.schematics_workspace_description
   location = var.schematics_workspace_location
   resource_group = var.schematics_workspace_resource_group
   tags = ["sample"]
   template_env_settings = [
      { env1 = "val1" },
      { env2 = "val2" }
   ]
   template_type= var.schematics_workspace_type
   template_git_url = "https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway"
   }

   provider "ibm" {
   region           = "us-south"
   ibmcloud_api_key = var.ibmcloud_api_key
   }

   ```
   {: codeblock}

   The following table lists supported parameters when you create and initialize a service instance with Terraform. For more information, about the detailed parameters to create workspace, see [ibm_schematics_workspace](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_workspace){: external}.

| Parameter | Description |
| -------- | --------- |
| name | The name of your workspace. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores. When you create a workspace for your own Terraform template, consider including the microservice component that you set up with your Terraform template and the IBM Cloud environment where you want to deploy your resources in your name.|
| description | The description of the workspace. |
| location | The location where you want to create your {{site.data.keyword.bpshort}} workspace and run {{site.data.keyword.bpshort}} actions. |
| resource_group | The ID of the resource group where you want to provision the workspace. |
| template_git_url | The Git repository URL, where you have the configuration details to provision the resource. |
| template_type |  Specify the Terraform version that you want to apply in {{site.data.keyword.bpshort}} workspace. |
{: caption="Table 1. Supported parameters for creating {{site.data.keyword.bpshort}} workspaces with Terraform." caption-side="top"}

3. Initialize the Terraform CLI. 

   ```
   terraform init
   ```
   {: pre}

   If the environment variable path for Terraform is not set, you can see `command not found: terraform` error. Fix the errof by setting the path to your [Terraform installed directory](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started#tf_installation).
   {: note}

4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to create the {{site.data.keyword.bpshort}} workspace in your account.

   ```
   terraform plan
   ```
   {: pre}

5. Create the {{site.data.keyword.bpshort}} workspace instance and IAM access policy in {{site.data.keyword.cloud_notm}}.

   ```
   terraform apply
   ```
   {: pre}

  For more information, about troubleshooting the terraform apply command errors, see [find the root cause of why Schematics apply is failing](/docs/schematics?topic=schematics-nullresource-errors).
  {: note}

6. From the [{{site.data.keyword.bpshort}} dashboard](https://cloud.ibm.com/schematics), check your `testmyworkspace` is created. And the resources are provisioned from the [{{site.data.keyword.bplong_notm}} resource list](https://cloud.ibm.com/resources){: external}.

7. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.bpshort}} workspace with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks: 

  - Learn how to create an [{{site.data.keyword.bplong_notm}} job](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_job){: external} resource to run your Terraform template in IBM Cloud.
  - To run Ansible playbooks in {{site.data.keyword.cloud_notm}} check out the [{{site.data.keyword.bplong_notm}} action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action){: external} resource.
  - Explore other supported Terraform resources and data sources for [{{site.data.keyword.bplong_notm}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action){: external} or check-out other arguments and attributes that you can use for the Terraform resources that were used in this example.
