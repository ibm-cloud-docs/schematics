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

## Reasons to use {{site.data.keyword.bpshort}}
{: #reasons}

You might want to use codified infrastructure in the following scenarios:
{:shortdesc}

| Scenario     | Reasons    |
| :------------- | :------------- |
| You want to re-create and reuse your infrastructure. | You can use {{site.data.keyword.bpshort}} for infrastructure management. With {{site.data.keyword.bpshort}}, you can provision, modify, and destroy your resources programmatically. When you codify and configure resources, you can build up a library of resources that can be reused again and again to net the same results.|
| You want transparency as to how your infrastructure is set up. | {{site.data.keyword.bpshort}} works with configurations in source control, which enables collaboration, review, and gives you an audit trail to see how and when changes were made. You can also view your changes if you need to roll back to a previous configuration. |
| You want to simplify the execution of environmental changes. | {{site.data.keyword.bpshort}} follows the declarative model that provides a single source of truth. When you plan a change to your environment, you state the outcome you want. |
| You already use a configuration management (CM) tool, but you want a more automated way to set up your environments. | {{site.data.keyword.bpshort}} can work along with CM tools. Environments that are deployed with {{site.data.keyword.bpshort}} are high-level abstractions that can create infrastructure resources. You can then use CM tools to install and configure software on the resources that {{site.data.keyword.bpshort}} provisioned.  


## IBM Cloud Schematics vs. Terraform
