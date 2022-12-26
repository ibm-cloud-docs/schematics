---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: blueprint delete, delete blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Delete a blueprint configuration
{: #delete-blueprint}

When a blueprint environment is no longer required, it can be deleted which will terminate billing for all deployed resources. See the [deleting blueprints](/docs/schematics?topic=schematics-delete-blueprint) lifecycle stage to understand the process of deleting blueprint environments and the steps. Deleting an environment is a two-step process that first destroys all the associated cloud resources (environment) and second deletes the blueprint config in {{site.data.keyword.bpshort}}.
{: shortdesc}

Deleting the blueprint configuration is the second step required to completely delete a blueprint from {{site.data.keyword.bpshort}}. To protect from accidental deletion, the config can only be deleted when cloud resources in all the blueprint modules have been deleted and the modules are in `Inactive` state. The first step is to run the [blueprint destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=ui) command to destroy the resources in the blueprint environment and remove the environment. 

This behavior can be overridden using the `-force-delete` flag when modules cannot be returned to an `Inactive` state due to a {{site.data.keyword.bpshort}} or Terraform error. 
{: shortdesc}

## Deleting a blueprint config using the CLI
{: #delete-blueprint-cli}
{: cli}

For more information, see the [blueprint delete](/docs/schematics?topic=schematics-delete-blueprint) command. The `blueprint destroy` command must have been run first to destroy the resources, only then can the `blueprint delete` command run. 

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint delete --id Blueprint_Basic.eaB.08d1
```
{: pre}

Output

```text
Modules to be deleted
SNO   Module Type   Name                   Status   
1     Workspace     basic-resource-group   INITIALISED   
2     Workspace     basic-cos-storage      INITIALISED   
      
Do you really want to delete the blueprint ? [y/N]> y
Job : us-east.JOB.Blueprint_Basic.992e4c2d Created

Job Type: BLUEPRINT DELETE

OK
```
{: screen}

### Verifying blueprint config deletion 
{: #verify-bp-delete-cli}

The delete CLI command does not wait for successful job completion and returns immediately. 

The status of the delete operation can be monitored by using the `blueprint job get` command. The following command runs a `blueprint job get` for the JOB ID `eu-gb.JOB.Blueprint-Basic-Example.f2d388d3`. The job ID is displayed in the delete output. 

```sh
ibmcloud schematics blueprint job get --id us-east.JOB.Blueprint_Basic.992e4c2d
```
{: pre}


```text
BLUEPRINT JOB DETAILS      
Job ID                  us-east.JOB.Blueprint_Basic.992e4c2d   
Blueprint ID            Blueprint_Basic.eaB.08d1   
Job Type                blueprint_delete   
Location                us-east   
Start Time              2022-11-18 11:22:03   
End Time                2022-11-18 11:22:44   
Status                  Normal   
                           
SNO   Child Job          Module ID   Job Status     Job ID   
1     blueprint_delete               job_finished   us-east.JOB.Blueprint_Basic.992e4c2d   
                         
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 1
                 
Module ID        
Status        job_finished   
Log Summary   (last few lines)..........  
               2022/11/18 11:22:06 -----  New blueprint Action  -----   
               2022/11/18 11:22:06 Request: blueprintId=Blueprint_Basic.eaB.08d1, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=schematics@in.ibm.com, requestID=1d7ee769-71f1-45f0-93f9-c0cd159e4117   
               2022/11/18 11:22:07 Related Job:  jobID=us-east.JOB.Blueprint_Basic.992e4c2d   
               2022/11/18 11:22:18  --- Ready to execute the blueprint flow delete command ---    
               2022/11/18 11:22:22 Invoke delete blueprint module component - us-east.workspace.basic-cos-storage.99a35e10   
               2022/11/18 11:22:33 Invoke delete blueprint module component - us-east.workspace.basic-resource-group.99503dea   
               2022/11/18 11:22:44 Done with the blueprint delete flow job   
                 
                 
Use ibmcloud schematics blueprint job logs --id us-east.JOB.Blueprint_Basic.992e4c2d to review the full job output 
OK
```
{: screen}

During the delete operation that the status shows `In Progress`, when completed the status changes to `Normal`. The blueprint configuration and all the dependent cloud resources are now deleted. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-apply-fails).

## Deleting a blueprint config using the UI 
{: #delete-blueprint-ui}
{: ui}

You can follow these steps to delete the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click the name of the blueprint that you want to delete.
2. Select the **Actions** drop down list.
3. Select **Delete blueprint** option.
4. Type your `<blueprint name>` in **Delete blueprint** text box.
5. Click **Delete blueprint** button.

### Verifying blueprint config deletion 
{: #verify-bp-deletion-ui}

1. From the [{{site.data.keyword.cloud_notm}} Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}, see that the deleted blueprint is not displayed.

## Deleting a blueprint config using the API
{: #delete-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Delete a blueprint config](/apidocs/schematics/schematics#delete-blueprint) by using API. 

You need to run `blueprint destroy` command and then run `blueprint delete` command. For more information, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprint) configuration.

Record the blueprint ID that needs to be deleted. To list the blueprint ID, run [get all the blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

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
    "command_object_id": "Blueprint-Basic-Test-API.soB.347a"
}

```
{: codeblock}

### Verifying blueprint delete using the API
{: #verify-bp-delete-api}

Verify that the blueprint is deleted successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test-API.soB.347a",
    "command_name": "blueprint_delete",
    "id": "us-south.JOB.Blueprint-Basic-Test-API.58700253",
    "name": "JOB.Blueprint-Basic-Test-API.blueprint_delete.1669215421598",
    "description": "Deploys a simple two module blueprint successfully Updated",
    "location": "us-south",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "submitted_at": "2022-11-23T14:57:01.598150765Z",
    "submitted_by": "test@in.ibm.com",
    "start_at": "2022-11-23T14:57:01.598139929Z",
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
            "updated_at": "2022-11-23T14:57:01.598203447Z"
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
                        "command_name": "workspace_destroy",
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
                        "command_name": "workspace_destroy",
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
{: #bp-delete-nextsteps}

- Looking for additional blueprint samples to work with? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the `Readme` files of the examples for further customization and usage for each sample. 
