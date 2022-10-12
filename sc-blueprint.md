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

{{site.data.keyword.bpshort}} blueprints assists DevOps teams to deploy and build large-scale and repeatable application environments. It builds on existing and tested {{site.data.keyword.bpshort}} automation capabilities. 
{: shortdesc} 

The core principles of IaC are commonly defined as:
- Codify everything
- All version and in the source control
- Continuously test, integrate, and deploy
- Make your infrastructure code modular

{{site.data.keyword.bpshort}} blueprints applies these IaC principles to manage the definition and lifecycle of large-scale HashiCorp Terraform environments. The definition and linking of {{site.data.keyword.bpshort}} hosted Terraform environments enables {{site.data.keyword.bpshort}} to simplify the creation and management of large-scale infrastructure deployments on {{site.data.keyword.cloud_notm}}. 

This approach to large-scale environment management is represented by the key concepts that are outlined in the diagram.

![Managing lLarge-scale environments using Terraform and blueprints](images/bp-largescale-env.svg){: caption="Managing large-scale environments using Terraform and blueprints" caption-side="bottom"}

## Architecture
{: #blueprint-architecture}

The key to building scalable cloud architectures with {{site.data.keyword.bpshort}} blueprints is open source IaC automation modules. {{site.data.keyword.cloud_notm}} automation modules are reusable IaC definitions that implement the layers of an infrastructure stack as HashiCorp Terraform configurations. To simplify creation of cloud environments using blueprints, automation modules are purposely developed to a set of [guidelines](https://github.com/terraform-ibm-modules/getting-started/blob/master/README.md){: external} for resource naming conventions, variable definitions, inputs, and outputs.
{: shortdesc} 

In {{site.data.keyword.bpshort}}, blueprint modules are deployed as linked {{site.data.keyword.bpshort}} (Terraform) Workspaces. {{site.data.keyword.bpshort}} manages data handling between the linked Workspaces based on the resource dependencies between the modules. The linking of the modules defines the infrastructure architecture and resource topology.  

The mapping of a blueprint template with input variables, and automation modules to the {{site.data.keyword.bpshort}} Workspaces, and deployed cloud resources is illustrated in the diagram. 

![{{site.data.keyword.bpshort}} blueprints architecture](images/bp-architecture.svg){: caption="{{site.data.keyword.bpshort}} blueprints architecture" caption-side="bottom"}

Module statements in a blueprint template define the modules that creates in the respective source repositories that contain the Terraform module configurations. The layers of an infrastructure architecture and resource dependencies are created from the dependencies and links between the modules. When {{site.data.keyword.bpshort}} blueprints deploys an environment,  resource data is passed between modules to link the layers of the archirecture. 

## Next steps
{: #nextsteps-bp-arch}

So far you learned a little about {{site.data.keyword.bpshort}} blueprints, its architecture, and advantages. Following are the next steps to explore.

- [Working with blueprints](/docs/schematics?topic=workingwithblueprints) to configure blueprint templates and use blueprint commands to deploy environments.
- See [blueprint permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to set access permissions to run the blueprint commands.
- Explore [deploying blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a blueprint.
- [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions.
- [Beta code for {{site.data.keyword.bpshort}} blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand Beta limitations.
