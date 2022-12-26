---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: blueprint destroy, destroy blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Destroy a blueprint environment 
{: #destroy-blueprint}

When a blueprint environment is no longer required, it can be deleted which will terminate billing for all deployed resources. Deleting a blueprint environment is a two-stage process that first destroys all the associated cloud resources and second deletes the blueprint config in {{site.data.keyword.bpshort}}. See [Deleting blueprints](/docs/schematics?topic=schematics-delete-blueprint) to understand the process of deleting blueprint environments. 
{: shortdesc}

The cloud resources that are created by a blueprint are destroyed by using the `blueprint destroy` command. The saved blueprint config can then be removed from {{site.data.keyword.bpshort}}. The delete can be performed after all resources are destroyed by using the [blueprint destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=cli) command. 

For Terraform based modules, the destroy operation runs a Terraform Destroy command for each module. This removes all cloud resources in reverse dependency order. 

## Destroying a blueprint environment using the UI 
{: #destroy-blueprint-ui}
{: ui}

You can follow these steps to destroy the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click the name of the blueprint that you want to destroy.
2. Select the **Actions** drop down list.
3. Select **Destroy resources** to delete the resources associated with this blueprint.
4. Type the `<name>` in **Destroy resources** text box.
5. Click **Destroy** button.

### Verifying blueprint environment destroy 
{: #verify-bp-destroy-ui}

1. Click your blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the destroy operation. 
2. Click **Overview** tab to see the blueprint summary, including `Modules`, `Variables`, `Details`, `Recent Job runs` of your environment. 
3. Click **Modules** tab to see the list of resource modules in an `Inactive` state.
4. Click **Jobs history** tab view the result of the destroy job and operations that were run by the automation modules. 

## Destroying a blueprint environment using the CLI
{: #destroy-blueprint-cli}
{: cli}

The following command runs a `blueprint destroy` for the environment with the ID `Blueprint_Basic.eaB.08d1`

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint destroy --id Blueprint_Basic.eaB.08d1
```
{: pre}

On successful completion, the destroy command returns **`fullfilment_success`**. 

For more information, review the [blueprint destroy](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-destroy) command.

### Verifying blueprint destroy success 
{: #verify-bp-destroy-cli}

Verify that the blueprint environment resources are destroyed successfully. When you destroy using the CLI, the command displays details of the modules containing the resources to be destroyed, and the status of {{site.data.keyword.bpshort}} jobs that run the Terraform destroy operations. Confirm that the user intends to destroy all resources. The command returns on completion.

```text
Modules to be destroyed
SNO   Module Type   Name                   Status   
1     Workspace     basic-resource-group   APPLIED   
2     Workspace     basic-cos-storage      APPLIED   
      
Do you really want to destroy all the resources of blueprint Blueprint_Basic.eaB.08d1? [y/N]> y
Blueprint job us-east.JOB.Blueprint_Basic.d5486e24 started at 2022-11-18 16:44:52

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint_Basic.d5486e24 completed at 2022-11-18 16:48:22

Module Type   Name                   Status               Job ID   
Blueprint     Blueprint_Basic        job_finished   us-east.JOB.Blueprint_Basic.d5486e24   
Workspace     basic-resource-group   job_finished             
Workspace     basic-cos-storage      job_finished            
              
Blueprint ID Blueprint_Basic.eaB.08d1 job_finished at 2022-11-18 16:48:23
OK
```
{: screen}

On successful completion, destroy command returns `job_finished`. Successful command completion indicates that resources in all modules are destroyed.

## Destroying blueprint environment using the API
{: #destroy-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [destroy a blueprint environment](/apidocs/schematics/schematics#delete-blueprint) by using API.

Record the blueprint ID that is destroyed. To list the blueprint IDs, run [get all the blueprints](/apidocs/schematics/schematics#list-blueprint) command.

### Verifying blueprint destroy using the API
{: #bp-verify-display-api}

Verify that the blueprint is destroyed successfully.
{: shortdesc}

Example

```json
POST /v2/jobs/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
refresh_token: <refresh_token>

{
    "command_name": "blueprint_destroy",
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test-API.soB.347a"
}
```
{: codeblock}

Output

```text
{
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test-API.soB.347a",
    "command_name": "blueprint_destroy",
    "id": "us-south.JOB.Blueprint-Basic-Test-API.5780d18b",
    "name": "JOB.Blueprint-Basic-Test-API.blueprint_destroy.1669215098005",
    "description": "Deploys a simple two module blueprint successfully Updated",
    "location": "us-south",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "submitted_at": "2022-11-23T14:51:38.004884038Z",
    "submitted_by": "test@in.ibm.com",
    "start_at": "2022-11-23T14:51:38.00487612Z",
    "end_at": "0001-01-01T00:00:00Z",
    "status": {
        "workspace_job_status": {
            "flow_status": {
                "updated_at": "0001-01-01T00:00:00Z"
            },
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "action_job_status": {
            "action_name": "Blueprint Basic Test API",
            "status_code": "job_pending",
            "status_message": "Job created and pending to start",
            "updated_at": "2022-11-23T14:51:38.004890987Z"
        },
        "system_job_status": {
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "flow_job_status": {
            "updated_at": "0001-01-01T00:00:00Z"
        }
    },
    "data": {
        "job_type": "",
        "workspace_job_data": {
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "action_job_data": {
            "updated_at": "0001-01-01T00:00:00Z",
            "inventory_record": {
                "created_at": "0001-01-01T00:00:00Z",
                "updated_at": "0001-01-01T00:00:00Z"
            }
        },
        "system_job_data": {
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "flow_job_data": {
            "workitems": [
                {
                    "command_object_id": "us-south.workspace.basic-resource-group.483b1ea2",
                    "command_object_name": "basic-resource-group",
                    "source": {
                        "source_type": "git_hub",
                        "git": {
                            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-DefaultResourceGroup",
                            "git_branch": "main"
                        },
                        "catalog": {},
                        "cos_bucket": {}
                    },
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
                    },
                    "updated_at": "0001-01-01T00:00:00Z"
                },
                {
                    "command_object_id": "us-south.workspace.basic-cos-storage.535f45d6",
                    "command_object_name": "basic-cos-storage",
                    "source": {
                        "source_type": "git_hub",
                        "git": {
                            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-Storage",
                            "git_branch": "main"
                        },
                        "catalog": {},
                        "cos_bucket": {}
                    },
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
                    },
                    "updated_at": "0001-01-01T00:00:00Z"
                }
            ],
            "updated_at": "0001-01-01T00:00:00Z"
        }
    },
    "bastion": {},
    "log_summary": {
        "log_start_at": "0001-01-01T00:00:00Z",
        "log_analyzed_till": "0001-01-01T00:00:00Z",
        "repo_download_job": {},
        "workspace_job": {},
        "flow_job": {},
        "action_job": {
            "recap": {}
        },
        "system_job": {}
    },
    "updated_at": "0001-01-01T00:00:00Z"
}
```
{: screen}

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Next steps
{: #bp-destroy-nextsteps}

After the cloud resources are destroyed, the blueprint can be [deleted](/docs/schematics?topic=schematics-delete-blueprint&interface=api) from {{site.data.keyword.bpshort}}. Alternatively ,the cloud environment can be re-constituted and the resources re-created by running [blueprint apply](/docs/schematics?topic=schematics-apply-blueprint) again using the same blueprint configuration.

