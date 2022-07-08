---

copyright:
  years: 2016, 2022
lastupdated: "2022-07-08"

keywords: glossary, IBM Cloud schematics glossary, terms, definitions, schematics glossary

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Glossary
{: #glossary}

This glossary provides terms and definitions for {{site.data.keyword.bpshort}} objects, such as `Action`, `Agent`, `Blueprint`, `Catalog`, `Inventory`, `Job`, `Template or Modules`, and `Workspace`.
{: shortdesc}

The following cross-references are used in this glossary:

- *See* refers you from a nonpreferred term to the preferred term or from an abbreviation to the spelled-out form.
- *See also* refers you to a related or contrasting term.

## Actions
{: #glossary-actions}

Automate resource and application management with Ansible playbook.

## Blueprints
{: #glossary-blueprint}

It the {{site.data.keyword.bpshort}} service used to deploy and manage large-scale environments created from Infrastructure as Code (IaC) building blocks. 

### Blueprint
{: #bpb1}

A Blueprint is a specific instance of a repeatable solution architecture, customised with input values to a specific use case. It is also the term used when you are working on specific Blueprint in {{site.data.keyword.bpshort}} created from an initial [Blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3) with customised input values.  

### Blueprint definition
{: #bpb2}

[Blueprint definitions](/docs/schematics?topic=schematics-blueprint-definitions) define a solution architecture out of IaC automation building blocks written in HashiCorp Terraform, or Red Hat Ansible. These files are written in YAML with a minimum of syntax that specifies the automation modules to be used in the definition, their versions, source libraries, and relationships for passing resource dependency data between modules. The term 'blueprint' may be used as shorthand for the Blueprint definition file. 


### Blueprint configuration
{: #bpb3}

Are the initial settings provided by the user when a Blueprint is first created in {{site.data.keyword.bpshort}}. The configuration defines the Blueprint definition to be used, input files and any dynamic input variables. 

### Blueprint environment
{: #bpb4}

The set of {{site.data.keyword.cloud_notm}} resources created by a Blueprint and its associated Workspaces. 

### Blueprint Modules
{: #bpb5}

Blueprints are composed from IaC automation modules implemented in HashiCorp Terraform, or Red Hat Ansible. Both Terraform configs and Terraform modules can be used as Blueprint Modules.    

### Blueprint dynamic inputs
{: #bpi1}

Dynamic inputs are used at Blueprint create time to pass inputs to dynamically customise the Blueprint and over ride inputs from an a version controlled input file sourced from a Git repo. They can be used to pass input values that would be a security exposure if written to a Git repository.

### Blueprint Jobs
{: #bpj1}

Blueprints operations (command) are executed as jobs by {{site.data.keyword.bpshort}}. [Blueprints job commands](/docs/schematics?topic=schematics-review-blueprint) shows the value of the output variables defined in a Blueprint. 

### Blueprint Lifecycle
{: #bpl1}

Blueprints follow a lifecycle approach to deploy and manage application environments on {{site.data.keyword.cloud_notm}}. Blueprints follow a [cycle of create, update and delete operations](/docs/schematics?topic=schematics-blueprint-lifecycle-cmds). 

## Catalog
{: #glossa-catalog}

An enterprise platform for the built-in Terraform, Ansible, Helm, CloudPak, and Operator capabilities in {{site.data.keyword.bpshort}} to set up an IaC.

## Inventories
{: #glossa-inventory}

The collection of hosts that you can run your Ansible playbook.

### Resource inventory
{: #rir1}

A resource inventory defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources where you want to run Ansible playbooks, modules, or roles by using {{site.data.keyword.bpshort}} Actions.

## Jobs
{: #glossary-job}

Job maintains a record of an automation execution of the {{site.data.keyword.bpshort}} objects such as `Workspaces`, `Actions`, `Blueprints`, `Agents`.

## Templates or Modules
{: #glossary-template}

Contains the list of ansible playbooks to perform cloud operations on target hosts.

The template exposes the  data sources to use the templates to generate strings for other Terraform resources in IaC.

Modules is a container for multiple resources or templates that are used together in IaC.

## Workspaces
{: #glossary-workspace}

A platform to build the infrastructure, provision resources and deploy applications with support to multiple environments with feasibility or previlige to access using Git private or public repositories with secured access token.

### {{site.data.keyword.bpshort}} Workspaces
{: #wkss1}

A platform to build the infrastructure, provision resources and deploy applications with support to multiple environments with feasibility or previlige to access using Git private or public repositories with secured access token.

### Jobs
{: #wksj1}

The status of the provisioned resource is in `inactive`, `progress`, `active`, or `failed` results of an executed {{site.data.keyword.bpshort}} objects performing it by execution `create`, `plan`, or `apply`.

### Resources
{: #wksr1}

Maps to the provisioned Terraform resources within the infrastructure.

### Readme
{: #wksr2} 

Refers to the demonstration of an example template with steps to prerequisites, provision resources with limitations, and steps to execute terraform template with support of IBMCloud Schematics.

### Settings
{: #wkss1} 

Common area to edit values of the variables so that new terraform template can be generated or re-imply with new changes.

### Workspace
{: #wksw1}

A platform to build the infrastructure by using Open Source tools such as `Terraform`, `Ansible`, `Helm`, `Open Shift` cloud to manage various cloud infrastructure resources.
