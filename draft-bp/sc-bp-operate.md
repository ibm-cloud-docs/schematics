---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-12"

keywords: schematics blueprints, operate blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Operating Blueprint environments
{: #operate-blueprints}

Operating a cloud environment is about managing continual change. Cloud environments are not static. User infrastructure requirements change and the {{site.data.keyword.cloud}} platform are constantly evolving. Without maintenance and updates of the Blueprint IaC definitions, inputs and modules, a deployed environment loses currency, compliance, and cease to be manageable through {{site.data.keyword.bpshort}} IaC automation. 
{: shortdesc}

- The {{site.data.keyword.cloud}} platform is constantly adding features, and depreciating end-of-life services. It also maintains the currency of services, for example {{site.data.keyword.containerlong}} and the databases {{site.data.keyword.databases-for-redis_full}} and {{site.data.keyword.databases-for-mongodb_full}} continually move to new versions. 
- Alongside, the open source IaC tools that are used by {{side.data.keyword.bpshort}} evolve, with the new versions of Terraform, and the supporting Terraform providers.
- As the platform evolves, {{site.data.keyword.IBM}} authored automation modules are refreshed to support new service features, maintain currency and to address evolving security compliance requirements.
- Changes are also expected in the solution environment to reflect new user requirements, scaling up or down, rotation of API keys, certificates and more. 

Operation of a Blueprint environment is an iterative cycle of applying changes to the deployed environment to maintain currency and compliance.   


## Updating deployed environments
{: #operate-multistep}

After deployment, Blueprint environments will continue to evolve through managed change that is implemented as updates to the Blueprint definition, IaC modules, and inputs.

Changes to the deployed environment are first prepared as updates to the Blueprint definition and input values. These changes are then applied to the Blueprint environment. The two-step process ensures controlled application of change first to the {{site.data.keyword.bpshort}} Blueprint configuration and definition, then second to the cloud resources.  
{: shortdesc}

During this operate lifecycle stage, the Blueprint might be updated many times. Changes are applied to the cloud resources to satisfy changing application requirements. Or to maintain platform currency and compliance as security policies evolve. Additionally scheduled operations can run compliance checks, and runs drift detection on the environment. 

As noted earlier, {{site.data.keyword.IBM}} authored automation modules are maintained and refreshed by {{site.data.keyword.IBM}} to support new service features, maintain {{site.data.keyword.cloud}} currency and to address evolving security compliance requirements. It is suggested that Blueprint configurations, and definitions are regularly updated to use the current version of modules and these updates are applied to Blueprint environments. The risk of not setting regular updates is that environments lose currency, compliance, and cease to be manageable through {{site.data.keyword.bpshort}} IaC automation. 

Review the section on Blueprint version management to understand how to manage change to Blueprint definitions and input YAML files by using Git tags and branches.  


The two-step process flow to update a Blueprint environment is illustrated in the diagram.

![Blueprint update flow](../images/sc-bp-operate.svg){: caption="Blueprint update flow" caption-side="bottom"}

1. Edit the Blueprint definition, and the input value YAML files to implement your changes to the solution environment. 
    - Push the updated Blueprint definition and input files to your Git repositories. If needed, create a new Git version release tag for version management. 
2. Update your Blueprint configuration for the environment in {{site.data.keyword.bpshort}}. 
    - While versioning, the new Git release tag or branch must be specified for the modified Blueprint definition and input value YAML files. If no version or branch is specified, {{site.data.keyword.bpshort}} automatically checks the Git repository for a recent commit, and runs a `pull latest` to pull in any updates. For more information, see [Update Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update).
3. On a successful configuration update, {{site.data.keyword.bpshort}} automatically reinitializes the modules (Workspaces) with any updated input values and updates to the IaC module code.  
4. Apply the updated configuration and definitions. The modules that have pending IaC code changes, or inputs are highlighted by {{site.data.keyword.bpshort}}. The changes to the Blueprint environment are applied with the `blueprint apply` command or UI Apply operation.
    - Based on your updated Blueprint configuration, {{site.data.keyword.bpshort}} creates a deployment plan and runs the IaC modules in dependency order to update the environment.
    - For each module, it runs a Terraform Apply to create, modify, or delete cloud resources as determined by the configuration changes from the update. For more information, see [Apply Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-install).  
5. On successful deployment of the updates, the Blueprint output values are updated with any changed outputs.

## Next steps
{: #operate-nextsteps}

The final next stage of working with Blueprints is [Deleting Blueprint environments](/docs/schematics?topic=schematics-delete-blueprints). 
