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

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the provisioning, update, and deletion of your _service-name_ instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

Before you begin, make sure that you have the [required access](/docs/schematics?topic=schematics-access) to create and work with {{site.data.keyword.bplong_notm}} workspace resources. 

1. Follow the [Terraform on {{site.data.keyword.cloud}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform) to install the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in for Terraform. The plug-in abstracts the {{site.data.keyword.cloud}} APIs that are used to provision, update, or delete _service-name_ service instances and resources. 
2. Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to create a {{site.data.keyword.keymanagementserviceshort}} service instance and to assign a user an access policy in Identity and Access Management (IAM) for that instance by using HashiCorp Configuration Language (HCL). For more information, see the [Terraform documentation](https://www.terraform.io/docs/language/index.html){: external}. 

   The {{site.data.keyword.bplong_notm}} workspace instance in the following example is named `myworkspace` and is created with the tiered pricing plan in the `us-south` region. The `user@ibm.com` is assigned the Manager role in the IAM access policy. For other supported regions, see [Regions and endpoints](/docs/key-protect?topic=key-protect-regions).

   The following example creates the {{site.data.keyword.bplong_notm}} workspace `schematics_workspace` in the `default` resource group of `us-east` region. This workspace resources uses `terraform_v0.13.5` and the Terraform repository Git URL to provision the resources in the `us-east` region and specified resource group. 
   
   ```terraform

   data "ibm_schematics_workspace" "schematics_workspace" {
    workspace_id = "workspace_id"
   }

   resource "ibm_schematics_workspace" "schematics_workspace" {
     name = "myworkspace"
     description = "myworkspace description."
     location = "us-east"
     resource_group = "default"
     template_type = "terraform_v0.13.5"
    }

   ```
   {: codeblock}

  You can retrieve the unique identifier `id` of the {{site.data.keyword.bpshort}} workspace by using data source as `ibm_schematics_workspace.schematics_workspace.id` for listing the created workspace.

   ```terraform

   data "ibm_schematics_workspace" "schematics_workspace" {
    workspace_id = "workspace_id"
   }

   ```
   {: codeblock}
   
3. Initialize the Terraform CLI. 

   ```
   terraform init
   ```
   {: pre}
4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to create the {{site.data.keyword.keymanagementserviceshort}} instance in your account.

   ```
   terraform plan
   ```
   {: pre}
5. Create the {{site.data.keyword.bpshort}} workspace instance and IAM access policy in {{site.data.keyword.cloud_notm}}.

   ```
   terraform apply
   ```
   {: pre}
6. From the [{{site.data.keyword.bplong_notm}} resource list](/resources){: external}, select the {{site.data.keyword.bpshort}} workspace instance that you created and note the instance ID. 
7. Verify that the {{site.data.keyword.bpshort}} workspace instance is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.bpshort}} service instance with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks: 

  - Learn how to create an [{{site.data.keyword.bplong_notm}} job](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_job) resource.
  - Learn how to create an [{{site.data.keyword.bplong_notm}} action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action) resource.
  - Explore the other supported data sources, arguments and attributes reference for the [{{site.data.keyword.bplong_notm}} Terraform resources and data sources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_action) that are used in this example.
