---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: blueprint update, update blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Update a blueprint configuration
{: #update-blueprint}

Cloud environments are not static. User infrastructure requirements change and the {{site.data.keyword.cloud}} platform is constantly evolving. Without maintenance and updates of the blueprint templates, inputs and modules, a deployed environment loses currency, compliance, and will cease to be manageable through {{site.data.keyword.bpshort}} automation.  
{: shortdesc}

After the [deploy](/docs/schematics?topic=schematics-deploy-blueprints) lifecycle stage of a cloud environment, an environment will continue to evolve through managed change that is implemented as updates to the blueprint template, automation modules and inputs. Review the section on [updating blueprints](/docs/schematics?topic=schematics-update-op-blueprints) to understand more about the process of maintaining blueprint environments and the steps required to run regular updates. 

## Update process
{: #update-blueprint-process} 

Updating a deployed blueprint environment and cloud resources is a two-step process, Update, and Apply. The first step updates the blueprint configuration in {{site.data.keyword.bpshort}} with the intended changes to the blueprint template, IaC module code, and inputs. The second [blueprint apply](/docs/schematics?topic=schematics-apply-blueprint&interface=cli) step runs the automation module code to deploy the changes in the blueprint configuration. 
{: shortdesc} 

Updating a blueprint environment uses the capabilities of Terraform to update deployed cloud resources. The Terraform config and inputs for a module are updated by the `blueprint update` operation. From the updated configuration, Terraform determines the changes that must be run against the existing deployed resources and runs the needed resource updates, deletions, or creates. 

Update supports modification of:
- Blueprint template YAML file 
- Blueprint input file
- Dynamic (override) inputs  

Record the blueprint ID that needs to be updated. To list the blueprint IDs, run [listing blueprints](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-list) command.
{: note}

## Updating a blueprint through CLI
{: #update-blueprint-cli}
{: cli}

Run the [`ibmcloud schematics blueprint update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) command to refresh the blueprint configuration with the changes. It updates the blueprint and workspaces with the updated input values.
{: shortdesc} 

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
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

On successful completion the config update returns `update_success`. For more information, see [Update command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-update) and refer the specified example.

```sh
ibmcloud schematics blueprint update --id Blueprint_Basic.eaB.08d1 --inputs resource_group_name=basic-rg-demo-pre
```
{: pre}

### Verifying blueprint update
{: #verify-update}

Verify that the blueprint config is updated successfully. When you update the config using the CLI, the command displays the details of the linked workspaces to be updated. And continuously updates the status of the progress of the {{site.data.keyword.bpshort}} jobs initializes the workspaces. The command returns on completion.

```text
Update blueprint  Blueprint_Basic

Modules to be updated
SNO   Module Type   Name                   Updates   
1     Workspace     basic-resource-group   NA   
2     Workspace     basic-cos-storage      NA   
```
{: screen}

On successful completion the config update returns **`update_success`**. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Updating a blueprint environment using the UI 
{: #update-blueprint-ui}
{: ui}

You can follow these steps to update the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click the blueprint name that you want to edit.
2. Click **Variables** tab to view your **Inputs** tab.
3. Click **Edit** option in **Inputs Source**:
    - The source of the input variables can be changed to a different input file, version or branch
    - **Input file GIT URL**  - `<New or edited input file Git URL>`.
    - **Personal access token (private repositories only)** - `<Provide your Git personal access token, only for private Git repos>`.
    - Click **Update**.
4. Click **Settings** tab.
    - In **Details** section, click **Edit**.
       - **Name** - Edit the blueprint name.
       - **Description** - Edit the description.
       - Click **Update**.
    - In **Blueprint Source** section, click **Edit**.
       - The source of the template YAML file to refer to a different version or Git branch containing a revised version of the template. 
       - **Repository URL** - `<Edit the GIT URL>`. Refer to the [blueprint FAQs](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-url) for more information on the URL format. 
       - **Personal access token (private repositories only)** - `<Provide your Git personal access token, only for private Git repos>`.
    - Click **Update**.
5. Click **Generate Plan** to confirm the changes to the blueprint config.

### Verify blueprint update using the UI
{: #verify-bp-update-ui}

You can follow these steps to list the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external}. Click your blueprint name to view the blueprint details.
2. Click **Overview** to view the blueprint summary, that includes `Modules status`, `Variables summary`, `Blueprint Details` and `Recent Job runs` of your blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of modules and their current status. 
    - Optional: Click **Show details** to view the module details.
    - Optional: Click **Name** that takes to the modules `Workspace` page. 
4. Click **Resources** tab to view the list of resources provisioned status by the blueprint.
5. Click **Variables** tab to view your **Inputs** and **Outputs** variables and values. Optional: you can edit the input variable and click **Save variables**.
6. Click **Jobs history** tab view the job logs of the blueprint and module activities.
7. Click **Settings** tab to view the configuration settings of the blueprint.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Updating a blueprint using the API
{: #update-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Update a blueprint](/apidocs/schematics/schematics#replace-blueprint) by using API.

Blueprint update API runs `blueprint update` `API`, to perform the changes in configuration by using blueprint operations.
{: important}

To list the blueprint ID, run [get all the blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

```json
PUT /v2/blueprints/Blueprint-Basic-Test-API.soB.347a/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
refresh_token: <refresh_token>

{
    "name": "Blueprint Basic Test API",
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
    "description": "Deploys a simple two module blueprint successfully Updated",
    "resource_group": "Default"
}
```
{: codeblock}

### Verify blueprint create using the API
{: #verify-bp-update-api}

Verify that the blueprint update is success as shown in the output.
{: shortdesc}

Output

```text
{
    "name": "Blueprint Basic Test API",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "main",
            "git_commit": "68ce0e62f2e1b33c2341fc35fb125ffe998128d6",
            "git_commit_timestamp": "2022-11-15 11:08:48 +0000 UTC"
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
                    "git_branch": "main",
                    "git_commit": "68ce0e62f2e1b33c2341fc35fb125ffe998128d6",
                    "git_commit_timestamp": "2022-11-15 11:08:48 +0000 UTC"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "inputs": [
                {
                    "name": "cos_instance_name",
                    "value": "mycos4"
                },
                {
                    "name": "cos_storage_plan",
                    "value": "standard"
                }
            ]
        }
    ],
    "description": "Deploys a simple two module blueprint successfully Updated",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "location": "us-south",
    "inputs": [
        {
            "name": "cos_instance_name",
            "value": "mycos4",
            "metadata": {
                "source": "userinput"
            }
        },
        {
            "name": "cos_storage_plan",
            "value": "standard",
            "metadata": {
                "source": "userinput"
            }
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
            "module_id": "us-south.workspace.basic-resource-group.483b1ea2",
            "module_type": "terraform",
            "name": "basic-resource-group",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-DefaultResourceGroup",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "outputs": [
                {
                    "name": "resource_group_id",
                    "metadata": {}
                },
                {
                    "name": "resource_group_name",
                    "metadata": {}
                }
            ],
            "last_job": {
                "command_object_id": "us-south.workspace.basic-resource-group.483b1ea2",
                "command_name": "workspace_apply",
                "job_status": "job_finished"
            }
        },
        {
            "module_id": "us-south.workspace.basic-cos-storage.535f45d6",
            "module_type": "terraform",
            "name": "basic-cos-storage",
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
                    "value": "$blueprint.cos_instance_name",
                    "metadata": {}
                },
                {
                    "name": "cos_single_site_loc",
                    "value": "ams03",
                    "metadata": {}
                },
                {
                    "name": "cos_storage_plan",
                    "value": "$blueprint.cos_storage_plan",
                    "metadata": {}
                },
                {
                    "name": "resource_group_id",
                    "value": "$module.basic-resource-group.outputs.resource_group_id",
                    "metadata": {}
                }
            ],
            "outputs": [
                {
                    "name": "cos_crn",
                    "metadata": {}
                },
                {
                    "name": "cos_id",
                    "metadata": {}
                }
            ],
            "last_job": {
                "command_object_id": "us-south.workspace.basic-cos-storage.535f45d6",
                "command_name": "workspace_apply",
                "job_status": "job_finished"
            }
        }
    ],
    "flow": {},
    "blueprint_id": "Blueprint-Basic-Test-API.soB.347a",
    "crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:blueprint:Blueprint-Basic-Test-API.soB.347a",
    "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
    "created_at": "2022-11-23T14:32:45.897540037Z",
    "created_by": "test@in.ibm.com",
    "updated_at": "2022-11-23T14:43:01.561699411Z",
    "updated_by": "test@in.ibm.com",
    "sys_lock": {
        "sys_locked_at": "0001-01-01T00:00:00Z"
    },
    "user_state": {
        "set_at": "0001-01-01T00:00:00Z"
    },
    "state": {}
}
```
{: screen}

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Next steps
{: #bp-update-nextsteps}

After updating the blueprint configuration in {{site.data.keyword.bpshort}}, the next step is to [apply](/docs/schematics?topic=schematics-apply-blueprint) the changes.


