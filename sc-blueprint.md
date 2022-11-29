---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-29"

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
{: shortdesc}

In a similar fashion, {{site.data.keyword.bpshort}} Blueprints enables users to define and deploy cloud environments using modules of reusable and well-defined [Terraform](https://www.terraform.io) automation code. This builds on the IaC best practice of [modular architectures](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity), where reusable modules implement the layers and components of an infrastructure architecture from well designed, tested and compliant Terraform code.

![Deploying modular large-scale environments with Blueprints](/images/new/bp-overview.svg){: caption="Deploying modular large-scale environments with Blueprints" caption-side="bottom"}

Blueprint templates determine the architecture, specifying the modules required for the implementation and infrastructure topology. {{site.data.keyword.bpshort}} deploys the modules as independent environments, managing their lifecycle and the passing of resource information.

Reuse and maintainability are key features. Publicly available modules designed for {{site.data.keyword.cloud}} can be [combined with third-party and user developed modules](/docs/schematics?topic=schematics-blueprint-terraform) to create customized solutions. Separately maintained input configurations, ensure reusability as blueprint templates can be configured at deploy time to create dev, stage and prod pipelines, allowing for sharing and reuse across organizations.

Modular composition and configuration ease the task of maintaining and updating cloud environments. Deployed modules and templates can be upgraded using versions, rather than by code change. IBM modules are regularly refreshed to maintain platform compatibility and meet current compliance requirements. Module [semantic versioning](https://semver.org/){: external} identifies the scope of any changes.

## Features
{: #blueprint-features}

Blueprints is built around [IaC best practices](/docs/schematics?topic=schematics-infrastructure-as-code#iac-best-practices) to manage the lifecycle of cloud environments. It provides cradle-to-grave environment management, versioned deployments, modular and parameterized configuration support for controlled change to environments, and as templates and modules are updated to remain current and compliant. 
{: shortdesc}

![{{site.data.keyword.bpshort}} Blueprints feature overview](/images/new/bp-features.svg){: caption="{{site.data.keyword.bpshort}} Blueprints feature overview" caption-side="bottom"}

{{site.data.keyword.bpshort}} Blueprints complements Terraform with IaC based environment management capabilities:

- [Modular composition](/docs/schematics?topic=schematics-blueprint-terraform): Build infrastructure architectures from an eco-system of reusable IBM Cloud automation modules written in Terraform
- [Reusability](/docs/schematics?topic=schematics-blueprint-reuse-pipelines): Reuse templates and modules across environments, pipelines and teams
- Scalability: Scale environments by linking discrete modular environments as the layers and components of large and complex application architectures.
- [Lifecycle](/docs/schematics?topic=schematics-work-with-blueprints): Manage environments cradle-to-grave, from initial creation, through maintenance and ops to final decommissioning. Future: scheduled ops, drift detection, cost estimation, policy compliance.

## Next steps
{: #nextsteps-bp-arch}

So far you have learned a little about {{site.data.keyword.bpshort}} Blueprints. The following are some next steps to explore.
{: shortdesc}

- [Working with blueprints and environments](/docs/schematics?topic=schematics-work-with-blueprints) to understand how to use blueprints to manage the lifecycle of deploying and managing cloud environments.
- See [understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) to dig into how to define cloud environments using versioned blueprint templates and inputs. 
- [Beta code for {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand beta limitations.
