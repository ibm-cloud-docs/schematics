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

- `See` refers you from a nonpreferred term to the preferred term or from an abbreviation to the spelled-out form.
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

{{site.data.keyword.bpshort}} blueprints is a pattern-based deployment and lifecycle management service for large-scale cloud environments. It builds on the {{site.data.keyword.bpshort}} Workspace support for Infrastructure as Code (IaC) and Hashicorp Terraform. See [Working with blueprints](/docs/schematics?topic=workingwithblueprints) for details of how to use blueprints and Terraform to create large-scale environments from solution patterns. When using the service, {{site.data.keyword.bpshort}} users create a [blueprint](/docs/schematics?topic=schematics-glossary#bpb1) to deploy and manage the cloud resources that are specified by a blueprint template.   

{{site.data.keyword.IBM}}

### Blueprint
{: #bpb1}

A blueprint is the resource in the {{site.data.keyword.bpshort}} a user works with to manage the cloud environment and resources that are created from a blueprint template. The blueprint resource in {{site.data.keyword.bpshort}} stores the details of the template and the specific configuration details. All operations against the deployed cloud resources are performed using the blueprint resource in {{site.data.keyword.bpshort}} blueprints. 

The blueprint (resource) maintains the record of operations set against the cloud environment, status, the cloud resources deployed, and the [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2). It defines the solution pattern and infrastructure architecture, and the unique input values that are used to configure the environment.

A blueprint is created from a [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The set of cloud resources deployed by a blueprint is referred to as a [blueprint environment](/docs/schematics?topic=schematics-glossary#bpb4). 

### Blueprint template
{: #bpb2}

A [Blueprint template](/docs/schematics?topic=schematics-bp-template-schema-yaml) defines the infrastructure architecture, topology, and cloud resources for a solution pattern. The definition implements the wanted solution architecture from reusable [automation modules that are written in Terraform. Definition files are written in YAML and specify the Terraform [automation modules](/docs/schematics?topic=schematics-glossary#bpb5) to be used, their versions, Git source libraries, and the relationships and dependencies between modules. 
{: shortdesc}

### Blueprint configuration
{: #bpb3}

A blueprint configuration is the initial settings that the user must provide to create a blueprint (resource) in {{site.data.keyword.bpshort}}. The configuration defines the blueprint template YAML file to be used, its Git source location, input value files, document version information, and any more inputs.  
{: shortdesc}

### Blueprint environment
{: #bpb4}

A blueprint environment is the set of {{site.data.keyword.cloud_notm}} resources that are created from a [Blueprint definition](/docs/schematics?topic=schematics-glossary#bpb2) and inputs that are specified by a [Blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3).  
{: shortdesc}

### Blueprint automation modules
{: #bpb5}

Blueprint definitions are composed from IaC automation modules that are implemented in HashiCorp Terraform. Automation modules can be implemented by using fully operable Terraform configurations ([root modules](https://www.terraform.io/language/modules#the-root-module)) or by using Terraform ([child](https://www.terraform.io/language/modules#child-modules)). Examples of {{site.data.keyword.IBM_notm}} authored modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.
{: shortdesc}

### Blueprint inputs
{: #bpi1}

Inputs are specified at blueprint config create time to pass inputs to dynamically customize the blueprint and over-ride inputs from a version-controlled input file that is sourced from a Git repo. They can be used to pass input values that would be a security exposure if written to a Git repository.

### Blueprint jobs
{: #bpj1}

Blueprints operations (command) are run as jobs by {{site.data.keyword.bpshort}}. [Blueprints job commands show the value of the output variables that are defined in a Blueprint.
{: shortdesc}

### Blueprint lifecycle
{: #bpl1}

Blueprints follow a lifecycle approach to deploying and managing {{site.data.keyword.cloud_notm}} environments. Blueprints and their environments follow a lifecycle of definition, deployment, operation, and deletion. See [Working with blueprints](/docs/schematics?topic=workingwithblueprints). 
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

Common area to edit values of the variables so that new Terraform template can be generated or reimply with new changes.
{: shortdesc}

### Workspace
{: #wksw1}

A platform to build the infrastructure by using open source tools such as `Terraform`, `Ansible`, `Helm`, `Open Shift` cloud to manage various cloud infrastructure resources.
{: shortdesc}
