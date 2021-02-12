---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-12"

keywords: ansible playbook, ansible playbook example, vsi start stop, reboot vsi on {{site.data.keyword.cloud_notm}}

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

---

# Provisioning {{site.data.keyword.cloud_notm}} resources with Terraform
{: #tf-how-it-works}
{: help}
{: support}

Review how {{site.data.keyword.bplong_notm}} provisions and manages your {{site.data.keyword.cloud_notm}} infrastructure and platform resources with Terraform. 
{: shortdesc}

<img src="images/schematics_flow.png" alt="Provisioning {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}}" width="800" style="width: 800px; border-style: none"/>

1. **Codify your {{site.data.keyword.cloud_notm}} resources**. Use Terraform HashiCorp Configuration Language (HCL) or JSON format to specify the {{site.data.keyword.cloud_notm}} resources that you want to provision in your {{site.data.keyword.cloud_notm}} environment. If you are not familiar with Terraform, you can select one of the default Terraform templates that {{site.data.keyword.bplong_notm}} provides to provision the {{site.data.keyword.cloud_notm}} resources that you want. Terraform templates can be stored in a GitHub or GitLab repository to ensure source control and enable collaboration, review, and auditing in your organization. You can save usage information in `readme` files to make the template shareable and usable across multiple teams. You can also upload tape archive files (`.tar`) from your local machine to provide {{site.data.keyword.bpshort}} with your template.
2. **Create your workspace**. You can point your {{site.data.keyword.bplong_notm}} workspace to a GitHub or GitLab repository where you store your Terraform template, or provide your template by uploading a `.tar` file. Workspaces help to organize resources that belong to one {{site.data.keyword.cloud_notm}} environment. For example, use workspaces to separate your test, staging, and production environment. With {{site.data.keyword.cloud_notm}} Identity and Access Management, you can control who has access to your resources and can provision or manage these resources in your {{site.data.keyword.cloud_notm}} account. 
3. **Create an execution plan**. {{site.data.keyword.bplong_notm}} uses the `terraform plan` command to parse the configuration files of the provided Terraform template, and to create a summary of actions that need to be performed to achieve the state that is described in your configuration files. To determine the actions, {{site.data.keyword.bplong_notm}} takes into account the resources that are already provisioned in your {{site.data.keyword.cloud_notm}} account to give you a preview whether resources must be added, modified, or removed. You can review the plan and any validation errors by reviewing the logs.  
4. **Provision your resources**. To create, modify, or remove resources from your {{site.data.keyword.cloud_notm}} account, {{site.data.keyword.bplong_notm}} uses the `terraform apply` command. This command calls the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, which is aware of the API for each resource to provision, configure, or remove the resource. 

