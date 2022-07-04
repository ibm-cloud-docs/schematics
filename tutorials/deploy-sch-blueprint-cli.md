---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-04"

keywords: deploy schematics blueprint, blueprint cli deployment, deploy schematics blueprint cli, 

subcollection: schematics

content-type: tutorial
services: schematics
account-plan:
completion-time: 60m

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Deploying {{site.data.keyword.bpshort}} Blueprints using the command-line
{: #deploy-schematics-blueprint-cli}
{: toc-content-type="tutorial"}
{: toc-services="schematics"}
{: toc-completion-time="60m"}

Use an {{site.data.keyword.IBM}} provided sample to deploy a scalable multi-Workspace {{site.data.keyword.bpshort}} Blueprint from the command-line.
{: shortdesc}

In this tutorial, you will deploy a {{site.data.keyword.bpshort}} Blueprints environment by using the {{site.data.keyword.bpshort}} command-line. The tutorial demonstrates the Blueprints lifecycle of creating cloud resources with a Blueprints managed cloud environment before deleting the resources and the {{site.data.keyword.bpshort}} Blueprint.

Before your begin

- Install and login to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- Select the {{site.data.keyword.cloud_notm}} region you wish to use to manage your {{site.data.keyword.bpshort}} Blueprints environment from. Set the region by running the command `ibmcloud target --region <us-south>`.
- Install the [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) plug-in, or [update the command line plug-in](/docs/schematics?topic=schematics-setup-cli#schematics-cli-update) to access the {{site.data.keyword.bpshort}} Blueprints commands.
- Check that you have the right [permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

## Select a Blueprint definition
{: #select-schematics-blueprint-cli}
{: step}

This tutorial uses a [sample Blueprint](https://github.com/Cloud-Schematics/blueprint-basic-example){: external} to create two {{site.data.keyword.bpshort}} Workspaces containing a resource group and the {{site.data.keyword.cos_full}} bucket.

The sample Blueprint takes two input parameters, `provision_rg=false`, and `resource_group_name=Default`. These configure the Blueprint to retrieve the ID of the Default resource group. See the example README file for alternative parameters to create a new resource group. 

## Create Blueprints in {{site.data.keyword.bpshort}}
{: #create-schematics-blueprint-cli}
{: step}

Create your Blueprints environment by using the [`ibmcloud schematics blueprint create`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-create) command. The following parameters are used to create a Blueprint.

- Name of the blueprint: `Blueprint_basic`
- {{site.data.keyword.bpshort}} management resource group: `Default`
- Blueprint URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Blueprint file: `basic-blueprint.yaml`
- Blueprint Git branch `main`
- Input file URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Input file: `basic-input.yaml` 
- Input Git branch `main`
- Inputs: 
    - `provisiong_rg`=**false**
    - `resource_group_name`=**Default**

### Syntax
{: #step1-syntax}

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=false,resource_group_name=Default
```
{: pre}

### Output
{: #step1-output}

```text
Created Blueprint ID: eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936

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

The output shows that the Workspaces `basic-resource-group` and `basic-cos-storage` have been created and are now in `Inactive` status to indicate that the Workspaces are initialized and ready for Terraform operations to be performed.

Record the generated ID of the Blueprint to use in the later commands.
{: important}

## Installing Blueprints to create cloud resources
{: #install-schematics-blueprint-cli}
{: step}

Here you can use the [`ibmcloud schematics blueprint install`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-create) command to perform Terraform Apply operations using the Terraform configurations specified in the Blueprint definition. The operation will create cloud resources. Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprints install -id Blueprints-Starter-Sample.c579f31d
```
{: pre}

### Output
{: #step3-output}

```text
Modules to be installed
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   INACTIVE   
2     Workspace   basic-cos-storage      INACTIVE   
      
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.40d9cb67
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:0    Active:2    Failed:0   
Type        Name                      Status               Job ID   
Blueprint   Blueprint Basic Example   FULFILMENT_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.40d9cb67   
Workspace   basic-resource-group      ACTIVE                  
Workspace   basic-cos-storage         ACTIVE                  
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 fulfilment_success at Mon Jun 27 16:23:25 BST 2022
OK
```
{: screen}

The output shows that the Workspaces `basic-resource-group` and `basic-cos-storage` are now in `Active` status to indicate that Terraform resources have been created by an `Apply` operation.

If the resource group specified in the input parameters already exists, the Terraform `Apply` for the create of the resource group will fail. This is the expected Terraform behaviour. The Blueprint command output will identify the Workspace `Apply` failure and will return a summary of the Terraform error to the user.

## Reviewing Blueprints and its output
{: #review-schematics-blueprint-cli}
{: step}

Run the [`ibmcloud schematics blueprint get`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-get) command to display the details about the Blueprint and the deployed environment. Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint get -id blueprint_id
```
{: pre}

The computed output values of the Blueprint can be retrieved by using the `--output` option.

```sh
ibmcloud schematics blueprint get -id blueprint_id --output
```
{: pre}

### Output
{: #step4-output}

```text
BLUEPRINT          
                
Name            Blueprint Basic Example   
ID              eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936   
Description     Simple two module blueprint. Deploys Resource Group and COS bucket   
Status          Normal   
Location        eu-de   
Creator         steve_strutt@uk.ibm.com   
Last modified   2022-06-27T15:22:15.199Z   
                
MODULES
SNO   Workspace Name         Workspace ID                                    Status   Location   Updated   
1     basic-resource-group   eu-de.workspace.basic-resource-group.e55b17d4   ACTIVE   eu-de         
2     basic-cos-storage      eu-de.workspace.basic-cos-storage.dc0af714      ACTIVE   eu-de         
      
BLUEPRINT OUTPUTS
Key      Value   
cos_id   583a2e76-135f-45c8-aaad-ea5661fda56e   
            
OK
```
{: screen}

## Destroying Blueprints cloud resources
{: #destroy-schematics-blueprint-cli}
{: step}

Run the [`ibmcloud schematics blueprint destroy`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-destroy) command for the Blueprint environment to destroy all the cloud resources that are created in earlier steps. 
* Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.
* When prompted reply `yes`, or `y` to destroy the cloud resources.

```sh
ibmcloud schematics blueprints destroy -id Blueprints-Starter-Sample.c579f31d
```
{: pre}

### Output
{: #step5-output}

```text
Modules to be destroyed
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   ACTIVE   
2     Workspace   basic-cos-storage      ACTIVE   
      
Do you really want to destroy all the resources of blueprint eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936? [y/N]> y
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.62942aa1
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   
Type        Name                      Status               Job ID   
Blueprint   Blueprint Basic Example   FULFILMENT_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.62942aa1   
Workspace   basic-resource-group      INACTIVE                
Workspace   basic-cos-storage         INACTIVE                
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 fulfilment_success at Mon Jun 27 16:43:30 BST 2022
OK
```
{: screen}


## Deleting the Blueprint
{: #delete-schematics-blueprint-cli}
{: step}

Run the [`ibmcloud schematics blueprint delete`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-delete) command for the Blueprints environment to remove the Blueprint created in earlier steps. 
* Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.
* When prompted reply `yes`, or `y` to delete the cloud resources.

```sh
ibmcloud schematics blueprint delete -id us-east.ENVIRONMENT.Blueprints-Starter-Sample.c579f31d
```
{: pre}

### Output
{: #step6-output}

```sh
Modules to be deleted
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   INACTIVE   
2     Workspace   basic-cos-storage      INACTIVE   
      
Do you really want to delete the blueprint ? [y/N]> y
Job : eu-de.JOB.Blueprint-Basic-Example.7c0cb235 Created

Job Type: BLUEPRINT DELETE

OK
```
{: pre}

## Next steps
{: #nextsteps-schematics-blueprint-cli}

You have now deployed a {{site.data.keyword.bpshort}} Blueprint. By completing this tutorial, you've learned to take a Blueprint through its lifecycle of creation to retirement by creating multiple Workspaces, cloud resources, and then destroying the created cloud environment.  

Looking for more samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint).

