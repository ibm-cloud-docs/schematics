---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-16"

keywords: blueprint update, update blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Updating a Blueprint
{: #update-blueprint}

Updating cloud resources with Blueprints is a two step process, update and install. The first step updates the Blueprint configuration in {{site.data.keyword.bpshort}} with the intended changes to the definition, IaC modules or inputs. The second [install](/docs/schematics?topic=schematics-install-blueprint&interface=cli) step executes the IaC automation modules to deploy the changes in the Blueprint configuration.  For more information, refer to, [Blueprints lifecycle](/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) commands.
{: shortdesc} 

Blueprint update leverages the capabilities of Terraform to perform updates to deployed cloud resources. The Terraform config and inputs to a Workspace are updated by the Blueprint update operation. From the updated configuration, Terraform determines the changes that must be performed against the existing deployed resources and performs the required resource updates, deletions or creates.  

Blueprints allows updating of:
- Blueprint definition
- Input files
- Dynamic inputs  

Run the [`ibmcloud schematics blueprint update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) command to refresh the Blueprint configuration stored by {{site.data.keyword.bpshort}} with updates to the Blueprint definition and module source repositories. When using the `git_release` option With `latest`, {{site.data.keyword.bpshort}} will identify the if any of the module Git repositories have been updated and perform a **pull latest** to update the Workspaces with the modified Terraform configs.

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

If explicit Blueprint versioning is used with release tags for each Blueprint definition release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new Blueprint release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --bp-git-release x.y.z  
```
{: pre}

Update the input file source and push a new release to its Git source repository. 

If explicit input file versioning is used with release tags for each input file release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --input-git-release x.y.z  
```
{: pre}

Where no Git release is specified and relaxed versioning (latest) is used for input files in the Blueprint config, no change to the Blueprint config is required and the input file changes will be pulled in automatically by Schematics. 

Record the Blueprint ID that needs to be updated. To list the Blueprint IDs, run [Get all the blueprint instances](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-list) command.
{: note}

## Updating a Blueprint from the UI 
{: #update-blueprint-ui}
{: ui}

Currently, you can only update Blueprint from command-line by using the [update command](#update-blueprint-cli) to update the Blueprint configuration and then run the [install](/docs/schematics?topic=schematics-install-blueprint) command to deploy the changes.

### Verify Blueprint update from the UI
{: #verify-bp-update-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the update operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` should show the summary details of the Blueprint update job. 
3. Click **Modules** tab to see the status of the resource modules. 
4. Click **Jobs history** tab view the result of the Blueprint update job and operations performed against the resource modules.  
5. Click **Settings** tab to view the summary of the updated Blueprint.

For more information, about how to diagnose and resolve issues if the command fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails&interface=cli).

## Updating a Blueprint from the CLI
{: #update-blueprint-cli}
{: cli}

Run the [`ibmcloud schematics blueprint update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) command to refresh the Blueprint configuration with the changes. This will update the Blueprint and Workspaces with the updated input values.
{: shortdesc} 

For all the Blueprints commands, syntax, and detailed option flags, refer to, [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

Syntax

```sh
ibmcloud schematics blueprint update --id BLUEPRINT_ID [--file CONFIG_FILE_PATH] [--input INPUT_VARIABLES_LIST] [--github-token GITHUB_TOKEN]
```
{: pre}

Example

```sh
ibmcloud schematics blueprint update -name Blueprint_Basic -resource-group default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=mynewrgdemo
```
{: pre}

On successful completion the update command will return **update_success**. For more information, about the command options, see [Update command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update).

### Verify Blueprint update
{: #verify-update}

Verify that the Blueprint has been updated successfully. When you update the Blueprint from the CLI, the command displays details of the linked Workspaces to be updated and a continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initalising the Workspaces. The command only returns on completion.

```text
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
```
{: screen}

On successful completion the update command will return **update_success**. 

For more information, about how to diagnose and resolve issues if the command fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails&interface=cli).



## Updating a Blueprint from the API
{: #update-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about Blueprint update, refer to, [Update a Blueprint](/apidocs/schematics/schematics#replace-blueprint) by using API.

Blueprint update API runs `Blueprint update`, and `Blueprint jobs` APIs together, to performs the update, and install Blueprint operations.
{: important}

Record the Blueprint ID that need to be deleted. To list the Blueprint ID, run [Get all the Blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

```json
PUT /v2/blueprints/us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer 
{
    "name": "Blueprint Basic Test",
    "schema_version": "1.0.0",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "main"
        }
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "main"
                }
            }
        }
    ],
    "inputs": [
        {
            "name": "provision_rg",
            "value": "true"
        },
        {
            "name": "resource_group_name",
            "value": "myrg4"
        }
    ],
    "description": "Deploys a simple two module blueprint-description update",
    "resource_group": "Default"
}
```
{: codeblock}

Output:

```text
{
    "name": "Blueprint Basic Example",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml"
        },
        "catalog": {},
        "cos_bucket": {}
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "inputs": [
                {
                    "name": "resource_group_name",
                    "value": "myrg4"
                },
                {
                    "name": "provision_rg",
                    "value": "true"
                },
                {
                    "name": "cos_instance_name",
                    "value": "Blueprint-basic"
                }
            ]
        }
    ],
    "description": "Simple two module blueprint. Deploys Resource Group and COS bucket",
    "resource_group": "47ecbb1f38ea4b8aa0a091edb1e4e909",
    "location": "us-south",
    "inputs": [
        {
            "name": "resource_group_name",
            "metadata": {}
        },
        {
            "name": "provision_rg",
            "metadata": {}
        },
        {
            "name": "cos_instance_name",
            "metadata": {}
        }
    ],
    "settings": [
        {
            "name": "TF_VERSION",
            "value": "1.0",
            "metadata": {}
        }
    ],
    "outputs": [
        {
            "name": "cos_id",
            "value": "$module.basic-cos-storage.outputs.cos_id",
            "metadata": {}
        }
    ],
    "modules": [
        {
            "name": "basic-resource-group",
            "layer": "RG",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            ..........
    "flow": {},
    "blueprint_id": "us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d",
    "crn": "crn:v1:bluemix:public:schematics:us-south:a/16a85b7b99a6622e7c186fb6503781a0:17e412e5-dfac-486a-804c-907d21a4454b:blueprint:us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d",
    "account": "16a85b7b99a6622e7c186fb6503781a0",
    "created_at": "2022-07-01T08:21:30.145345818Z",
    "created_by": "kgurudut@in.ibm.com",
    "updated_at": "2022-07-01T08:55:14.077179726Z",
    "updated_by": "kgurudut@in.ibm.com",
    "sys_lock": {
        "sys_locked_at": "0001-01-01T00:00:00Z"
    },
    "user_state": {
        "state": "Environment_Create_Init",
        "set_at": "0001-01-01T00:00:00Z"
    },
    "state": {}
}
```
{: screen}

For more information, about how to diagnose and resolve issues if the command fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails&interface=cli).

## Next steps
{: #bp-update-nextsteps}

After updating the Blueprint configuration in {{site.data.keyword.bpshort}}, the next step in deploying the cloud resources defined by the Blueprint is to [install](/docs/schematics?topic=schematics-install-blueprint) the changes to the Blueprint. 

The configuration of the Blueprint and outputs can be reviewed using the `blueprint get` command. See section [Displaying Blueprints](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-get). 

