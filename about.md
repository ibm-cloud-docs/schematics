---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-05"

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

# About IBM Cloud Schematics
{: #about-schematics}

{{site.data.keyword.cloud_notm}} Schematics delivers Terraform-as-a-Service so that you can use a high-level scripting language to model the resources that you want in your {{site.data.keyword.cloud_notm}} environment, and enable Infrastructure as Code (IaC). 
{: shortdesc}

## Service architecture
{: #schematics-architecture}

[Terraform ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.terraform.io/) is an Open Source software that is developed by HashiCorp that enables predictable and consistent resource provisioning to rapidly build complex, multi-tier cloud environments.

**How is {{site.data.keyword.cloud_notm}} Schematics different from Terraform?** </br>
With {{site.data.keyword.cloud_notm}} Schematics, you can organize your {{site.data.keyword.cloud_notm}} resources across environments by using workspaces. Every workspace is connected to a GitHub repository that contains a set of Terraform configuration files which as a whole build a Schematics template. The Schematics templates can be provided by IBM, or you can create your own Schematics template from an existing GitHub repository. 

Workspaces allow for the separation of concerns for cloud resources. Each environment is isolated from the other so that they can operate independently. For example, you might use three different workspaces for your test, staging, and prod environment. Each workspace can be managed separately so that you can test out your changes in your staging workspace without changing your production resouces. {{site.data.keyword.cloud_notm}} Schematics is integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management to manage and control who can apply the Terraform configuration in your {{site.data.keyword.cloud_notm}} account. 

For more information, see [About {{site.data.keyword.cloud_notm}} Schematics](/docs/schematics?topic=schematics-about). 

**I am not familiar with Terraform, can I still use {{site.data.keyword.cloud_notm}} Schematics?** </br>
Yes. {{site.data.keyword.cloud_notm}} Schematics provides a set of pre-defined Terraform templates that you can choose from. Simply select the template that you want from the {{site.data.keyword.cloud_notm}} console and fill out the variables to customize your {{site.data.keyword.cloud_notm}} resource. Then, create a workspace in {{site.data.keyword.cloud_notm}} Schematics from this template and watch {{site.data.keyword.cloud_notm}} Schematics provision the resourcs for you. 

## Benefits
{: #schematics-benefits}

Review the capabilities that {{site.data.keyword.cloud_notm}} Schematics provides to templatize and organize your  {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

| Benefit    | Description   |
| :------------- | :------------- |
| Enable Infrastructure as Code (IaC) | Use Terraform configuration files to model, codify, and configure the {{site.data.keyword.cloud_notm}} resources that you want, and build your own resource library that you can replicate or re-create across environments. If you want to change your environment, you state the outcome that you want and let {{site.data.keyword.cloud_notm}} Schematics determine the actions that must be performed to get to the described state. |
| Use native Terraform capabilities | Build your Terraform configuration files in HCL or JSON format and provision your specified resources with {{site.data.keyword.cloud_notm}} Schematics. {{site.data.keyword.cloud_notm}} Schematics supports all {{site.data.keyword.cloud_notm}} resources that are provided by the [{{site.data.keyword.cloud_notm}} Provider plug-in for Terraform](https://ibm-cloud.github.io/tf-ibm-docs/) with the advantage that you don't have to install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in. Simply use the built-in Terraform capabilities in the {{site.data.keyword.cloud_notm}} console to connect {{site.data.keyword.cloud_notm}} Schematics to the GitHub repository that hosts your files, create a provisioning plan, and watch {{site.data.keyword.cloud_notm}} Schematics spin up your resources.  |
| One language to describe resources | Every {{site.data.keyword.cloud_notm}} resources comes with a CLI or API that you can use to provision and work with the resource. By using the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, you don't need to learn each CLI or API to automate the provisioning of your resources. Instead, you use the Terraform language to model all your resources. |
| Organize {{site.data.keyword.cloud_notm}} resources in workspaces | With {{site.data.keyword.cloud_notm}} Schematics, you can organize your {{site.data.keyword.cloud_notm}} resources across environments by using workspaces. Every workspace is connected to a GitHub repository that contains a set of Terraform configuration files. Use workspaces to distinguish between your test, staging, and prod environment, and to change resource configurations without affecting resources in other environments.  |
| Control access to your {{site.data.keyword.cloud_notm}} resources | Assign platform and services access permissions to your users in {{site.data.keyword.cloud_notm}} Identity and Access Management to control who can provision and manage resources in your {{site.data.keyword.cloud_notm}} account. |
| Leverage GitHub for version control | Every workspace is connected to a repository in GitHub so that you can keep your Terraform configuration files in source control and enable collaboration, review, and auditing of changes. You can also roll back to a previous version of your configuration file and let {{site.data.keyword.cloud_notm}} Schematics deploy the change to your {{site.data.keyword.cloud_notm}} environment. |
| Combine with other configuration management tools | Use {{site.data.keyword.cloud_notm}} Schematics to provision your {{site.data.keyword.cloud_notm}} resources, and install or configure additional software by using other configuration tools, like for example Ansible, Chef, or Puppet.  
| Get {{site.data.keyword.cloud_notm}} help and support | {{site.data.keyword.cloud_notm}} Schematics is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue with using {{site.data.keyword.cloud_notm}} Schematics, [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support). |

## IBM Cloud Schematics vs. Terraform
