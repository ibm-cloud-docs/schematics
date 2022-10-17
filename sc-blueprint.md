---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-10"

keywords: schematics blueprints, blueprints, blueprints architecture

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# {{site.data.keyword.bpshort}} blueprints
{: #blueprint-intro}

{{site.data.keyword.bpshort}} blueprints supports the creation and lifecycle management of large-scale cloud environments from reusable automation building blocks. 
{: shortdesc} 

## Overview
{: #blueprint-overview}

{{site.data.keyword.bplong}} blueprints is an [Infrastructure as Code](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac) (IaC) deployment and lifecycle management service for large-scale cloud environments. It utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout and the major building blocks. A craftsman builds the house from well defined components using the blueprint for guidance.      

In a similar fashion, {{site.data.keyword.bpshort}} blueprints enables users to rapidly define and deploy cloud environments from reusable and well defined building blocks of [Terraform](https://www.terraform.io) automation code. Reusable modules implement the layers and components of an infrastructure architecture from well designed, tested and compliant Terraform code. Templates determine the architecture, specifying the modules required for the implementation and infrastructure topology. Reuse is at the heart of {{site.data.keyword.bpshort}} blueprints. Publicly available modules designed for IBM Cloud can be mixed with third-party and user developed modules to create customized solutions. Templates are reusable across environments with separately maintained configurations, supporting dev, stage and prod pipelines and reuse across organizations. 
{: shortdesc} 


![Large-scale environments by using Terraform and blueprints](/images/bp-largescale-env.svg){: caption="Large-scale environments using Terraform and blueprints" caption-side="bottom"}


{{site.data.keyword.bpshort}} blueprints complements Terraform's IaC automation capabilities with:
- Composition: Build from an eco-system of reusable and maintained IBM Cloud architecture components written in Terraform
- Reusability: Reuse templates (architectures) across environments, pipelines and teams
- Scalability: Structure large environments by linking modules and Terraform workspaces  
- Lifecycle: Cradle-to-grave. Future: scheduled ops, drift detection, cost estimation, policy compliance  
- Extensibility (future): Provisioning and configuration with RedHat Ansible              

IAC best practices, support the lifecycle of blueprint environments, cradle-to-grave. Versioning and configuration, support controlled change as requirements evolve, while templates and modules are maintained and updated to remain current and compliant.   


![{{site.data.keyword.bpshort}} blueprints overview](/images/blueprints-v2-Overview.svg){: caption="{{site.data.keyword.bpshort}} blueprints overview" caption-side="bottom"}



## Next steps
{: #nextsteps-bp-arch}

So far you learned a little about {{site.data.keyword.bpshort}} blueprints, its architecture, and advantages. Following are the next steps to explore.

- [Working with blueprints](/docs/schematics?topic=workingwithblueprints) to configure blueprint templates and use blueprint commands to deploy environments.
- See [blueprint permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to set access permissions to run the blueprint commands.
- Explore [deploying blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a blueprint.
- [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions.
- [Beta code for {{site.data.keyword.bpshort}} blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand Beta limitations.
