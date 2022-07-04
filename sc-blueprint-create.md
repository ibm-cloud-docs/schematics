---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-04"

keywords: blueprint create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Creating a Blueprint 
{: #create-blueprint}

Deploying cloud resources with Blueprints is a two step process, create and install. The first step creates a Blueprint in {{site.data.keyword.bpshort}}. The second [install](/docs/schematics?topic=schematics-sc-blueprint-install) step executes the IaC automation modules to deploy cloud resources. Refer to [Blueprints lifecycle](https://test.cloud.ibm.com/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands create, update and delete and the Blueprints lifecycle. 

The first step in deploying cloud resources is the creation of a Blueprint in {{site.data.keyword.bpshort}}. This saves the Blueprint configuration for future operations. The Blueprint config specfies the Git source and release of the Blueprint definition and Input files, and any optional input values that will be used to create cloud resources. A linked Workspace is created for each module in the Blueprint definition, initialised from the modules' IaC source repository and module inputs. 
{: shortdesc}


## Creating a Blueprint from the CLI 
{: #create-blueprint-cli}
{: cli}

To create and deploy your Blueprint with the CLI, use the `ibmcloud schematics blueprint create` command. This command requires a name and the Git URL of a Blueprint definition and other optional arguments. For a complete listing of options, see the [ibmcloud schematics blueprint create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.

Before your begin:

- Install and login to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- Select the {{site.data.keyword.cloud_notm}} region you wish to use to manage your {{site.data.keyword.bpshort}} Blueprints environment from. Set the region by running the command `ibmcloud target --region <us-south>`.
- Install the [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) plug-in, or [update the command line plug-in](/docs/schematics?topic=schematics-setup-cli#schematics-cli-update) to access the {{site.data.keyword.bpshort}} Blueprints commands.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

The following command creates a Blueprint by using the definition file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. This Blueprint definition requires the two  inputs `provision_rg=true` and `resource_group_name=test_rg` are passed. 

**Syntax:**

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=test_rg
```
{: screen}


On successful completion the create command will return **create_success** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint.  

For more information, about the command options, see [Create command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create).

## Verfiy Blueprint creation 
{: #bp-verify-create}

Verify that the Blueprint has been created successfully. When you create the Blueprint from the CLI, the command displays details of the linked Workspaces to be created and an continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initalising the Workspaces. The command only returns on completion.

```text
Created Blueprint ID: eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
Modules to be created
SNO   Type        Name   
1     Workspace   basic-resource-group   
2     Workspace   basic-cos-storage   
      
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.da1b13ca
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   

Type        Name                      Status           Job ID   
Blueprint   Blueprint Basic Example   CREATE_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.da1b13ca   
Workspace   basic-resource-group      INACTIVE            
Workspace   basic-cos-storage         INACTIVE            
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 create_success at Mon Jun 27 16:18:47 BST 2022
OK
```
{: screen}

On successful completion the create command will return **create_success** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint. 

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-init-fails).

## Next steps
{: #bp-create-nextsteps}

After creating the Blueprint in {{site.data.keyword.bpshort}}, the next step in deploying the cloud resources defined by the Blueprint is to [install](/docs/schematics?topic=schematics-install-blueprint) the Blueprint. 
