---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics blueprints, reusable, scaling, large, large-scale, reuse, modules

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Scaling environments
{: #blueprint-scaling}

As infrastructures scale, and resources number in the hundreds if not thousands, best practice is to break large Terraform configurations into smaller deployable units. {{site.data.keyword.bpshort}} Blueprints enables application environments to continue to scale with an orchestration framework. The Blueprints automation framework allows large environments to be created from (smaller) linked environments and managed as a single unit. 
{: shortdesc}

Compared to working with large monolithic IAC configurations, building and managing environments composed from smaller discrete linked environments has several benefits: 

- It is safer to perform changes. Changes are localized to a single environment that can be deployed and updated independently
- The blast-radius of a change is reduced. Thus limiting the impact of unintended changes or failures. 
- Smaller changes are easier to reason with and understand the impact 
- Execution time is reduced as changes can be targeted to individual environments. 
- Reuse of IaC code is easier as functionality can be separated from its configuration into reusable components 

The two approaches commonly employed of breaking large architectures down into smaller units to improve the scaling and management are:

- Linking environments (workspaces) using Terraform remote-state data sources. 

- Using an orchestration framework like the open-source [Terragrunt](https://terragrunt.gruntwork.io/){: external) tool to manage the deployment and dependency management of linked Terraform workspaces. 


{{site.data.keyword.bpshort}} Blueprints takes the orchestration approach to scaling environments. This builds on the IaC best practice of modular architectures and the reusable modules in the {{site.data.keyword.cloud_notm}} module library. The benefits and challenges of these two approaches to scaling are discussed in the following sections. 


## Remote state data sources
{: #blueprint-scaling-remotestate}

[Remote-state data sources](/docs/schematics?topic=schematics-remote-state) as implemented in Terraform and {{site.data.keyword.bpshort}} play an important role in enabling sharing of resource information between Terraform workspaces and teams. 

Data sources allow responsibility for different elements of infrastructure to be delegated to different teams with information shared between workspaces (and teams) as read-only resources. Using data sources, common infrastructure resources like the components of a network backbone, can be shared between environments and teams without risk of unintended change. 

One of the downsides of embedding remote_state data sources into Terraform automation code is the impact on the reusability of the modules across environments. Modules are specific to the remote-state they expect to receive as input and are dependent on the remote_state already existing. 

More importantly for large environments, there is a lack of visibility of the remote-state dependency between workspaces. Consequently there is no dependency management, sequencing of module deployments or notification of dependency changes.  

The solution is to externalize the dependencies and use orchestration.  


## Orchestration and modules
{: #blueprint-scaling-orchestration}

The alternative to data sources, is to make the passing of resource information between environments and workspaces explicit. Resource dependencies between workspaces are externalized at the workspace level. Here an orchestration framework performs dependency management, determining execution order and the passing of resource data between workspaces. This is the approach to scaling adopted by open source solutions like [Terragrunt](https://terragrunt.gruntwork.io/){: external) and other IaC automation frameworks, including {{site.data.keyword.bpshort}} Blueprints.     

{{site.data.keyword.bpshort}} Blueprints uses an orchestration framework to enable scaling that allows complex infrastructures to composed from smaller deployable modules. The framework allows a group of discrete modular environments to be deployed and managed by a user as a whole. A blueprint template defines the composition and structure of the larger solution environment by defining dependencies between modules and deployed modular environments. The modules specify their dependencies as inputs, that are satisfied at execution time by the orchestrator.  

This design principle for clearly defined interfaces between IaC modules follows the standard programming paradigm for reusable modules. The use of data-sources is not encouraged with blueprints as this exposes the internal module implementation and creates implementation specific dependencies between modules. Dependencies which are difficult to manage using semantic versioning.     

The blueprint approach to module composition by {{site.data.keyword.bpshort}} Blueprints builds on the [IBM module authoring guidelines](https://terraform-ibm-modules.github.io/documentation/#/implementation-guidelines){: external}. These guidelines support composition though well defined module input and output definitions for passing resource dependency data that are compatible across modules. The {{site.data.keyword.IBM_notm}} Cloud reusable Terraform modules in the [terraform-ibm-modules](https://github.com/terraform-ibm-modules){: external} GitHub repo and the Terraform registry adhere to these guidelines to support composition. 




The use of blueprint templates to compose and deploy large-scale environments from modules is discussed in more detail in these sections:
- [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates)
- [Using Terraform modules with blueprint templates](/docs/schematics?topic=schematics-blueprint-terraform) 
- [Infrastructure as code modular architectures](/docs/schematics?topic=schematics-iac-bp-modularity)


