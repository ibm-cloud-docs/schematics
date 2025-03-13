---

copyright:
  years: 2016, 2025
lastupdated: "2025-03-13"

keywords: terminology, IBM Cloud schematics terminology, terms, definitions, schematics terminology

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Glossary terms for {{site.data.keyword.bpshort}}
{: #sch-terms}

This page provides the terms and definitions for {{site.data.keyword.bpshort}} objects, such as `action`, `Agent`, `Blueprint`, `Catalog`, `Inventory`, `Job`, `Template or Modules`, and `workspace`.
{: shortdesc}

The following cross-references are used in terminology:

- `See` refers you from a non-preferred term to the preferred term or from an abbreviation to the spelled-out form.
- `See also` refers you to a related or contrasting term.

## Actions
{: #sch-terms-actions}

Automate resource and application management with Ansible playbooks.
{: shortdesc}

## Agents
{: #sch-terms-agents}

Extends {{site.data.keyword.bpshort}} ability to reach user infrastructure.
{: shortdesc}

### Agent
{: #agentsa1}

Method or service to bind {{site.data.keyword.bpshort}} Agent with {{site.data.keyword.bpshort}} workspaces.
{: shortdesc}

### Agent service
{: #agentsa2}

A collection of Agent related microservices deployed on the Agent infrastructure. It is composed of the following microservices.

- `Jobrunner` microservice
- `Sandbox` microservice
- `Runtimeworkspace` Job
{: shortdesc}

### Agent Infrastructure
{: #agentsa3}

A Kubernetes cluster used to deploy and run the Agent services. It is composed of the following resources.

- VPC infrastructure as `public_gateways`, subnets
- Kubernetes Service as vpc_kubernetes_cluster
- IBM LogDNA instance 
{: shortdesc}



## Catalog
{: #sch-terms-catalog}

A collection of automation templates that can be ordered from {{site.data.keyword.cloud_notm}}. You can onboard your Terraform automation to the {{site.data.keyword.cloud_notm}} catalog, and share the catalog of templates in a controlled manner with your team by using IAM permissions and policies. 

{{site.data.keyword.cloud_notm}} catalog already supports a collection of {{site.data.keyword.IBM_notm}} owned and Third party developed automation in the catalog. The automation is used to provision infrastructure and softwares by using Helm charts, Kubernetes Operators, OVA images, `Cloudpak` automation. {{site.data.keyword.bpshort}} workspaces are used to run these automations.|
{: shortdesc}

## Inventories
{: #sch-terms-inventory}

A collection of cloud resources that are used as target for running the Ansible playbooks, modules, or roles.
{: shortdesc}

### Resource inventory
{: #rir1}

A resource inventory can be defined by using a static inventory file, or dynamically retrieve to your target Cloud resources from {{site.data.keyword.bpshort}} workspaces in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

## Jobs
{: #sch-terms-job}

A job maintains a record of the execution of tasks or operations for {{site.data.keyword.bpshort}} `Workspaces`, `Actions`, `Agents` and resources. You can see these job records appearing in the context of `action`, and `workspace`. The job record describes the status of the Job, inputs used to run the job, outputs produced by the job and the console logs.
{: shortdesc}

## Templates or Modules
{: #sch-terms-template}

Automation code written for provisioning and configuring a cloud infrastructure by using Terraform, Ansible, Helm, Operators, and so on., in the IaC language. </br> You can use {{site.data.keyword.bpshort}} to run the automation templates by using workspaces or Actions. 
{: shortdesc}

Modules are reusable IaC automation code that can be used to assemble an automation template

## Workspaces
{: #sch-terms-workspace}

A platform to build the infrastructure, provision resources, and deploy applications with the support to multiple environments. It has feasibility, or privilege to access by using Git private or public repositories with a secured access token.
{: shortdesc}

### {{site.data.keyword.bpshort}} workspaces
{: #wkss1}

A platform to build the infrastructure, provision resources and deploy applications with support to multiple environments. It has feasibility, or privilege to access by using Git private or public repositories with a secured access token.
{: shortdesc}

### Jobs
{: #wksj1}

The status of the provisioned resource is in `inactive`, `progress`, `active`, or `failed` results of the {{site.data.keyword.bpshort}} object by running `create`, `plan`, or `apply` commands.
{: shortdesc}

### Resources
{: #wksr1}

Maps to the provisioned Terraform resources within the infrastructure.
{: shortdesc}

### Readme file
{: #wksr2} 

Refers to the demonstration of an example template with steps to prerequisites, provision resources with limitations, and steps to run the Terraform template with support of {{site.data.keyword.bplong_notm}}.
{: shortdesc}

### Settings
{: #wkss2} 

Common area to edit values of the variables so that new Terraform template can be generated or re-imply with new changes.
{: shortdesc}

### Workspace
{: #wksw1}

A platform to build the infrastructure by using open source tools such as `Terraform`, `Ansible`, `Helm`, `Open Shift` cloud to manage various cloud infrastructure resources.
{: shortdesc}
