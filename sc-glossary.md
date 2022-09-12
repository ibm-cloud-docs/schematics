---

copyright:
  years: 2016, 2022
lastupdated: "2022-09-12"

keywords: glossary, IBM Cloud schematics glossary, terms, definitions, schematics glossary

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Glossary
{: #glossary}

This glossary provides terms and definitions for {{site.data.keyword.bpshort}} objects, such as `Action`, `Agent`, `Blueprint`, `Catalog`, `Inventory`, `Job`, `Template or Modules`, and `Workspace`.
{: shortdesc}

The following cross-references are used in this glossary:

- *See* refers you from a non preferred term to the preferred term or from an abbreviation to the spelled-out form.
- *See also* refers you to a related or contrasting term.

## Actions
{: #glossary-actions}

Automate resource and application management with Ansible playbook.

## Agents
{: #glossary-agents}

Extends {{site.data.keyword.bpshort}} ability to reach user infrastructure.

### Agent
{: #agentsa1}

Method or service to bind {{site.data.keyword.bpshort}} Agents with {{site.data.keyword.bpshort}} Workspaces.

### Agent service
{: #agentsa2}

A collection of Agent related microservices deployed on the Agent infrastructure. It is composed of the following microservices.
- Jobrunner microservice
- Sandbox microservice
- RuntimeWorkspace Job

### Agent Infrastructure
{: #agentsa3}

A Kubernetes cluster used to deploy and run the Agent services. It is composed of the following resources.
- VPC infrastructure as `public_gateways`, subnets
- Kubernetes Service as vpc_kubernetes_cluster
- IBM LogDNA instance 

## Blueprints
{: #glossary-blueprint}

{{site.data.keyword.bpshort}} Blueprints is a pattern based deployment and lifecycle management service for large scale cloud environments. It builds on the proven {{site.data.keyword.bpshort}} Workspace support for Infrastructure as Code (IaC) and Hashicorp Terraform. See [Working with Blueprints](/docs/schematics?topic=workingwithblueprints) for details of how to use Blueprints and Terraform to create large scale environments from solution patterns. In the Blueprints service, {{site.data.keyword.bpshort}} users create a [Blueprint](/docs/schematics?topic=schematics-glossary#bpb1) to deploy and manage the cloud resources specified by a solution pattern.   

{{site.data.keyword.IBM}}

### Blueprint
{: #bpb1}

A Blueprint is the resource in {{site.data.keyword.bpshort}} a user works with to manage the cloud environment and resources created from a Blueprint soluton pattern. The Blueprint resource in {{site.data.keyword.bpshort}} stores the details of the solution pattern and the specific configuration details. All operations against the deployed cloud environment are performed using the Blueprint resource in {{site.data.keyword.bpshort}} Blueprints. 

The Blueprint resource in {{site.data.keyword.bpshort}} maintains the record of operations performed against the cloud environment, current status, the cloud resources deployed, the [Blueprint defintion](/docs/schematics?topic=schematics-glossary#bpb2) that defines the solution pattern and infrastructure architecture, and the unique input values used to configure the environment.   

A Blueprint is created from a [Blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The set of deployed cloud resources is referred to as a [Blueprint environment](/docs/schematics?topic=schematics-glossary#bpb4). 

### Blueprint definition
{: #bpb2}

A [Blueprint definition](/docs/schematics?topic=schematics-blueprint-definitions) defines the infrastructure architecture, topology and cloud resources for a solution pattern. The definition implements the desired solution architecture from reusable [automation modules](/docs/schematics?topic=schematics-glossary#bpb5) written in Terraform. Definition files are written in YAML and specify the Terraform [automation modules](/docs/schematics?topic=schematics-glossary#bpb5) to be used, their versions, Git source libraries, and the relationships and dependencies between modules. 


### Blueprint configuration
{: #bpb3}

A Blueprint configuration is the initial settings the user must provide to create a Blueprint (resource) in {{site.data.keyword.bpshort}}. The configuration defines the Blueprint definition YAML file to be used, its Git source location, input value files, document version information and any additional inputs.  

### Blueprint environment
{: #bpb4}

A Blueprint environment is the set of {{site.data.keyword.cloud_notm}} resources created from a [Blueprint defintion](/docs/schematics?topic=schematics-glossary#bpb2) and inputs specified by a [Blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3).  

### Blueprint automation modules
{: #bpb5}

Blueprint definitions are composed from IaC automation modules implemented in HashiCorp Terraform. Automation modules can be implemented using fully operable Terraform configurations ([root modules](https://www.terraform.io/language/modules#the-root-module)) or using Terraform ([child](https://www.terraform.io/language/modules#child-modules)). Examples of {{site.data.keyword.IBM_notm}} authored modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.

### Blueprint inputs
{: #bpi1}

Inputs are specified at Blueprint create time to pass inputs to dynamically customize the Blueprint and over-ride inputs from a version controlled input file sourced from a Git repo. They can be used to pass input values that would be a security exposure if written to a Git repository.

### Blueprint Jobs
{: #bpj1}

Blueprints operations (command) are executed as jobs by {{site.data.keyword.bpshort}}. [Blueprints job commands](/docs/schematics?topic=schematics-list-blueprint-jobs-cli&interface=cli) shows the value of the output variables defined in a Blueprint. 

### Blueprint Lifecycle
{: #bpl1}

Blueprints follow a lifecycle approach to deploying and managing {{site.data.keyword.cloud_notm}} environments. Blueprint environments follow a lifecycle of definition, deployment, operation and deletion. See [Working with Blueprints](/docs/schematics?topic=workingwithblueprints). 
{: shortdesc}

## Catalog
{: #glossa-catalog}

An enterprise platform for the built-in Terraform, Ansible, Helm, CloudPak, and Operator capabilities in {{site.data.keyword.bpshort}} to set up an IaC.

## Inventories
{: #glossa-inventory}

The collection of hosts that you can run your Ansible playbook.

### Resource inventory
{: #rir1}

A resource inventory defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources against which you want to run Ansible playbooks, modules, or roles when using {{site.data.keyword.bpshort}} Actions.

## Jobs
{: #glossary-job}

A job maintains a record of the execution of tasks or operations for {{site.data.keyword.bpshort}} resources, `Workspaces`, `Actions`, `Blueprints` and `Agents`.

## Templates or Modules
{: #glossary-template}

Contains the list of ansible playbooks to perform cloud operations on target hosts.

The template exposes the  data sources to use the templates to generate strings for other Terraform resources in IaC.

Modules is a container for multiple resources or templates that are used together in IaC.

## Workspaces
{: #glossary-workspace}

A platform to build the infrastructure, provision resources and deploy applications with support to multiple environments with feasibility or privilege to access using Git private or public repositories with secured access token.

### {{site.data.keyword.bpshort}} Workspaces
{: #wkss1}

A platform to build the infrastructure, provision resources and deploy applications with support to multiple environments with feasibility or privilege to access using Git private or public repositories with secured access token.

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
{: #wkss2} 

Common area to edit values of the variables so that new terraform template can be generated or re-imply with new changes.

### Workspace
{: #wksw1}

A platform to build the infrastructure by using Open Source tools such as `Terraform`, `Ansible`, `Helm`, `Open Shift` cloud to manage various cloud infrastructure resources.
