---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-22"

keywords: blueprint update, update blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Updating a Blueprint
{: #update-blueprint}

Cloud environments are not static. User infrastructure requirements change and the {{site.data.keyword.cloud}} platform are constantly evolving. Without maintenance and updates of the Blueprint IaC definitions, inputs and modules, a deployed environment loses currency, compliance, and cease to be manageable through {{site.data.keyword.bpshort}} IaC automation.   
{: shortdesc}

After the [deployment](/docs/schematics?topic=sc-bp-deploy) lifecycle stage of a cloud environment, the environment will continue to evolve through managed change that is implemented as updates to the Blueprint definition, IaC modules, and inputs. See [Updating deployed environments](/docs/schematics?topic=schematics-operate-blueprints#operate-multistep) to understand more about the process of updating Blueprint environments and the steps needed to set regular updates. 

## Update process
{: #update-blueprint-process} 

Updating a deployed Blueprint environment and cloud resources is a two-step process, Update, and Apply. The first step updates the Blueprint configuration in {{site.data.keyword.bpshort}} with the intended changes to the Blueprint definition, IaC module code, and inputs. The second [Apply](/docs/schematics?topic=schematics-apply-blueprint&interface=cli) step runs the automation module code to deploy the changes in the Blueprint configuration.  
{: shortdesc} 

Updating a Blueprint environment uses the capabilities of Terraform to runs updates to deployed cloud resources. The Terraform config and inputs to a Workspace are updated by the Blueprint Update operation. From the updated configuration, Terraform determines the changes that must be set against the existing deployed resources and sets the needed resource updates, deletions, or creates.  

Update supports modification of:
- Blueprint definition YAML file 
- Input values file
- Extra inputs  

Run the [`ibmcloud schematics blueprint update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) command to refresh the Blueprint configuration that is stored by {{site.data.keyword.bpshort}} with updates to the Blueprint definition and IaC code in the module source repositories. 

## Explicit and relaxed versioning
{: #update-blueprint-versioning}

Blueprints support two update approaches for changes to definitions and modules. Either a relaxed 'development' mode where the current version of a definition or module is always pulled, or a 'production' mode where an explicit version is specified. 

### Relaxed versioning
{: #update-blueprint-relaxed} 

In some development environments, or where IaC code and Blueprint definitions are being developed, versioning of IaC definitions that are not needed.  

Blueprints support a default option to always pull the current definition, inputs, or modules on an update option. Here definition and module statements can use the `git_release` option `latest`. When specified the {{site.data.keyword.bpshort}} identifies if the Blueprint definition, or module Git repositories are updated, and sets a `pull latest` to update the Blueprint configuration. Any Workspaces with modified Terraform module configurations.

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

### Explicit versioning
{: #update-blueprint-strict} 

If explicit Blueprint version is used with Git release tags for each Blueprint definition release, the Blueprint configuration is only updated if a new release tag is specified on the Update operation. 
 

```sh
ibmcloud schematics blueprint update --id basic-demo-pre.deB.1794 --inputs resource_group_name=basic-rg-demo-pre 
```
{: pre}

Update the input file source and push a new release to its Git source repository. 

If explicit input file version is used with release tags for each input file release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --input-git-release x.y.z  
```
{: pre}

Where no Git release is specified, and relaxed version (current) is used for input files in the Blueprint config. You cannot see the change to the Blueprint config that are needed, and the input file changes are pulled in automatically by {{site.data.keyword.bpshort}}. 

Record the Blueprint ID that needs to be updated. To list the Blueprint IDs, run [get all the blueprint instances](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-list) command.
{: note}

## Updating a Blueprint from the CLI
{: #update-blueprint-cli}
{: cli}

Run the [`ibmcloud schematics blueprint update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) command to refresh the Blueprint configuration with the changes. It updates the Blueprint and Workspaces with the updated input values.
{: shortdesc} 

For all the Blueprints commands, syntax, and option flag details, see [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
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

On successful completion the update command returns `update_success`. For more information, see [Update command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) and refer the specified example.

```sh
ibmcloud schematics blueprint update --id Blueprint_Basic.eaB.5cd9 --inputs resource_group_name=basic-rg-demo-pre
```
{: pre}

### Verify Blueprint update
{: #verify-update}

Verify that the Blueprint is updated successfully. When you update the Blueprint from the CLI, the command displays the details of the linked Workspaces to be updated. And continuously updates the status of the progress of the {{site.data.keyword.bpshort}} jobs initializes the Workspaces. The command returns on completion.

```text
Update Blueprint  blueprint

Modules to be updated
SNO   Module Type   Name                   Updates   
1     Workspace     basic-resource-group   Updating   
2     Workspace     basic-cos-storage      NA   
      
Blueprint job running us-east.JOB.Blueprint_Basic.e4081308

Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:0    Active:0    Failed:0   

Module Type   Name                   Status           Job ID   
Blueprint     blueprint              UPDATE_SUCCESS   us-east.JOB.Blueprint_Basic.e4081308   
Workspace     basic-resource-group   INACTIVE            
Workspace     basic-cos-storage      INACTIVE            
              
Blueprint ID Blueprint_Basic.eaB.5cd9 update_success at 2022-08-15 13:19:27
OK
```
{: screen}

On successful completion the update command returns **`update_success`**. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails&interface=cli).


## Updating a Blueprint from the UI 
{: #update-blueprint-ui}
{: ui}

Currently, you can update Blueprint from CLI by using the [update command](#update-blueprint-cli) to update the Blueprint configuration and then run the [Apply](/docs/schematics?topic=schematics-apply-blueprint) command to deploy the changes.

### Verify Blueprint update from the UI
{: #verify-bp-update-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the update operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` must show the summary details of the Blueprint update job. 
3. Click **Modules** tab to see the status of the resource modules. 
4. Click **Jobs history** tab view the result of the Blueprint update job and operations that are set against the resource modules.  
5. Click **Settings** tab to view the summary of the updated Blueprint.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails&interface=cli).

## Updating a Blueprint from the API
{: #update-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Update a Blueprint](/apidocs/schematics/schematics#replace-blueprint) by using API.

Blueprint update API runs `Blueprint update` `API`, to perform the changes in configuration by using Blueprint operations.
{: important}

To list the Blueprint ID, run [get all the Blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

```json
PUT /v2/blueprints/Blueprint-Basic-Test.eaB.bbb9/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
refresh_token: <refresh_token>

{
    "name": "Blueprint Basic Test",
    "schema_version": "1.0.0",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "master"
        }
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "master"
                }
            }
        }
    ],
    "inputs": [
        {
            "name": "provision_rg",
            "value": "false"
        },
        {
            "name": "resource_group_name",
            "value": "myrg4"
        },
        {
            "name": "cos_instance_name",
            "value": "myrg4"
        }
    ],
    "description": "Deploys a simple two module blueprint Updated",
    "resource_group": "Default"
}

```
{: codeblock}

### Verify Blueprint create from the API
{: #verify-bp-update-api}


**`Output:`**

```text
{
    "name": "Blueprint Basic Test",
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
                    "git_branch": "master"
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
                    "value": "myrg4"
                },
                {
                    "name": "cos_storage_plan",
                    "value": "standard"
                }
            ]
        }
    ],
    "description": "Deploys a simple two module blueprint Updated",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
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
        },
        {
            "name": "cos_storage_plan",
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
            "module_type": "terraform",
            "name": "basic-resource-group",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "inputs": [
                {
                    "name": "name",
                    "value": "$blueprint.resource_group_name"
                },
                {
                    "name": "provision",
                    "value": "$blueprint.provision_rg"
                }
            ],
            "outputs": [
                {
                    "name": "resource_group_id"
                },
                {
                    "name": "resource_group_name"
                }
            ],
            "last_job": {
                "job_status": "job_failed"
            },
            "deleted": "false"
        },
        {
            "module_type": "terraform",
            "name": "basic-cos-storage",
            "layer": "DB",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-Storage",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "inputs": [
                {
                    "name": "cos_instance_name",
                    "value": "$blueprint.cos_instance_name"
                },
                {
                    "name": "cos_single_site_loc",
                    "value": "ams03"
                },
                {
                    "name": "cos_storage_plan",
                    "value": "$blueprint.cos_storage_plan"
                },
                {
                    "name": "resource_group_id",
                    "value": "$module.basic-resource-group.outputs.resource_group_id"
                }
            ],
            "outputs": [
                {
                    "name": "cos_crn"
                },
                {
                    "name": "cos_id"
                }
            ],
            "last_job": {
                "job_status": "job_failed"
            },
            "deleted": "false"
        }
    ],
    "flow": {},
    "blueprint_id": "Blueprint-Basic-Test.eaB.bbb9",
    "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
    "created_at": "2022-09-15T07:04:09.725355429Z",
    "created_by": "smulampa@in.ibm.com",
    "updated_at": "2022-09-15T10:13:05.108885567Z",
    "updated_by": "smulampa@in.ibm.com",
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

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails&interface=cli).

## Next steps
{: #bp-update-nextsteps}

After updating the Blueprint configuration in {{site.data.keyword.bpshort}}, the next step is destroying the Blueprint [destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=api) resource.

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 