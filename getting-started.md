---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

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

# Getting started with IBM Cloud Schematics (beta)
{: #getting-started}

Enable Infrastructure as Code (IaC) with {{site.data.keyword.cloud_notm}} Schematics, and start templatizing, provisioning, and managing {{site.data.keyword.cloud_notm}} resources in your {{site.data.keyword.cloud_notm}} environment by using Terraform configuration files. 
{: shortdesc}

{{site.data.keyword.cloud_notm}} Schematics is available as a beta to test out Terraform-as-a-Service capabilities in {{site.data.keyword.cloud_notm}} and might be unstable or change frequently. {{site.data.keyword.cloud_notm}} beta services also might not provide the same level of performance or compatibility that generally available services provide, and are not intended for use in a production environment.
{: #preview}

## Create your Terraform configuration file
{: #create-config}



## Setting up your workspace
{: #setup-workspace}

Create a workspace in {{site.data.keyword.cloud_notm}} Schematics that points to the GitHub repository that hosts your Terraform configuration file. 
{: shortdesc}

1. From the [{{site.data.keyword.cloud_notm}} catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=devops), select **Schematics**. 
2. Click **Create a workspace**. 
3. Configure your workspace. 
   1. Enter the link to your public GitHub repository. The link must point to the `master` branch in GitHub. You cannot link to other branches during the beta. 
   2. Enter a descriptive name for your workspace. To find your workspace more easily after you create it, make sure to include the microservice component and the environment in your name. For more information about how to structure your workspaces, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace).
   3. Optional: Enter tags for your workspace. You can use the tags later to find workspaces that are related to each other. 
   4. Select a resource group and a region for your workspace. All {{site.data.keyword.cloud_notm}} resources that you create in your workspace are provisioned in the selected resource group and region. 
   5. Optional: Enter a description for your workspace.
   6. Enter the values for your variables. When you enter the GitHub repository URL that hosts your Terraform configuration files, {{site.data.keyword.cloud_notm}} Schematics automatically parses through your files to find variable declarations. 
4. Click **Create** to create your workspace. When you create the workspace, all Terraform configuration files are loaded into {{site.data.keyword.cloud_notm}} Schematics, but your resources are not yet deployed to {{site.data.keyword.cloud_notm}}.


## Provision your {{site.data.keyword.cloud_notm}} resources
{: #provision-resources}

Use {{site.data.keyword.cloud_notm}} Schematics to run the infrastructure code in your Terraform configuration files, and to provision your {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

Before you begin, [set up your workspace in {{site.data.keyword.cloud_notm}} Schematics](#setup-workspace). 

1. From the workspace details page, click **Run new plan** to create a Terraform execution plan. This plan equals the output of the `terraform plan` command. You can review the status of your plan in the **Recent activtity** section of your workspace details page. 
2. Review the log files of your execution plan. This plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform configuration files. If you have syntax errors in your configuration files, you can review the error message in the log file. 
3. Apply your Terraform configuration by clicking **Apply plan**. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. 
4. After your resources are provisioned, review the log file to ensure that no errors occured during the provisioning, modification, or deletion process. 
5. From the workspace details page, select the **Resources** tab to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account. 

## What's next? 
{: #whats-next}


