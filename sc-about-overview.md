---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics capabilities, schematics overview, benefits

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} overview 
{: #learn-about-schematics} 

{{site.data.keyword.bpshort}} is an {{site.data.keyword.cloud_notm}} service, that delivers [Infrastructure as Code](/docs/schematics?topic=schematics-infrastructure-as-code) (IaC) tools as a service. You can use the capabilities of {{site.data.keyword.bpshort}} to consistently deploy and manage your cloud infrastructure environments. From a single pane of glass, you can run end-to-end automation to build one or more stacks of cloud resources, manage their lifecycle, manage changes in their configurations, deploy your app workloads, and perform day-2 operations.
{: shortdesc}

## IaC automation as-a-service
{: #sc-IaCaas} 

Building on open-source [Ansible](https://www.ansible.com/){: external}, [Terraform](https://www.terraform.io/){: external} and related technologies like Git and Helm, {{site.data.keyword.bplong}} provides a powerful set of [IaC](/docs/schematics?topic=schematics-infrastructure-as-code) tools as a service to program your cloud infrastructure.

An IaC approach to infrastructure provisioning and automation improves consistency, speeds deployments, reduces manual errors, and avoids undocumented or ad hoc configuration changes.

With IaC, configuration files define your infrastructure, which also makes it easier to edit, share, and reuse configurations. By codifying your infrastructure, you provision the same environment every time avoiding undocumented, ad hoc configuration changes.
Review the section on [IaC best practices](/docs/schematics?topic=schematics-infrastructure-as-code#iac-best-practices) to learn more about the core IaC principles and best practices that you can adopt when using {{site.data.keyword.bpshort}}. 


## {{site.data.keyword.bpshort}} IaC offerings
{: #sc-offerings}

{{site.data.keyword.bpshort}} builds on open-source Ansible and Terraform to provide a powerful set of IaC tools as a service to program your cloud infrastructure. With {{site.data.keyword.bpshort}} you can use this rich set of IaC automation capabilities to build stacks of cloud resources, manage their lifecycle, manage changes in their configurations, deploy your app workloads, and perform day-2 operations.
{: shortdesc}

The four core {{site.data.keyword.bpshort}} offerings are:  



### {{site.data.keyword.bpshort}} Blueprints
{: #sc-blueprints}

{{site.data.keyword.bpshort}} Blueprints brings [infrastructure as code (IaC) practices](/docs/schematics?topic=schematics-infrastructure-as-code) to the creation and lifecycle management of large-scale cloud environments. Blueprints takes large environments from their initial creation, through maintenance and ops to final decommissioning and clean up of all allocated resources. 
{: shortdesc} 

It enables users to define and deploy cloud environments using modules of reusable and well-defined [Terraform](https://www.terraform.io) automation code. For more information about Blueprints and its features, see [{{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro).
{: shortdesc}

### {{site.data.keyword.bpshort}} Workspaces
{: #sc-workspaces}

With {{site.data.keyword.bpshort}} Workspaces, use Terraform to automate the [provisioning and configuration management](/docs/schematics?topic=schematics-schematics-open-projects) of your {{site.data.keyword.cloud_notm}} resources, and rapidly build, duplicate, and scale complex, multitiered cloud environments. For more information, see [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-sch-workspaces).
{: shortdesc}

### {{site.data.keyword.bpshort}} Actions
{: #sc-actions}

With {{site.data.keyword.bpshort}} Actions, use Ansible playbooks to perform complex day-2 operations on your cloud resources, cloud environment, and app workloads. Whether you want to deploy multitiered apps, start or stop virtual servers or clusters, rotate keys, backup and restore app data, perform security scans, manage database schemas, or manage users, simply specify the tasks that you want to run in your playbook, and let {{site.data.keyword.bpshort}} securely connect and complete the tasks.For more information about managing {{site.data.keyword.bpshort}} Actions and its features, see [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sc-actions).
{: shortdesc}

### {{site.data.keyword.bpshort}} Agents
{: #sc-agents}

Agents extends and integrates {{site.data.keyword.bpshort}} workspaces that running in your private network to the {{site.data.keyword.bplong_notm}} service. Agents provisions, configure, and operate your private or on-premises cloud cluster resources without any time, network, or software restrictions. For more information about architecture and features of Agents, see [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro).
{: shortdesc}

## Benefits of using Schematics
{: #sc-benefits}

You do not need to install the open source projects on your machine or learn their API and command-line. You need to simply point {{site.data.keyword.bpshort}} to your IaC code repository and let {{site.data.keyword.bpshort}} run the specified tasks.
{: shortdesc}

| Benefits | Description |
| --- | --- |
| The open source projects used by {{site.data.keyword.bpshort}} | Terraform, Ansible, Helm provisioning engine, and execution platform are tested, maintained, and monitored by {{site.data.keyword.IBM_notm}}. {{site.data.keyword.IBM_notm}} automatically applies the latest security standards and patches to {{site.data.keyword.bpshort}} to ensure reliability and availability of the service. You do not need to manually apply updates to the {{site.data.keyword.bpshort}} platform.|
|All versions are tested by {{site.data.keyword.IBM_notm}}. |As new versions of Terraform and Ansible become available, {{site.data.keyword.IBM_notm}} begins with hardening and testing these versions, so that they can be supported in the {{site.data.keyword.bpshort}} platform. For more information, see [when are new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-actions-faq#new-versions) |
|{{site.data.keyword.bpshort}} is fully integrated with IAM | You can use service access roles to control who can access and collaborate on your workspaces and actions, or roll out changes. You can invite {{site.data.keyword.cloud_notm}} users to your account and leverage IAM access groups to streamline the access assignment process in your organization. As a multi-tenant solution, {{site.data.keyword.bpshort}} creates all resources in your personal account. Resources are not shared or reused by other {{site.data.keyword.cloud_notm}} tenants. Because {{site.data.keyword.bpshort}} is built on Kubernetes, IAM service access roles are mapped to role-based access controls (RBAC) in Kubernetes to enforce resource isolation within your account.|
|Full {{site.data.keyword.IBM_notm}} support for the open-source tools and plug-ins related to {{site.data.keyword.cloud_notm}} | {{site.data.keyword.bpshort}} is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue by using the `{{site.data.keyword.terraform-provider_full_notm}}` or the Ansible modules for {{site.data.keyword.cloud_notm}}, you can [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-using-avatar#getting-support).|
{: caption="{{site.data.keyword.bpshort}} benefits" caption-side="bottom"}

