---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-15"

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Using Terraform to configure {{site.data.keyword.bpshort}} 
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services to rapidly build complex, multitiered cloud environments following Infrastructure as Code (IaC) principles. Similarly, you can automate the provisioning, update, and deletion of your {{site.data.keyword.bplong_notm}} Workspace and Actions  instances using Terraform.
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud_notm}} solution? Try out [{{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command-line and the {{site.data.keyword.cloud_notm}} Provider plug-in. {{site.data.keyword.bpshort}} also provides predefined Terraform templates that you can install from the {{site.data.keyword.cloud_notm}} catalog.
{: tip}

Before you begin, make sure that you have the [required access](/docs/schematics?topic=schematics-access) to create and work with {{site.data.keyword.bplong_notm}} Workspace.

## Example: Creating the {{site.data.keyword.bpshort}} Workspaces by using Terraform 
{: #workspace-resource}

Complete the following steps to create the {{site.data.keyword.bpshort}} Workspaces by using Terraform:

1. Follow the [Terraform on {{site.data.keyword.cloud_notm}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform) to install the Terraform CLI and configure the {{site.data.keyword.terraform-provider_full_notm}}. The plug-in abstracts the {{site.data.keyword.cloud_notm}} `APIs` that are used to provision, update, or delete {{site.data.keyword.bpshort}} resources. 

2. Create the Terraform configuration files named `main.tf`, `terraform.tfvars`, and `versions.tf`.

    You can use the Terraform example Git URL `https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-api-gateway`. This example uses service instance to set up an API for an {{site.data.keyword.cloud_notm}} service of your choice. You can specify the API endpoint that you want to use to access your service, and define subscription keys so that you can securely consume your API. 

    If you have a workspace created other in a region other than `us`, you must set the API endpoint to that region. For example, if your region specified is `eu`, the API endpoint should be specified as `IBMCLOUD_SCHEMATICS_API_ENDPOINT=https://eu.schematics.cloud.ibm.com` in the environment variable. For more information about the {{site.data.keyword.bpshort}} Workspaces locations and endpoints to be used, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
    {: note}

    Then create the {{site.data.keyword.bpshort}} Workspaces `tf-testwks-apigwy` in the `default` resource group of your region. This workspace points to a Terraform template of your choice that requires the Terraform version `terraform_v1.0`. 

    **versions.tf**

    The sample `versions.tf` file to specify the provider version that you need to create the workspace.

    ```terraform
      terraform {
      required_version = ">=1.0.0, <2.0"
        required_providers {
          ibm = {
            source = "IBM-Cloud/ibm"
          }
        }
      }
    ```
    {: codeblock}

    **`terraform.tfvars`**

    The sample `terraform.tfvars` file to store sensitive information, such as credentials. For more information, see [Referencing credentials from a `terraform.tfvars` file](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#tf-variables). To create API keys, see [Creating and API Keys](/docs/account?topic=account-userapikey#create_user_key).

    ```terraform
    schematics_workspace_name="tf-testwks-apigwy"
    schematics_workspace_description="Sample workspace created with terraform with URL"
    schematics_workspace_type="terraform_v1.0"
    schematics_workspace_location="us-south"
    schematics_workspace_resource_group="default"
    ibmcloud_api_key="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

    ```
    {: codeblock}

    **main.tf**

    Review the following sample `main.tf` file. This file invokes the variables from the `terraform.tfvars` file by using the Git URL, then creates a {{site.data.keyword.bpshort}} Workspaces by using your {{site.data.keyword.cloud_notm}} API key.
    

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

    The following table lists supported parameters when you create and initialize a service instance with Terraform. For more information about the detailed parameters to create workspace, see [`ibm_schematics_workspace`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_workspace){: external} resource.

    | Parameter | Description |
    | -------- | --------- |
    | `description` | The description of the workspace. |
    | `location` | The location where you want to create your {{site.data.keyword.bpshort}} Workspaces and run {{site.data.keyword.bpshort}} Actions. |
    | `resource_group` | The ID of the resource group where you want to provision the workspace. |
    | `name` | The name of your workspace. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores. When you create a workspace for your own Terraform template, consider including the microservice component that you set up with your Terraform template and the {{site.data.keyword.cloud_notm}} environment where you want to deploy your resources in your name.|
    | `tags` | A list of tags that are associated with the workspace. |
    | `template_env_settings` | A list of environment variables that you want to apply during the execution of a Terraform action. |
    | `template_git_url` | The Git repository URL, where you have the configuration details to provision the resource. |
    | `template_type` |  Specify the Terraform version that you want to apply in {{site.data.keyword.bpshort}} Workspace. |
    {: caption="Supported parameters for creating {{site.data.keyword.bpshort}} Workspaces with Terraform." caption-side="top"}

3. Initialize the Terraform CLI. 

    ```sh
    terraform init
    ```
    {: pre}

    If the environment variable path for Terraform is not set, you can see `command not found: terraform` error. Fix the error by setting the path to your [Terraform installed directory](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started#tf_installation).
    {: note}

4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to create the {{site.data.keyword.bpshort}} Workspaces in your account.

    ```sh
    terraform plan
    ```
    {: pre}

5. Create the {{site.data.keyword.bpshort}} Workspaces instance and IAM access policy in {{site.data.keyword.cloud_notm}}.

    ```sh
    terraform apply
    ```
    {: pre}

    For more information about troubleshooting the `terraform apply` command errors, see [find the root cause of why {{site.data.keyword.bpshort}} apply is failing](/docs/schematics?topic=schematics-nullresource-errors).
    {: note}

6. From the [{{site.data.keyword.bpshort}} dashboard](https://cloud.ibm.com/schematics), check your `tf-testwks-apigwy` workspace is created. And the resources are provisioned from the [{{site.data.keyword.bplong_notm}} resource list](https://cloud.ibm.com/resources){: external}.

7. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.bpshort}} Workspaces with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks: 

- Learn how to create an [{{site.data.keyword.bplong_notm}} job](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_job){: external} resource to run your Terraform template in IBM Cloud.
- To run `Ansible playbooks` in {{site.data.keyword.cloud_notm}} check out the [{{site.data.keyword.bplong_notm}} action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action){: external} resource.
- Explore other supported Terraform resources and data sources for [{{site.data.keyword.bplong_notm}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action){: external} or checkout other arguments and attributes that you can use for the Terraform resources that were used in this example.


