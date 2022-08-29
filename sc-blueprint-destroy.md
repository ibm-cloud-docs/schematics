---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-29"

keywords: blueprint destroy, destroy blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Destroying a Blueprint
{: #destroy-blueprint}

The cloud resources created by a Blueprint are destroyed using the `blueprint destroy` command. If it is then needed to remove the Blueprint from Schematics, this is performed after all resources have been destroyed using the [Blueprint destroy](/docs/schematics?topic=schematics-destroy-blueprint&interface=cli) command. Refer to [Blueprints lifecycle](/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands create, update, and delete and the Blueprints lifecycle. 

For Terraform Workspaces, destroy runs a Terraform destroy operation against each Workspace in turn. This removes all cloud resources in reverse dependency order. 

## Destroying a Blueprint from the UI 
{: #destroy-blueprint-ui}
{: ui}

You can only destroy the cloud resources created by a Blueprint from command-line by using the [destroy command](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete).

### Verify Blueprint destroy 
{: #verify-bp-destory-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the destroy operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`, `Recent Job runs` of your Blueprint. 
3. Click **Modules** tab to see the list of resource modules. These will be in an `Inactive` state.
4. Click **Jobs history** tab view the result of the Blueprint destroy job and operations performed against the resource modules.  

## Destroying a Blueprint from the CLI
{: #destroy-blueprint-cli}
{: cli}

The following command performs a Blueprint destroy for the Blueprint with the ID `eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936`

For all the Blueprints commands, syntax, and option flag details, refer to, [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint destroy --id Blueprint_Basic.eaB.5cd9
```
{: pre}

On successful completion the destroy command returns **`fullfilment_success`**. 

For more information, about the command options, see [Destroy command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-destroy).

### Verify Blueprint destroy success 
{: #verify-bp-destory-cli}

Verify that the Blueprint resources have been destroyed successfully. When you run destroy from the CLI, the command displays details of the Workspaces to be destroyed and the status of {{site.data.keyword.bpshort}} jobs executing the Terraform destroy operations. After prompting to confirm that the user intends to destory all resources, the command only returns on completion.

```text
Modules to be destroyed
SNO Type Name Status
1 terraform basic-resource-group ACTIVE
2 terraform basic-cos-storage ACTIVE

Do you really want to destroy all the resources of Blueprint
Blueprint_Basic.eaB.5cd9? [y/N]> y
Blueprint job running us-east.JOB.Blueprint_Basic.f69f5bf2
Waiting:0 Draft:0 Connecting:0 In Progress:0 Inactive:2 Active:0
Failed:0
Type Name Status Job ID
Blueprint Blueprint_Basic FULFILMENT_SUCCESS useast.JOB.Blueprint_Basic.f69f5bf2
terraform basic-resource-group INACTIVE
terraform basic-cos-storage INACTIVE

Blueprint ID Blueprint_Basic.eaB.5cd9 fulfilment_success at 2022-08-04
17:48:36
OK
```
{: screen}

On successful completion the destroy command will return **fulfillment_success**. Successful command completion and the status of the Workspaces as `Inactive` indicates that resources in all linked Workspaces have been destroyed


## Destroying a Blueprint from the API
{: #destroy-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about Blueprint delete, refer to, [Destroy a Blueprint](/apidocs/schematics/schematics#delete-blueprint) by using API.

Blueprint destroy API runs `Blueprint destroy`, and `Blueprint jobs` `APIs` together, to performs the delete, and install Blueprint operations.
{: important}

Record the Blueprint ID that needs to be destroyed. To list the Blueprint ID, run [Get all the Blueprint instances](/apidocs/schematics/schematics#list-blueprint) command.

Example

```json
POST /v2/jobs/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer 

{
    "command_name": "env_destroy",
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
    "command_name": "env_destroy",
    "id": "us-south.JOB.Blueprint-Basic-Example.89fca6a0",
    "name": "JOB.Blueprint-Basic-Example.env_destroy.1656666265457",
    "description": "Simple two module blueprint. Deploys Resource Group and COS bucket",
    "location": "us-south",
    "resource_group": "47ecbb1f38ea4b8aa0a091edb1e4e909",
    "submitted_at": "2022-07-01T09:04:25.457117754Z",
    "submitted_by": "kgurudut@in.ibm.com",
    "start_at": "2022-07-01T09:04:25.457110765Z",
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
            "updated_at": "2022-07-01T09:04:25.457124147Z"
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
     ..........
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
{: #bp-destroy-nextsteps}

After the cloud resources are destroyed, the Blueprint can be [deleted](/docs/schematics?topic=schematics-delete-blueprint&interface=cli) from {{site.data.keyword.bpshort}}. Alternatively ,the cloud environment can be re-constituted and the resources re-created by running [Blueprint install](/docs/schematics?topic=schematics-install-blueprint&interface=cli) again using the same Blueprint configuration.

The configuration of the Blueprint and outputs can be reviewed using the `blueprint get` command. See section [Displaying Blueprints](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-get). 

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
