---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-18"

keywords: schematics faqs, what is terraform, infrastructure as code, iac, schematics price, schematics pricing, schematics cost, schematics charges, schematics personal information, schematics pii, delete pii from schematics, schematics compliance

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
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}

# FAQs
{: #faqs}

Answers to common questions about the {{site.data.keyword.bplong_notm}} service.
{:shortdesc}


## What is {{site.data.keyword.bplong_notm}} and how does it work? 
{: #what-is-schematics}
{: faq}

{{site.data.keyword.bplong_notm}} delivers Terraform-as-a-Service so that you can use a high-level scripting language to model the resources that you want in your {{site.data.keyword.cloud_notm}} environment, and enable Infrastructure as Code (IaC). For more information, see [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics).

## What is Terraform? 
{: #what-is-terraform}
{: faq}

Terraform is an open-source software that is developed by HashiCorp that enables predictable and consistent provisioning of cloud platform and infrastructure resources by using a high-level scripting language. You can use Terraform to automate your {{site.data.keyword.cloud_notm}} resource provisioning, rapidly build complex, multi-tier cloud environments, and enable Infrastructure as Code (IaC).

## What is Infrastructure as Code?
{: #what-is-iac}
{: faq}

Infrastructure as Code (IaC) helps you codify your cloud environment so that you can automate the provisioning and management of your resources in the cloud. Rather than manually provisioning and configuring infrastructure resources or using scripts to adjust your cloud environment, you use a high-level scripting language to specify your resource and its configuration. Then, you use tools like Terraform to provision the resource in the cloud by leveraging its API. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring.

## What am I charged for when I use {{site.data.keyword.bpshort}}?
{: #charges}
{: faq}

{{site.data.keyword.bplong_notm}} workspaces are provided to you at no cost. However, when you decide to apply your Terraform template in {{site.data.keyword.cloud_notm}} by clicking **Apply plan** from the workspace details page or running the `ibmcloud terraform apply` command, you are charged for the {{site.data.keyword.cloud_notm}} resources that are described in your Terraform template. Review available service plans and pricing information for each resource that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first.

## What information is stored when I decide to create a workspace with {{site.data.keyword.bpshort}}? 
{: #data-residency}
{: faq}

The following personal information is stored with IBM when you create and use a {{site.data.keyword.bpshort}} workspace: 
- Workspace details
- Workspace variables
- Terraform config files that your workspace points to
- Terraform state files
- Terraform log files
- User activity logs and {{site.data.keyword.cloudaccesstrailshort}} events

By default, all information is encrypted in transit and at rest. To make your data highly available, all data is stored in the **US South** region and replicated to the **US East** region. Make sure that your data can reside in these regions before you start using {{site.data.keyword.bpshort}}. 

## How can I ensure that all my personal information is removed when I stop using {{site.data.keyword.bpshort}}? 
{: #data-deletion}
{: faq}

To remove your personal information from {{site.data.keyword.bplong_notm}}, choose among the following options: 
- **Delete the workspace**: When you delete your workspace, all personal information related to a workspace is permanently deleted. 
- **Open an {{site.data.keyword.cloud_notm}} support case**: Contact IBM Support to remove your workspaces and personal information by opening a support case. For more information, see [Getting support](/docs/get-support?topic=get-support-getting-customer-support). 
- **End your {{site.data.keyword.cloud_notm}} subscription**: A {{site.data.keyword.bpshort}} cleanup job runs multiple times a day to verify that all workspaces that are stored with IBM belong to an active {{site.data.keyword.cloud_notm}} account. If no active account is found, the workspace and all personal information are deleted. 
