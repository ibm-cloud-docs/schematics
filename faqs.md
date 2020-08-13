---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-13"

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
{:support: data-reuse='support'}

# FAQs
{: #faqs}

Answers to common questions about the {{site.data.keyword.bplong_notm}} service.
{:shortdesc}


## What is {{site.data.keyword.bplong_notm}} and how does it work? 
{: #what-is-schematics}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} delivers Terraform-as-a-Service so that you can use a high-level scripting language to model the resources that you want in your {{site.data.keyword.cloud_notm}} environment, and enable Infrastructure as Code (IaC). For more information, see [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics).

## What is Terraform? 
{: #what-is-terraform}
{: faq}
{: support}

Terraform is an open-source software that is developed by HashiCorp that enables predictable and consistent provisioning of cloud platform and infrastructure resources by using a high-level scripting language. You can use Terraform to automate your {{site.data.keyword.cloud_notm}} resource provisioning, rapidly build complex, multi-tier cloud environments, and enable Infrastructure as Code (IaC).

## What is Infrastructure as Code?
{: #what-is-iac}
{: faq}

Infrastructure as Code (IaC) helps you codify your cloud environment so that you can automate the provisioning and management of your resources in the cloud. Rather than manually provisioning and configuring infrastructure resources or using scripts to adjust your cloud environment, you use a high-level scripting language to specify your resource and its configuration. Then, you use tools like Terraform to provision the resource in the cloud by leveraging its API. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring.

## What am I charged for when I use {{site.data.keyword.bpshort}}?
{: #charges}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} workspaces are provided to you at no cost. However, when you decide to apply your Terraform template in {{site.data.keyword.cloud_notm}} by clicking **Apply plan** from the workspace details page or running the `ibmcloud terraform apply` command, you are charged for the {{site.data.keyword.cloud_notm}} resources that are described in your Terraform template. Review available service plans and pricing information for each resource that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first.

## Can I run Ansible playbooks with {{site.data.keyword.bpshort}}?
{: #ansible-playbooks}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run Ansible playbooks or Ansible actions against your {{site.data.keyword.cloud_notm}} by using the Ansible provisioner in your Terraform configuration file. For example, use the Ansible provisioner to deploy software on {{site.data.keyword.cloud_notm}} resources or perform actions against your resources, such as shutting down a virtual server instance. For more information about how to use the Ansible provisioner, see the following blogs:

- [Discover best-practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/){: external}
- [Learn about repeatable and reliable end-to-end app provisioning and configuration](https://developer.ibm.com/articles/application-deployment-with-redhat-ansible-and-ibm-cloud-schematics/){: external}

## Does {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions?
{: #provider-versions}
{: faq}
{: support}

Yes, {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider executes latest version is `1.10.0`, and previous four versions such as `1.9.0`, `1.8.1`, `1.8.0`, `1.7.1` are supported. For more information, about the provider configuration, see [Provider configuration](https://www.terraform.io/docs/configuration/providers.html).

Example for a multiple provider configuration:

```
terraform{
  required_providers{
   ibm = ">= 1.6" // Error !! version unavailable.
   ibm = ">= 1.7.0" // Execute against latest version
   ibm = "== 1.8.0" // Executes v1.8.0.
   ibm = ">= 1.9.0" // Executes against latest version, currently, it is v1.10.0, in future it can be higher version.
  }
}

```
{: pre}

