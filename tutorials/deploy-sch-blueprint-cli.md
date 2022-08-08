---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-08"

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

In this tutorial, you will deploy a {{site.data.keyword.bpshort}} Blueprint environment by using the {{site.data.keyword.bpshort}} command-line. The tutorial demonstrates the Blueprints lifecycle of creating cloud resources with a Blueprint managed cloud environment before deleting the resources and the {{site.data.keyword.bpshort}} Blueprint.

Before your begin

- Install and login to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- Select the {{site.data.keyword.cloud_notm}} region you wish to use to manage your {{site.data.keyword.bpshort}} Blueprint environment. Set the region by running the command `ibmcloud target -r <us-south>`.
- Install the [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) plug-in, or [update the command line plug-in](/docs/schematics?topic=schematics-setup-cli#schematics-cli-update) to access the {{site.data.keyword.bpshort}} Blueprints commands.
- Check that you have the right [permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

## Select a Blueprint definition
{: #select-schematics-blueprint-cli}
{: step}

This tutorial uses a [sample Blueprint](https://github.com/Cloud-Schematics/blueprint-basic-example){: external} to create two {{site.data.keyword.bpshort}} Workspaces referencing an existing resource group and creating an {{site.data.keyword.cos_full}} instance and bucket.

The sample Blueprint takes two input parameters, `provision_rg=false`, and `resource_group_name=Default`. These configure the Blueprint to retrieve the ID of the Default resource group. See the example README file for alternative parameters to create a new resource group. 

## Create the Blueprint
{: #create-schematics-blueprint-cli}
{: step}

Create your Blueprint environment by using CLI command [`ibmcloud schematics blueprint create`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create). Use the following parameter to create a Blueprint.

- The Name of the Blueprint: `Blueprint_basic`
- The {{site.data.keyword.bpshort}} management resource group: `Default` **Note** you can create a new resource group for the Blueprint.
- The Blueprint URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- The Blueprint file: `basic-blueprint.yaml`
- The Blueprint Git branch `main`
- An Input file URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- An Input file: `basic-input.yaml` 
- An Input Git branch `main`
- Inputs: 
    - `provisiong_rg`=**false**
    - `resource_group_name`=**Default**

### Syntax
{: #step1-syntax}

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=default
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

Refer to [Troubleshooting Blueprint create failures](/docs/schematics?topic=schematics-bp-create-fails) for details on debugging create failures. 

## Install Blueprint to create cloud resources
{: #install-schematics-blueprint-cli}
{: step}

Here you can use the [`ibmcloud schematics blueprint install`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command to perform Terraform Apply operations using the Terraform configurations specified in the Blueprint definition. The operation will create cloud resources. Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint install -id Blueprints-Starter-Sample.c579f31d
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

The output shows that the Workspaces `basic-resource-group` and `basic-cos-storage` are now in `Active` status to indicate that the Terraform Apply has run successfully and Terraform resources have been created.

Refer to [Troubleshooting Blueprint install failures](/docs/schematics?topic=schematics-bp-install-fails) for details on debugging install failures. 

## Access and test the Blueprint created resources
{: #review-schematics-blueprint}
{: step}

After a successful installation of the Blueprint, review the deployed Blueprint via the CLI or UI.  

### Using the cloud UI 
{: #review-schematics-blueprint-ui}

From the [{{site.data.keyword.bpshort}} Blueprints list](https://cloud.ibm.com/schematics/blueprints){: external}, select the provisioned Blueprint to view the your Workspaces and cloud resources. Review UI tabs such as **Overview**, **Modules**, **Resources**, **Variables**, **Jobs history**, and **Settings** for the computed output values of the Blueprint. 

### Using the CLI
{: #review-schematics-blueprint-cli}

Run the [`ibmcloud schematics blueprint get`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-get) command to display the details about the Blueprint and the deployed environment. Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint get -id blueprint_id
```
{: pre}

The computed output values of the Blueprint can be retrieved by using the `--profile outputs` option.

```sh
ibmcloud schematics blueprint get -id blueprint_id -profile outputs
```
{: pre}

#### Output
{: #step4-output}

```text
BLUEPRINT          
                
Name            Blueprint Basic Example   
ID              eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936   
Description     Simple two module blueprint. Deploys Resource Group and COS bucket   
Status          Normal   
Location        eu-de   
Creator         sabc@uk.ibm.com   
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

When using the `-profile output` flag, the `Blueprints Outputs` section in the command output lists any output values computed by the Blueprint. A Blueprint definition can be configured to output any of the Terraform output values from the linked Workspaces.  

## Destroy Blueprint cloud resources
{: #destroy-schematics-blueprint-cli}
{: step}

Optional: Run the [`ibmcloud schematics blueprint destroy`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-destroy) command for the Blueprint environment to destroy all the cloud resources that are created in earlier steps. 
* Insert the ID saved from the [output of the create](#create-schematics-blueprint-cli) command.
* When prompted reply `yes`, or `y` to destroy the cloud resources.

```sh
ibmcloud schematics blueprint destroy -id Blueprints-Starter-Sample.c579f31d
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

## Delete the Blueprint
{: #delete-schematics-blueprint-cli}
{: step}

Optional: Run the [`ibmcloud schematics blueprint delete`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-delete) command to remove the Blueprint created in earlier steps. 

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

By completing this tutorial, you've learned to deploy the cloud environment using {{site.data.keyword.bpshort}} Blueprint.

- Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customisation and usage scenarios for each sample. 

