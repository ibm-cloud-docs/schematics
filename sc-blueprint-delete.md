---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-22"

keywords: blueprint delete, delete blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Deleting a Blueprint
{: #delete-blueprint}

When a Blueprint environment is no longer needed, it enters the delete lifecycle stage. See [Deleting Blueprint environments](/docs/schematics?topic=schematics-delete-blueprints) to understand the process of deleting Blueprint environments and the steps. Deleting a Blueprint environment is a two-stage process of first destroys all the associated cloud resources and then deleting the Blueprint in {{site.data.keyword.bpshort}}.
{: shortdesc}


Blueprint delete is the second step that is needed to completely delete a Blueprint from {{site.data.keyword.bpshort}}. To protect from accidental deletion, a Blueprint can be deleted when cloud resources in all the linked Workspaces are deleted and the Workspaces are in `Inactive` state. The first step is to run the [destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=ui) command to destroy the resources that are used in the Workspaces.

This behavior can be modified by using the `-force-delete` flag to allow deletion when Workspaces cannot be returned to an `Inactive` state.
{: shortdesc}

## Deleting Blueprint from the CLI
{: #delete-blueprint-cli}
{: cli}

For more information, see [delete command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-delete). You need to run Blueprint destroy command and then run Blueprint delete command. For more information, see [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).

For all the Blueprints commands, syntax, and option flag details, see [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint delete --id Blueprint_Basic.eaB.5cd9
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

### Verify Blueprint delete 
{: #verify-bp-delete-cli}

During the Beta, the delete CLI command does not wait for successful job completion and returns immediately. 

The status of the delete operation can be monitored by using the `blueprint job get` command. The following command sets a Blueprint `job get` for the JOB ID `eu-gb.JOB.Blueprint-Basic-Example.f2d388d3`. The job ID is displayed in the delete command output. 

```sh
mcloud schematics blueprint job get --id us-east.JOB.Blueprint_Basic.e4081308
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

During the delete operation that the status shows `In Progress`, when completed the status changes to `Normal`. The Blueprint and all the depended cloud resources are now deleted. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails&interface=cli).

## Deleting a Blueprint from the UI 
{: #delete-blueprint-ui}
{: ui}

You can delete a Blueprint from command line by using the [delete command](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete).

### Verify Blueprint deletion 
{: #verify-bp-deletion-ui}

After deletion, the Blueprint is not displayed in the UI. 

## Deleting a Blueprint from the API
{: #delete-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Delete a Blueprint](/apidocs/schematics/schematics#delete-blueprint) by using API. 

You need to run Blueprint destroy command and then run Blueprint delete command. For more information, see [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).


Record the Blueprint ID that needs to be deleted. To list the Blueprint ID, run [get all the blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

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

Output

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
    "submitted_by": "smulampa@in.ibm.com",
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

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails&interface=cli).

## Next steps
{: #bp-delete-nextsteps}

To destroy or delete a Blueprint, see [destroy a Blueprint](/docs/schematics?topic=schematics-destroy-blueprint&interface=cli), and [delete a Blueprint](/docs/schematics?topic=schematics-delete-blueprint&interface=cli#delete-blueprint-cli).

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
