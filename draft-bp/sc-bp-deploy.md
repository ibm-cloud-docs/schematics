---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-08"

keywords: schematics blueprints, deploy blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying Blueprint environments
{: #deploy-blueprints}

Deploying a Blueprint environment from a Blueprint definition and custom input values is a two-step operation in {{site.data.keyword.bpshort}}.
{: shortdesc} 

- `Create the Blueprint in {{site.data.keyword.bpshort}} for the environment instance:` The Blueprint configuration specifies the source of the Blueprint definition in the Git and the input values to customize the environment instance. This step validates the supplied Blueprint definition and the inputs. All the IaC automation modules are imported from source repositories and linked {{site.data.keyword.bpshort}} Workspaces are initialised to manage the lifecycle of the IaC module.
- `Apply the Blueprint:` {{site.data.keyword.bpshort}} runs the IaC module automation code in dependency order to create the solution environment and cloud resources.   

The deployment process is illustrated in the diagram.

![Blueprint definition and input value application deployment](../images/sc-bp-deploy.svg){: caption="Blueprint definition and input value application deployment" caption-side="bottom"}

1. Prepare the input configuration for the Blueprint. This specifies the source of the Blueprint definition in Git and the input values to customize the environment instance. 
2. Create your Blueprint for the solution environment in {{site.data.keyword.bpshort}}. For more information, see [Creating Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
    - This step retrieves the definition and input values from Git and runs validation. 
3. On successful Blueprint creation, {{site.data.keyword.bpshort}} automatically initializes its definitions and creates a set of linked Workspaces to manage the resources of the solution environment. Each Workspace represents a single IaC automation module in the Blueprint definition. Review the saved configuration of the Blueprint.    
4. The Blueprint environment can be deployed with the `blueprint apply` command or UI Apply operation. For more information, see [Apply Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-install) command.  
    - Based on your Blueprint configuration, {{site.data.keyword.bpshort}} creates a deployment plan and runs the IaC modules in dependency order to create the environment instance. 
    - For each module, it runs a Terraform Apply or Ansible playbook to create cloud resources. 
5. On successful deployment, the defined Blueprint output values are returned. 

These can be used as input to further configuration steps or used directly to access resources in the application environment.   

## Next steps
{: #deploy-nextsteps}

The next stage of working with Blueprints is [Operating Blueprint IaC managed environments](/docs/schematics?topic=schematics-operate-blueprints).

