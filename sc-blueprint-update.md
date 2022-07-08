---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-08"

keywords: blueprint update, update blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Updating a Blueprint
{: #update-blueprint}

Updating cloud resources with Blueprints is a two step process, update and install. The first step updates the Blueprint configuration in {{site.data.keyword.bpshort}} with the intended changes to the definition, IaC module or inputs. The second [install](/docs/schematics?topic=schematics-sc-blueprint-install) step executes the IaC automation modules to deploy the changes in the Blueprint configuration.  Refer to [Blueprints lifecycle](https://test.cloud.ibm.com/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands update, update and delete and the Blueprints lifecycle. 
{: shortdesc} 

Blueprint update leverages the capabilities of Terraform to perform updates to deployed cloud resources. The Terraform config and inputs to a workspace are updated and Terraform performs the execution.

After initial resource deployment on Blueprint create, and install follows the same approach as

Blueprints allows updating of:
- Blueprint definition
- Input files
- Dynamic inputs






  

Run the `ibmcloud schematics blueprint update` command to refresh the Blueprint configuration stored by {{site.data.keyword.bpshort}} with the update to the Blueprint definition. With 'latest' release, {{site.data.keyword.bpshort}} will identify the updated module Git repos and perform a Pull-Latest to update any Workspaces with the modified Terraform configs.  

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

If explicit Blueprint versioning is used with release tags for each Blueprint definition release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new Blueprint release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --bp-git-release x.y.z  
```
{: pre}



Update the input file source and push a new release to its Git source repository . 


If explicit input file versioning is used with release tags for each input file release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --input-git-release x.y.z  
```
{: pre}

Where no Git release is specified and relaxed module versioning (latest) is used for input files in the Blueprint config, no change to the Blueprint config is required and the input file changes will be pulled in automatically by Schematics.  

Run the `ibmcloud schematics blueprint update` command to refresh the Blueprint configuration with the changes. This will update the Blueprint and Workspaces with the updated input values. 

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}





**Syntax:**

```sh
ibmcloud schematics blueprint update -name Blueprint_Basic -bp-git-branch main -input-git-url -input-git-branch main  -inputs provision_rg=true,resource_group_name=test_rg
```
{: pre}

On successful completion the update command will return **update_success**. 

For more information, about the command options, see [Update command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update).



## Verfiy Blueprint update

Verify that the Blueprint has been updated successfully. When you update the Blueprint from the CLI, the command displays details of the linked Workspaces to be updated and a continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initalising the Workspaces. The command only returns on completion.

Update Blueprint ID: eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
Modules to be updated
SNO   Type        Name   
1     Workspace   basic-resource-group   
2     Workspace   basic-cos-storage   
      
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.da1b13ca
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   

Type        Name                      Status           Job ID   
Blueprint   Blueprint Basic Example   updatE_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.da1b13ca   
Workspace   basic-resource-group      INACTIVE            
Workspace   basic-cos-storage         INACTIVE            
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 update_success at Mon Jun 27 16:18:47 BST 2022
OK
{: screen}

On successful completion the update command will return update_success. 

After updating the Blueprint in {{site.data.keyword.bpshort}}, the next step in deploying the cloud resources defined by the Blueprint is to [install](/docs/schematics?topic=schematics-install-blueprint) the Blueprint. 
