---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-08"

keywords: schematics blueprints, operate blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Operating Blueprint environments
{: #operate-blueprints}

Managing a cloud environment is about managing continual change. A cloud environment is not static. After deployment, Blueprint environment instances continue to evolve through changes that are implemented as updates to the Blueprint configuration.
{: shortdesc}

The {{site.data.keyword.cloud}} platform is constantly changes, adds features, and depreciates end-of-life services. Also maintains the currency of services, such as {{site.data.keyword.containerlong}} and {{site.data.keyword.databases-for-redis_full}}, {{site.data.keyword.databases-for-mongodb_full}}, and so on. {{site.data.keyword.IBM}} authored automation modules are refreshed to support new service features and to address evolving security compliance requirements. Alongside, the open source IaC tools that are used by {{side.data.keyword.bpshort}} evolve, with the new versions of Terraform, Ansible, and the supporting Terraform providers and Ansible Collections. Without the maintenance of the modules, Blueprint IaC definitions and configs, a deployed environment loose currency and cease to be manageable through {{site.data.keyword.bpshort}} IaC automation.   

## Multi-step process
{: #operate-multistep}

Changes that are expected in the solution environment to reflect new requirements, scaling up or down, rotation of API keys, certificates, and so on. Operation of a Blueprint managed environment is an iterative cycle of applying changes to the deployed environment instance to maintain currency and compliance. Changes to the deployed environment are first prepared as updates to the Blueprint definition and input values. These changes are then applied to the Blueprint environment.
{: shortdesc}

The process flow to update a Blueprint environment is illustrated in a diagram.

![Blueprint definition and input value application update](../images/sc-bp-operate.svg){: caption="Blueprint definition and input value application update" caption-side="bottom"}

1. Edit the Blueprint definition `and/or` the input value YAML files to implement your changes to the solution environment. 
    - Push the updated Blueprint definition and input files to your Git repositories. If needed, create a Git version release tag for version management. 
2. Update your Blueprint configuration for the application environment in {{site.data.keyword.bpshort}}. 
    - On the update operation, the new Git release tag or branch must be specified for the modified Blueprint definition and input value files. If no version or branch is specified, {{site.data.keyword.bpshort}} automatically checks the Git repository for a recent commit, and runs a `pull latest` to pull in any updates. For more information, see [Update Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update).
3. On a successful update, {{site.data.keyword.bpshort}} automatically reinitializes the modules with updated input values and version changes to the IaC module code.  
4. The modules that have pending changes of code or inputs are highlighted by {{site.data.keyword.bpshort}}. The changes to the Blueprint environment can be deployed with the `blueprint apply` command or UI apply operation.
    - Based on your Blueprint configuration, {{site.data.keyword.bpshort}} creates a deployment plan and runs the IaC modules in dependency order to update the solution environment.
    - For each module, it runs a Terraform Apply or Ansible playbook to create, modify, or delete cloud resources as determined by the configuration changes from the update. For more information, see [Apply Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-install).  
5. On successful deployment, the defined Blueprint output values are updated with the changed module outputs.

## Next steps
{: #operate-nextsteps}

The final next stage of deleting the Blueprints is [Deleting Blueprint IaC managed environments](/docs/schematics?topic=schematics-delete-blueprints). 
