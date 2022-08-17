---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-17"

keywords: blueprint delete, delete blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Deleting a Blueprint
{: #delete-blueprint}

Blueprint delete is the second step required to completely delete a Blueprint from {{site.data.keyword.bpshort}}. To protect from accidental deletion, a Blueprint can only be deleted when cloud resources in all the linked Workspaces have been deleted and the Workspaces are in `Inactive` state.  

This behaviour can be modified by using the `-force-delete` flag to allow deletion when Workspaces cannot be returned to an Inactive state. 

## Deleting a Blueprint from the UI 
{: #delete-blueprint-ui}
{: ui}

You can only delete Blueprint from command-line by using the [delete command](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete).

### Verify Blueprint deletion 
{: #verify-bp-deletion-ui}

After deletion the Blueprint will not be displayed in the UI. 

## Deleting Blueprint from the CLI
{: #delete-blueprint-cli}
{: cli}

For more information, about the command options, see the [delete command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-delete). You need to run Blueprint destroy command and then run Blueprint delete command. For more information, about the difference between destroy and delete command, refer to, [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).

For all the Blueprints commands, syntax, and detailed option flags, refer to, [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
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

The status of the delete operation can be monitored using the `blueprint job get` command. The following command performs a Blueprint `job get` for the JOB ID `eu-gb.JOB.Blueprint-Basic-Example.f2d388d3`. The job ID will be displayed in the delete command output. 

```sh
mcloud schematics blueprint job get --id useast.JOB.Blueprint_Basic.e4081308
```
{: pre}


```text
ID                      useast.JOB.Blueprint_Basic.e4081308   
Blueprint ID            Blueprint_Basic.eaB.5cd9   
Job Type                blueprint_delete   
Location                us-east
Start Time              2022-08-03 15:51:12
End Time                2022-08-03 15:54:11    
                           
OK
```
{: screen}

During the delete operation the status will show `In Progress`, when completed the status will change to `Normal`. The Blueprint and all its cloud resources are now deleted. 

For more information, about how to diagnose and resolve issues if the command fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails&interface=cli).

## Deleting a Blueprint from the API
{: #delete-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about Blueprint delete, refer to, [Delete a Blueprint](/apidocs/schematics/schematics#delete-blueprint) by using API. 

You need to run Blueprint destroy command and then run Blueprint delete command. For more information, about the difference between destroy and delete command, refer to, [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).

Blueprint delete API runs `Blueprint delete`, and `Blueprint jobs` `APIs` together, to performs the delete, and install Blueprint operations.
{: important}

Record the Blueprint ID that needs to be deleted. To list the Blueprint ID, run [Get all the blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

```json
POST /v2/jobs/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer 
{
    "command_name": "env_delete",
    "command_object": "blueprint",
    "command_object_id": "us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d"
}

```
{: codeblock}

Output

```text
{
    "command_object": "blueprint",
    "command_object_id": "us-south.ENVIRONMENT.Blueprint-Basic-Example.b14a205d",
    "command_name": "env_delete",
    "id": "us-south.JOB.Blueprint-Basic-Example.da1e2204",
    "name": "JOB.Blueprint-Basic-Example.env_delete.1656667216764",
    "description": "Simple two module blueprint. Deploys Resource Group and COS bucket",
    "location": "us-south",
    "resource_group": "47ecbb1f38ea4b8aa0a091edb1e4e909",
    "submitted_at": "2022-07-01T09:20:16.763637028Z",
    "submitted_by": "kgurudut@in.ibm.com",
    "start_at": "2022-07-01T09:20:16.763633729Z",
    "end_at": "0001-01-01T00:00:00Z",
    "status": {
        "workspace_job_status": {
            "flow_status": {
                "updated_at": "0001-01-01T00:00:00Z"
            },
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "action_job_status": {
            "action_name": "Blueprint Basic Example",
            "status_code": "job_pending",
            "status_message": "Job created and pending to start",
            "updated_at": "2022-07-01T09:20:16.763640058Z"
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
                            "name": "name",
                            "value": "$blueprint.resource_group_name",
                            "metadata": {}
                        },
                        {
                            "name": "provision",
                            "value": "$blueprint.provision_rg",
                            "metadata": {}
                        }
                    ],
      ....................
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

For more information, about how to diagnose and resolve issues if the command fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails&interface=cli).

## Next steps
{: #bp-delete-nextsteps}

To destroy or delete a Blueprint, refer to, [destroy a Blueprint](/docs/schematics?topic=schematics-destroy-blueprint&interface=cli) and [delete a Blueprint](/docs/schematics?topic=schematics-delete-blueprint&interface=cli#delete-blueprint-cli).

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
