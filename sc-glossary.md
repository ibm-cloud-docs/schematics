---

copyright:
  years: 2016, 2022
lastupdated: "2022-10-11"

keywords: glossary, IBM Cloud schematics glossary, terms, definitions, schematics glossary

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Glossary
{: #glossary}

This glossary provides terms and definitions for {{site.data.keyword.bpshort}} objects, such as `Action`, `Agent`, `Blueprint`, `Catalog`, `Inventory`, `Job`, `Template or Modules`, and `Workspace`.
{: shortdesc}

The following cross-references are used in this glossary:

- `See` refers you from a non-preferred term to the preferred term or from an abbreviation to the spelled-out form.
- `See also` refers you to a related or contrasting term.

## Actions
{: #glossary-actions}

Automate resource and application management with Ansible playbook.
{: shortdesc}

## Agents
{: #glossary-agents}

Extends {{site.data.keyword.bpshort}} ability to reach user infrastructure.
{: shortdesc}

### Agent
{: #agentsa1}

Method or service to bind {{site.data.keyword.bpshort}} Agents with {{site.data.keyword.bpshort}} Workspaces.
{: shortdesc}

### Agent service
{: #agentsa2}

A collection of Agent related microservices deployed on the Agent infrastructure. It is composed of the following microservices.
- `Jobrunner` microservice
- `Sandbox` microservice
- `RuntimeWorkspace` Job
{: shortdesc}

### Agent Infrastructure
{: #agentsa3}

A Kubernetes cluster used to deploy and run the Agent services. It is composed of the following resources.
- VPC infrastructure as `public_gateways`, subnets
- Kubernetes Service as vpc_kubernetes_cluster
- IBM LogDNA instance 
{: shortdesc}

## Blueprints
{: #glossary-blueprint}

{{site.data.keyword.bplong}} blueprints is an [Infrastructure as Code](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac) (IaC) deployment and lifecycle management service for large-scale cloud environments. It utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout and the major building blocks. A craftsman builts the house from well defined components using the blueprint for guidance. 

It builds on the {{site.data.keyword.bpshort}} Workspace support for Infrastructure as Code (IaC) and Hashicorp Terraform. See [Working with blueprints](/docs/schematics?topic=workingwithblueprints) for details of how to use blueprints and Terraform to create large-scale environments from solution architectures. When using the service, {{site.data.keyword.bpshort}} users create a [blueprint](/docs/schematics?topic=schematics-glossary#bpb1) to deploy and manage the cloud resources that are specified by a [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2).   
{: shortdesc}

### Blueprint
{: #bpb1}

A blueprint is the resource in {{site.data.keyword.bpshort}} a user works with to manage the environment and cloud resources created from a [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2) and config. The blueprint resource in {{site.data.keyword.bpshort}} records the details of the template and the specific [configuration](/docs/schematics?topic=schematics-glossary#bpb3) details. All operations against the deployed cloud resources are performed using the blueprint resource or ID. 

The blueprint (resource) maintains the record of operations run, job status, the cloud resources deployed, and the [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2). It defines the infrastructure architecture and the unique input values that are used to configure the environment.

A blueprint is created with a [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The set of cloud resources deployed by a blueprint is referred to as a [blueprint environment](/docs/schematics?topic=schematics-glossary#bpb4).
{: shortdesc} 

### Blueprint template
{: #bpb2}

A [blueprint template](/docs/schematics?topic=schematics-bp-template-schema-yaml) defines the infrastructure architecture, topology, and cloud resources for a solution architecture. The template implements the desired architecture from reusable [modules](/docs/schematics?topic=schematics-glossary#bpb5) that are written in Terraform. Template files are written to a [YAML schema](/docs/schematics?topic=schematics-bp-template-schema-yaml) and specify the Terraform [automation modules](/docs/schematics?topic=schematics-glossary#bpb5) to be used, their versions, Git source libraries, and the relationships and dependencies between modules. Template files versioned and sourced from a version control system, GitHub or GitLab.   
{: shortdesc}

### Blueprint configuration
{: #bpb3}

A blueprint configuration is the initial settings that the user provides to create a blueprint in {{site.data.keyword.bpshort}}. The configuration defines the [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2) YAML file to be used, its Git source location, [input files](/docs/schematics?topic=schematics-glossary#bpi2), file version information, and additional inputs.  
{: shortdesc}

### Blueprint environment
{: #bpb4}

A blueprint environment is the set of {{site.data.keyword.cloud_notm}} resources that are created from a [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2) and inputs that are specified by a [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3).  
{: shortdesc}

### Blueprint modules
{: #bpb5}

Blueprint templates are composed from IaC automation modules that are implemented in HashiCorp Terraform. Automation modules can be implemented by using fully operable Terraform configurations ([root modules](https://www.terraform.io/language/modules#the-root-module)) or by using Terraform ([child](https://www.terraform.io/language/modules#child-modules)). Examples of {{site.data.keyword.IBM_notm}} authored (child) modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.
{: shortdesc}

### Blueprint inputs
{: #bpi1}

A blueprint template, optionally declares a set of input variables that can be used to customize the blueprint template, while deploying or managing a blueprint environment.  The template metadata for the input variables include the following: variable name, variable type, default value, variable description, sensitive, readonly, hidden. 

Blueprint inputs can be provided as:
- User-defined input, provided via the Schematics API, CLI or UI at config create time. They can be used to pass input values that would be a security exposure if written to a Git repository.
- Version-controlled [blueprint input file]((/docs/schematics?topic=schematics-glossary#bpi2)) (from a Git repository)
{: shortdesc}

### Blueprint input files
{: #bpi2}

Version-controlled input file (from a Git repository) to pass inputs to customize the blueprint template for a specific environment. The type of an input variable is defined meta data in the template file. 
{: shortdesc}

### Blueprint jobs
{: #bpj1}

Blueprint operations (commands) are run as jobs by {{site.data.keyword.bpshort}}. A blueprint job keeps a record of the status, inputs, outputs, and files of every blueprint config, plan & run command execution.
{: shortdesc}

### Blueprint lifecycle
{: #bpl1}

Blueprints follow a lifecycle approach to deploying and managing {{site.data.keyword.cloud_notm}} environments. The tasks of working with a blueprint over its lifecycle can be grouped under four headings, [defining blueprints](/docs/schematics?topic=schematics-define-blueprints), [deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints), [updating and operating blueprints](/docs/schematics?topic=schematics-update-blueprints), and [deleting blueprints](/docs/schematics?topic=schematics-delete-blueprints). See the section [working with blueprints](/docs/schematics?topic=workingwithblueprints#blueprint-lifecycle) for more details. 
{: shortdesc}

## Catalog
{: #glossa-catalog}

An enterprise platform for the built-in Terraform, Ansible, Helm, CloudPak, and Operator capabilities in {{site.data.keyword.bpshort}} to set up an IaC.
{: shortdesc}

## Inventories
{: #glossa-inventory}

The collection of hosts that you can run your Ansible playbook.
{: shortdesc}

### Resource inventory
{: #rir1}

A resource inventory defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources against which you want to run Ansible playbooks, modules, or roles when the {{site.data.keyword.bpshort}} Actions are used.
{: shortdesc}

## Jobs
{: #glossary-job}

A job maintains a record of the execution of tasks or operations for {{site.data.keyword.bpshort}} resources, `Workspaces`, `Actions`, `Blueprints`, and `Agents`.
{: shortdesc}

## Templates or Modules
{: #glossary-template}

Contains the list of ansible playbooks to set cloud operations on target hosts.
{: shortdesc}

The template displays the data sources to use the templates to generate strings for other Terraform resources in IaC.

Modules are a container for multiple resources or templates that are used together in IaC.

## Workspaces
{: #glossary-workspace}

A platform to build the infrastructure, provision resources, and deploy applications with the support to multiple environments. It has feasibility, or privilege to access by using Git private or public repositories with a secured access token.
{: shortdesc}

### {{site.data.keyword.bpshort}} Workspaces
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
