---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics blueprints, blueprints, blueprints architecture

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# {{site.data.keyword.bpshort}} Blueprints
{: #blueprint-intro}

{{site.data.keyword.bpshort}} Blueprints brings [infrastructure as code (IaC) practices](/docs/schematics?topic=schematics-infrastructure-as-code) to the creation and lifecycle management of large-scale and complex cloud environments. Blueprints manages operations across multiple environments to scale cloud environments from their initial creation, through maintenance and ops to final decommissioning and clean up of all allocated resources. 
{: shortdesc} 

## Overview
{: #blueprint-overview}

{{site.data.keyword.bplong}} Blueprints is an Infrastructure as Code (IaC) automation solution for large-scale cloud environments. It utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, the major building blocks and standard components. Using the blueprint for guidance, a builder can confidently build the house from the set of well-defined components.
{: shortdesc}

In the same way, {{site.data.keyword.bpshort}} Blueprints enables users to define and deploy complex cloud environments using modules of reusable and well-defined [Terraform](https://www.terraform.io) automation code. This builds on the IaC best practice of [modular architectures](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity). It [scales the Terraform deployment model](/docs/schematics?topic=schematics-blueprint-scaling), by connecting modular IaC environments, as the layers and components of large infrastructure architectures. The definition of an architecture using modules and deployment as linked environments is illustrated. 

![Deploying modular large-scale environments with Blueprints](/images/new/bp-overview.svg){: caption="Deploying modular large-scale environments with Blueprints" caption-side="bottom"}

A blueprint template determines the infrastructure architecture, specifying the modules required for the implementation, the infrastructure topology and relationships between modules. Inputs customize the template for the target deployment. Blueprint operations build up the infrastructure by deploying the smaller modular environments defined by the template. Blueprints linking the module environments into the whole, by the sharing and passing of resource dependency data between modules.  

Reuse and maintainability are key features to deploy at scale. Publicly available modules designed for {{site.data.keyword.cloud}} can be [combined with third-party and user developed modules](/docs/schematics?topic=schematics-blueprint-terraform) to create customized solutions. Separately maintained input configurations, ensure reusability as blueprint templates can be configured at deploy time to create customized dev, stage and prod pipelines, or repeatable application deployments.  

This modular approach to composition and configuration also eases the task of maintaining and updating large environments. Blueprint operations update environments on request through configuration of module versions, rather than by code change. IBM module libraries are regularly refreshed and versioned to maintain platform compatibility and meet current compliance requirements. Module [semantic versioning](https://semver.org/){: external} identifies the scope of any changes, allowing environments to be automatically refreshed with minor patches and revisions, while maintaining user control over major feature enhancements.  

## Features
{: #blueprint-features}

Blueprints is built around [IaC best practices](/docs/schematics?topic=schematics-infrastructure-as-code#iac-best-practices) to manage the lifecycle of large cloud environments. It provides cradle-to-grave management for modular environments. Modular and parameterized configuration support for reuse. Versioned deployments to control change to environments, as templates and modules are updated to meet new requirements and remain current, and compliant. 
{: shortdesc}

These features are illustrated under the four headings below. 


![{{site.data.keyword.bpshort}} Blueprints feature overview](/images/new/bp-features.svg){: caption="{{site.data.keyword.bpshort}} Blueprints feature overview" caption-side="bottom"}

{{site.data.keyword.bpshort}} Blueprints complements Terraform with IaC based environment management capabilities:

- [Modular composition](/docs/schematics?topic=schematics-blueprint-terraform): Build infrastructure architectures from an eco-system of reusable {{site.data.keyword.cloud_notm}} automation modules written in Terraform
- [Scalability](/docs/schematics?topic=schematics-blueprint-scaling): Scale environments by linking discrete modular environments as the layers and components of large and complex application architectures using dependencies.
- [Reusability](/docs/schematics?topic=schematics-blueprint-reuse): Share and reuse templates and modules across environments, pipelines and teams     
- [Lifecycle](/docs/schematics?topic=schematics-work-with-blueprints): Manage environments cradle-to-grave, from initial creation, through maintenance and ops to final decommissioning. 
    - Future: scheduled ops, drift detection, cost estimation, policy compliance               


## Next steps
{: #nextsteps-bp-arch}

So far you have learned a little about {{site.data.keyword.bpshort}} Blueprints. The following are some next steps to explore.
{: shortdesc}

- [Working with blueprints and environments](/docs/schematics?topic=schematics-work-with-blueprints) to understand how to use blueprints to manage the lifecycle of deploying and managing cloud environments.
- See [understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) to dig into how to define cloud environments using versioned blueprint templates and inputs. 
- [Beta code for {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand beta limitations.
