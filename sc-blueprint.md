---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: schematics blueprints, blueprints, blueprints architecture

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# {{site.data.keyword.bpshort}} Blueprints
{: #blueprint-intro}

{{site.data.keyword.bpshort}} Blueprints brings [infrastructure as code (IaC) practices](/docs/schematics?topic=schematics-infrastructure-as-code) to the creation and lifecycle management of large-scale cloud environments. Blueprint operations take cloud environments from their initial creation, through maintenance and ops to final decommissioning and clean up of all allocated resources. 
{: shortdesc} 

## Overview
{: #blueprint-overview}

{{site.data.keyword.bplong}} Blueprints is an Infrastructure as Code (IaC) automation solution for large-scale cloud environments. It utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, the major building blocks and standard components. Using the blueprint for guidance, a builder can confidently build the house from the set of well-defined components.

In a similar fashion, {{site.data.keyword.bpshort}} Blueprints enables users to define and deploy cloud environments using modules of reusable and well-defined [Terraform](https://www.terraform.io) automation code. This builds on the IaC best practice of [modular architectures](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity), where reusable modules implement the layers and components of an infrastructure architecture from well designed, tested and compliant Terraform code. A blueprint template determines the architecture, specifying the modules required for the implementation and infrastructure topology. {{site.data.keyword.bpshort}} deploys the modules as independent environments, managing their lifecycle and the passing of resource information.   

Reuse and maintainability are at the heart of {{site.data.keyword.bpshort}} Blueprints. Publicly available modules designed for {{site.data.keyword.cloud}} can be combined with third-party and user developed modules to create customized solutions. Separately maintained input configurations, ensure reusability as blueprint templates can be configured at deploy time to create dev, stage and prod pipelines, allowing for sharing and reuse across organizations. 

Modular composition and configuration also simplify the task of maintaining and updating cloud environments. Modules and templates can be easily upgraded based on versions, rather than code change. IBM reusable modules are [semantically versioned](https://semver.org/){: external} as they are refreshed, to maintain platform compatibility and meet current compliance requirements. Module version control in the blueprint template, puts users in control of upgrading their environments at a time that is convenient to them and without the need for deep Terraform knowledge.      
{: shortdesc} 

![Large-scale environments by using Terraform and blueprints](/images/new/bp-largescale-env.svg){: caption="Large-scale environments using Terraform and blueprints" caption-side="bottom"}

A blueprint template defines the architecture of the environment to be deployed and the modules from which it will be constructed. When deployed, an input file passes the values that customize the template for the intended environment usage. In this example the input files would typically determine the scaling of the deployed infrastructure for the dev, stage and proc environments.      

## Features
{: #blueprint-features}

{{site.data.keyword.bpshort}} Blueprints complements Terraform's IaC automation capabilities with:
- [Composition](/docs/schematics?topic=schematics-define-blueprints): Build infrastructure architectures from an eco-system of reusable IBM Cloud automation modules written in Terraform
- [Reusability](/docs/schematics?topic=schematics-blueprint-templates): Reuse templates and modules across environments, pipelines and teams
- Scalability: Structure and scale application environments by linking discrete modular environments as the layers and components of an application architecture.    
- [Lifecycle](/docs/schematics?topic=schematics-work-with-blueprints): Cradle-to-grave operations model, from initial creation, through maintenance and ops to final decommissioning. Future: scheduled ops, drift detection, cost estimation, policy compliance               

[IaC best practices](/docs/schematics?topic=schematics-infrastructure-as-code#iac-best-practices), underpin the lifecycle of blueprint environments. Cradle-to-grave management, version control, modular a parameterized configuration, support controlled change to environments as requirements evolve, and templates and modules are maintained, and updated to remain current and compliant.   

![{{site.data.keyword.bpshort}} Blueprints overview](/images/new/bp-features.svg){: caption="{{site.data.keyword.bpshort}} Blueprints overview" caption-side="bottom"} 

## Next steps
{: #nextsteps-bp-arch}

So far you have learned a little about {{site.data.keyword.bpshort}} Blueprints. The following are some next steps to explore.

- [Working with blueprints and environments](/docs/schematics?topic=schematics-work-with-blueprints) to understand how to use blueprints to manage the lifecycle of deploying and managing cloud environments.
- See [understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) to dig into how to define cloud environments using versioned blueprint templates and inputs. 
- [Beta code for {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand beta limitations.
