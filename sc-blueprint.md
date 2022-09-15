---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-15"

keywords: schematics blueprints, blueprints, blueprints architecture

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# {{site.data.keyword.bpshort}} Blueprints
{: #blueprint-intro}

{{site.data.keyword.bpshort}} Blueprints supports the creation and lifecycle management of large-scale cloud environments from reusable automation building blocks. 
{: shortdesc} 

## Overview
{: #blueprint-overview}

{{site.data.keyword.bpshort}} Blueprints helps DevOps teams deploy and build large-scale and repeatable application environments, by building on existing, and tested {{site.data.keyword.bpshort}} automation concepts.
{: shortdesc} 

The core principles of IaC are commonly defined as:
- Codify everything
- All version and in the source control
- Continuously test, integrate, and deploy
- Make your infrastructure code modular

{{site.data.keyword.bpshort}} Blueprints applies these IaC principles to manage the definition and lifecycle of large-scale HashiCorp Terraform environments. The definition and linking of {{site.data.keyword.bpshort}} hosted Terraform environments enables {{site.data.keyword.bpshort}} to simplify the creation and management of large-scale infrastructure deployments on {{site.data.keyword.cloud_notm}}. 

This approach to large-scale environment management is represented by the key Blueprints concepts that are outlined in the diagram.

![Large-scale environments by using Terraform and Blueprints](images/bp-largescale-env.svg){: caption="Large-scale environments using Terraform and Blueprints" caption-side="bottom"}

## Architecture
{: #blueprint-architecture}

The key to building scalable cloud architectures with {{site.data.keyword.bpshort}} Blueprints is open source IaC automation modules. {{site.data.keyword.cloud_notm}} automation modules are reusable IaC definitions implements the layers of an infrastructure stack as HashiCorp Terraform configurations. To help creating Blueprints and {{site.data.keyword.cloud_notm}} environments, automation modules are purposely developed to a set of [guidelines](https://github.com/terraform-ibm-modules/getting-started/blob/master/README.md){: external} for resource naming conventions, variable definitions, inputs, and outputs.
{: shortdesc} 

In {{site.data.keyword.bpshort}}, Blueprint modules are deployed as linked {{site.data.keyword.bpshort}} (Terraform) Workspaces. The {{site.data.keyword.bpshort}} sets data handling between the linked Workspaces based on the resource dependencies between the modules. The linking of the Workspace IaC configuration defines the solution architecture and resource topology.  

The mapping of a Blueprint definition with input variables, and automation modules to the {{site.data.keyword.bpshort}} Workspaces, and deployed in cloud resources is illustrated in the diagram. 

![{{site.data.keyword.bpshort}} Blueprints architecture](images/bp-architecture.svg){: caption="{{site.data.keyword.bpshort}} Blueprints architecture" caption-side="bottom"}

Module statements in the Blueprint definition define the Workspaces that creates in the respective source repositories that contain the Terraform module configurations. The composition of the solution is created by the dependencies and links between the modules. When the Blueprint is deployed, {{site.data.keyword.bpshort}} manages the data flows of output resource data from the modules in underneath infrastructure layers to that of the higher in the stack.

## Next steps
{: #nextsteps-bp-arch}

So far you learned a little about {{site.data.keyword.bpshort}} Blueprints, its architecture, and advantages. Following are the next steps to explore.

- [Working with Blueprints](/docs/schematics?topic=workingwithblueprints) to configure Blueprint definitions and use Blueprints commands to deploy Blueprints.
- See [Blueprints permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to set access permissions to set the Blueprint deployments.
- Explore [deploying {{site.data.keyword.bpshort}} Blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a Blueprint-managed cloud environment.
- [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions on Blueprints.
- [Beta code for {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand Beta limitations.
