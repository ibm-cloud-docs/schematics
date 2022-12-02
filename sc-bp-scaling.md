---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics blueprints, reusable, scaling, large, large-scale, reuse, modules

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Scaling environments
{: #blueprint-scaling}

As infrastructures scale, and resources number in the hundreds if not thousands, best practice is to break large Terraform configurations into smaller deployable units. Schematics Blueprints enables environments to continue to scale with an orchestration framework. This allows large environments to be created from (smaller) linked environments and managed as a single unit. 
{: shortdesc}

Compared to working with large monolithic IAC configurations, building environments from smaller discrete linked environments has several benefits: 

- It is safer to perform changes. Changes are localized to a single environment that can be deployed and updated independently
- The blast-radius of a change is reduced. Thus limiting the impact of unintended changes or failures. 
- Smaller changes are easier to reason with and understand the impact 
- Execution time is reduced as changes can be targeted to individual environments. 
- Reuse of IaC code is easier as functionality can be separated from its configuration into reusable components 

The two approaches  commonly employed to improve the scaling and management of large architectures by breaking them down into smaller units are:

- Linking environments (workspaces) using Terraform remote-state data sources. 

- Using an orchestration framework like the open-source [Terragrunt](
https://terragrunt.gruntwork.io/){: external)  tool to manage the deployment and dependency management of linked Terraform workspaces. 


Schematics Blueprints takes the orchestration approach to scaling environments. This builds on the IaC best practice of modular architectures and the IBM Cloud module library. 


## Remote state data sources
{: #blueprint-scaling-remotestate}

[Remote-state data sources](/docs/schematics?topic=schematics-remote-state) as implemented in Terraform and Schematics have an important role in enabling sharing of resource information between Terraform workspaces. 

These data sources allow responsibility for different elements of infrastructure to be delegated to different teams with information shared between workspaces (and teams) as read-only resources. They also allow for common infrastructure resources to be shared between environments and teams without risk of unintended change. 

The downside is that the use of remote_state data sources has an impact on the reusability of modules across environments. Modules are specific to the remote-state they expect to receive as input and are dependent on the remote_state already existing. Additionally there is a lack of visibility of the remote-state dependency, no clear dependency management or sequencing of module deployments. 

The solution is orchestration. 


## Orchestration and modules
{: #blueprint-scaling-orchestration}

Schematics Blueprints enables scaling using an orchestration framework that enables complex infrastructures to composed from smaller deployable modules. The framework allows a group of discrete modular environments to be deployed and managed by a user as a whole. A blueprint template defines the composition and structure of the larger solution environment by defining dependencies between modules. 

The use of blueprint templates to compose and deploy large-scale environments from modules is discussed in more detail in these sections:
- [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates)
- [Using Terraform modules with blueprint templates](/docs/schematics?topic=schematics-blueprint-terraform) 
- [Infrastructure as code modular architectures](/docs/schematics?topic=schematics-iac-bp-modularity)


