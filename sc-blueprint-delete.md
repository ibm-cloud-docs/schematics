---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: blueprint config delete, delete blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Delete a blueprint configuration
{: #delete-blueprint}

When a blueprint  environment is no longer required, it can be deleted which will terminate billing for all deployed resources. See the [deleting blueprints](/docs/schematics?topic=schematics-delete-blueprint) lifecycle stage to understand the process of deleting blueprint environments and the steps. Deleting an environment is a two-stage process that first destroys all the associated cloud resources (environment) and second deletes the blueprint config in {{site.data.keyword.bpshort}}.
{: shortdesc}

Deleting the blueprint configuration is the second step required to completely delete a blueprint from {{site.data.keyword.bpshort}}. To protect from accidental deletion, the config can only be deleted when cloud resources in all the blueprint modules have been deleted and the modules are in `Inactive` state. The first step is to run the [blueprint run destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=ui) command to destroy the resources in the blueprint environment and remove the environment. 

This behavior of disallowing delete when modules cannot be returned to an `Inactive` state due to a {{site.data.keyword.bpshort}} or Terraform error can be overridden using the `-force-delete` flag to allow deletion.  
{: shortdesc}

## Deleting a blueprint config from the CLI
{: #delete-blueprint-cli}
{: cli}

For more information, see [blueprint config delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-delete) command. The `blueprint run destroy` command must have been run first to destroy the resources, only then can the `blueprint config delete` command run. 

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint config delete --id Blueprint_Basic.eaB.5cd9
```
{: pre}

Output

```text
SNO Type Name Status
1 terraform basic-resource-group INACTIVE
2 terraform basic-cos-storage INACTIVE

Do you really want to delete the Blueprint ? [y/N]> y
Job : us-east.JOB.Blueprint_Basic.cbe0591e Created
Job Type: BLUEPRINT DELETE
OK
```
{: screen}

### Verifying blueprint config deletion 
{: #verify-bp-delete-cli}

During the Beta, the config delete CLI command does not wait for successful job completion and returns immediately. 

The status of the config delete operation can be monitored by using the `blueprint job get` command. The following command runs a `blueprint job get` for the JOB ID `eu-gb.JOB.Blueprint-Basic-Example.f2d388d3`. The job ID is displayed in the config delete output. 

```sh
ibmcloud schematics blueprint job get --id us-east.JOB.Blueprint_Basic.e4081308
```
{: pre}


```text
ID                      us-east.JOB.Blueprint_Basic.e4081308   
Blueprint ID            Blueprint_Basic.eaB.5cd9   
Job Type                blueprint_delete   
Location                us-east
Start Time              2022-08-03 15:51:12
End Time                2022-08-03 15:54:11    
                           
OK
```
{: screen}

During the delete operation that the status shows `In Progress`, when completed the status changes to `Normal`. The blueprint configuration and all the dependent cloud resources are now deleted. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Deleting a blueprint config from the UI 
{: #delete-blueprint-ui}
{: ui}

You can delete a blueprint from the command line by using the [config delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete).

### Verifying blueprint config deletion 
{: #verify-bp-deletion-ui}

After deletion, the blueprint is not displayed in the UI. 

## Deleting blueprint from the API
{: #delete-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Delete a blueprint config](/apidocs/schematics/schematics#delete-blueprint) by using API. 

You need to run `blueprint run destroy` command and then run `blueprint config delete` command. For more information, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprint) configuration.

Record the blueprint ID that needs to be deleted. To list the blueprint ID, run [get all the blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

**Example:**

```json
POST /v2/jobs/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
refresh_token: <refresh_token>
{
    "command_name": "blueprint_delete",
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test.eaB.bbb9"
}

```
{: codeblock}

### Verifying blueprint delete from the API
{: #verify-bp-delete-api}

Verify that the blueprint is deleted successfully as shown in the output.
{: shortdesc}

**Output:**

```text
{
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test.eaB.bbb9",
    "command_name": "blueprint_delete",
    "id": "us-east.JOB.Blueprint-Basic-Test.693bdc57",
    "name": "JOB.Blueprint-Basic-Test.blueprint_delete.1663585554252",
    "description": "Deploys a simple two module blueprint",
    "location": "us-east",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "submitted_at": "2022-09-19T11:05:54.251828146Z",
    "submitted_by": "test@in.ibm.com",
    "start_at": "2022-09-19T11:05:54.251825549Z",
    "end_at": "0001-01-01T00:00:00Z",
    "status": {
        "workspace_job_status": {
            "flow_status": {
                "updated_at": "0001-01-01T00:00:00Z"
            },
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "action_job_status": {
            "action_name": "Blueprint Basic Test",
            "status_code": "job_pending",
            "status_message": "Job created and pending to start",
            "updated_at": "2022-09-19T11:05:54.251830598Z"
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
                    "command_object_id": "us-east.workspace.basic-resource-group.a99dc1a0",
                    "command_object_name": "basic-resource-group",
                    "source": {
                        "source_type": "git_hub",
                        "git": {
                            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup",
                            "git_branch": "main"
                        },
                        "catalog": {},
                        "cos_bucket": {}
                    },
                    "inputs": [
                        {
                            "name": "provision",
                            "value": "$blueprint.provision_rg",
                            "metadata": {}
                        },
                        {
                            "name": "name",
                            "value": "$blueprint.resource_group_name",
                            "metadata": {}
                        }
                    ],
                    "outputs": [
                        {
                            "name": "resource_group_name",
                            "metadata": {}
                        },
                        {
                            "name": "resource_group_id",
                            "metadata": {}
                        }
                    ],
                    "last_job": {
                        "command_object_id": "us-east.workspace.basic-resource-group.a99dc1a0",
                        "command_name": "workspace_destroy",
                        "job_status": "job_finished"
                    },
                    "updated_at": "0001-01-01T00:00:00Z",
                    "updated": "false"
                },
                {
                    "command_object_id": "us-east.workspace.basic-cos-storage.61cb03be",
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
                            "name": "cos_storage_plan",
                            "value": "$blueprint.cos_storage_plan",
                            "metadata": {}
                        },
                        {
                            "name": "cos_single_site_loc",
                            "value": "ams03",
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
                            "name": "cos_id",
                            "metadata": {}
                        },
                        {
                            "name": "cos_crn",
                            "metadata": {}
                        }
                    ],
                    "last_job": {
                        "command_object_id": "us-east.workspace.basic-cos-storage.61cb03be",
                        "command_name": "workspace_destroy",
                        "job_status": "job_finished"
                    },
                    "updated_at": "0001-01-01T00:00:00Z",
                    "updated": "false"
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
{: #bp-delete-nextsteps}

- Looking for additional blueprint samples to work with? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the `Readme` files of the examples for further customization and usage for each sample. 
