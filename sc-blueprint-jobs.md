---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-21"

keywords: blueprint job, jobs get, jobs list, jobs logs, blueprint jobs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}


# Listing Blueprint jobs
{: #list-blueprint-jobs-cli}

To list your Blueprint jobs with the CLI, use the `ibmcloud schematics blueprint job list` command. The commands are interactive and prompt the user to drill down deeper into the job results. The command takes as input the `<blueprint_id>`. 
{: shortdesc}

For all the Blueprints commands, syntax, and option flag details, see [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

## Listing Blueprint jobs through CLI
{: #list-blueprint-cli}
{: cli}

Lists all the Blueprint job, the command is shown in the code block.

```sh
ibmcloud schematics blueprint job list -id <blueprint_id>
```
{: pre}

The list command returns the list of jobs that runs for this Blueprint. This example does not follow the interactive prompt. 

```text        
ID     us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_apply       Normal   2022-07-08 13:54:31   us-south.JOB.Blueprint_Complex.357fc181   
2     blueprint_create_init   Normal   2022-07-08 11:03:10   us-south.JOB.Blueprint_Complex.57b42827   
      
Enter Job sequence number to get the Blueprint job summary.(or enter no/n to ignore)> 
```
{: screen}

Following the interactive prompts, you can drill down into the details of a specific Blueprint job, then drill down to review the log summary for the operations set at a module level. Enter the sequence number of the job listed in the prior lines to drill down to the next level. 

```text                 
ID     us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_apply       Normal   2022-07-08 13:54:31   us-south.JOB.Blueprint_Complex.357fc181   
2     blueprint_create_init   Normal   2022-07-08 11:03:10   us-south.JOB.Blueprint_Complex.57b42827   
      
Enter Job sequence number to get the Blueprint job summary.(or enter no/n to ignore)> 1
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Job Type                blueprint_apply   
Status                  Normal   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_apply                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
2     Module_apply        us-south.workspace.terraform_module1.6cef8e6d   job_finished   67a110742e5e3f336262ca3ab994048f   
3     Module_apply        us-south.workspace.terraform_module2.875fda22   job_finished   dced88881344254af903fac251731ed2   
                          
Enter Job sequence number to get Blueprint child job output summary(or enter no/n to ignore)> 
```
{: screen}


## Viewing Blueprint job results through CLI
{: #view-blueprint-job-get-cli}

Review the following section for the `blueprint job get` command for an explanation of the job output. 

To view the summary details of a Blueprint job with the CLI, use the `ibmcloud schematics blueprint job get` command. The command is interactive and prompts the user to drill down deeper into the job results. The command takes as input the `job_id`. The `job_id` is displayed when the `Create`, `Apply`, `Update`, `Destroy`, and `Delete` operations are set. It can also be retrieved by using the `blueprint job list` command.  
{: shortdesc}

```sh
ibmcloud schematics blueprint job get -id <job_id>
```
{: pre}

On successful completion, job get command returns the summary detail for the Blueprint operation set, along with the status for all the child jobs ran against the Blueprint modules. Similar to the job list command, it supports drill down to review a summary of the execution results for all child jobs. 


### Blueprint job get 
{: #blueprint-job-get-cli} 

The example here shows job summary output without following the interactive prompt.  

```text          
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Blueprint ID            us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0   
Job Type                blueprint_apply   
Location                us-south   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
Status                  Normal   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_apply                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
2     Module_apply        us-south.workspace.terraform_module1.6cef8e6d   job_finished   67a110742e5e3f336262ca3ab994048f   
3     Module_apply        us-south.workspace.terraform_module2.875fda22   job_finished   dced88881344254af903fac251731ed2   
                          
Enter Job sequence number to get Blueprint child job output summary(or enter no/n to ignore)> 
OK
```
{: screen}

The first section of the job output shows the overall execution status of the Blueprint operation (job), such as, `create`, `apply`, `update`, `destroy`, or `delete`. 

The second section has a detailed breakdown of the execution results at a Workspace level. 

Blueprint operations are set by the child `module`. The jobs operate against each module (Workspace), under the control of a `blueprint` orchestration job. For the Terraform based modules, it is the {{site.data.keyword.bpshort}} Workspace jobs. Module (Workspace) jobs contain the detail of the IaC operations set to deploy and configure cloud resources. A Blueprint job failure is typically caused by a Module job failure and the failing module log must be reviewed to identify the cause of the job failure. 

### Blueprint job get summary log
{: #blueprint-job-get-drilldown-cli} 

The example here shows job summary output, following the interactive prompt to review the log summary for the module with sequence number `2`. 


```text          
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Blueprint ID            us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0   
Job Type                blueprint_apply   
Location                us-south   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
Status                  Normal   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_apply                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
2     Module_apply        us-south.workspace.terraform_module1.6cef8e6d   job_finished   67a110742e5e3f336262ca3ab994048f   
3     Module_apply        us-south.workspace.terraform_module2.875fda22   job_finished   dced88881344254af903fac251731ed2   
                          
Enter Job sequence number to get Blueprint child job output summary(or enter no/n to ignore)> 2
                 
Module ID     us-south.workspace.terraform_module1.6cef8e6d   
Status        job_finished   
Log Summary   (last few lines)..........   
              7/08 13:55:22 Terraform apply |   {   
               2022/07/08 13:55:22 Terraform apply |     "external" = 9901   
               2022/07/08 13:55:22 Terraform apply |     "internal" = 9901   
               2022/07/08 13:55:22 Terraform apply |     "protocol" = "ldp"   
               2022/07/08 13:55:22 Terraform apply |   },   
               2022/07/08 13:55:22 Terraform apply | ])   
               2022/07/08 13:55:22 Terraform apply | test_tuple = [   
               2022/07/08 13:55:22 Terraform apply |   "hello",   
               2022/07/08 13:55:22 Terraform apply |   "hi",   
               2022/07/08 13:55:22 Terraform apply |   34.5,   
               2022/07/08 13:55:22 Terraform apply |   false,   
               2022/07/08 13:55:22 Terraform apply | ]   
               2022/07/08 13:55:22 Command finished successfully.   
                  
               2022/07/08 13:55:22 -----  Terraform OUTPUT  -----   
                  
               2022/07/08 13:55:22 Starting command: terraform1.0 output -no-color -json   
               2022/07/08 13:55:22 Starting command: terraform1.0 output -no-color -json   
               2022/07/08 13:55:23 Command finished successfully.   
               2022/07/08 13:55:28 Done with the workspace action   
                 
                 
Use ibmcloud schematics logs --id us-south.workspace.terraform_module1.6cef8e6d --act-id 67a110742e5e3f336262ca3ab994048f to review the full job output 
OK
```
{: screen}

Only the last 15 lines of a child job log are shown. On Terraform failures, these lines typically return sufficient error information to identify the Terraform error causes the Blueprint job failure.  

If wanted to review the **full** job log output for a child job. The CLI command views the full job log for a child job that is shown at the end of the get job output. Cut and paste into your command line to view the full log. 

### Viewing Blueprint job logs
{: #blueprint-job-log-cli}

The two types of Blueprint child job are, `blueprint` and `module`. 

Full `module` job logs can be reviewed with the `schematics logs` command as in the previous section. 

To view the full `blueprint` orchestration job log with the CLI, use the `ibmcloud schematics blueprint job logs` command. This command provides more detail than the summary log with the `blueprint job get` command. The command takes as input the `job_id`. The `job_id` is displayed when the `Create`, `Apply`, `Update`, `Destroy`, and `Delete` commands ran. It can also be retrieved by using the `blueprint job list` command.  
{: shortdesc}

The syntax is given in the code block.

```sh
ibmcloud schematics blueprint job logs -id <job_id>
```
{: pre}


Output

```text        
 2022/07/08 13:54:34 -----  New Environment Action  -----
 2022/07/08 13:54:34 Request: environmentId=us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=geetha_sathyamurthy@in.ibm.com, requestID=6aa7b2c7-565f-418d-9596-70d6edf08d05
 2022/07/08 13:54:34 Related Job:  jobID=us-south.JOB.Blueprint_Complex.357fc181
 2022/07/08 13:54:44  --- Ready to execute the environment flow apply command --- 
 2022/07/08 13:54:44 Processing WorkItem Entry us-south.workspace.terraform_module1.6cef8e6d
 2022/07/08 13:54:44 Work Item Status for WorkItemID=us-south.workspace.terraform_module1.6cef8e6d is INACTIVE
 2022/07/08 13:55:34 apply activity completed for workitem WorkItemID=us-south.workspace.terraform_module1.6cef8e6d
 2022/07/08 13:55:34 Status for workitem WorkItemID=us-south.workspace.terraform_module1.6cef8e6d is ACTIVE
 2022/07/08 13:55:37 Processing WorkItem Entry us-south.workspace.terraform_module2.875fda22
 2022/07/08 13:55:38 Work Item Status for WorkItemID=us-south.workspace.terraform_module2.875fda22 is INACTIVE
 2022/07/08 13:56:38 apply activity completed for workitem WorkItemID=us-south.workspace.terraform_module2.875fda22
 2022/07/08 13:56:38 Status for workitem WorkItemID=us-south.workspace.terraform_module2.875fda22 is ACTIVE
 2022/07/08 13:56:40 ENVIRONMENT_apply - ENVIRONMENT_SYSTEM_STATEENUM_FULFILMENT_SUCCESS
 2022/07/08 13:56:42  Done with the environment apply flow job 

OK
```
{: screen}


## Listing Blueprint jobs through UI
{: #list-blueprint-jobs-ui}
{: ui}

The results of Blueprints operations, `create`, `apply`, `update`, `destroy`, and `delete` can be reviewed on the **Jobs history** page of a Blueprint. 

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Overview** to view the Blueprint summary that includes `Recent Job runs` of your Blueprint. 
3. Click **Jobs history** tab view that the job logs for all Blueprint and module operations.

The color coding indicates whether the job was successful or failed. 

## Viewing Blueprint job results through UI
{: #blueprint-job-get-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Jobs history** tab view that the job logs for all Blueprint and module operations.
3. Click the job to `expand` or `collapse` arrow on the right side. 

The detailed view of the Blueprint job result can be seen, which contains a number of child jobs. The color coding of the child jobs indicate which job log must be reviewed for further information about job failures. 

Blueprint operations are performed by child `module` jobs operating against each module (Workspace), under the control of a `blueprint` orchestration job.  For Terraform based modules, these are {{site.data.keyword.bpshort}} Workspace jobs. Module (Workspace) jobs contain the detail of the IaC operations performed to deploy and configure cloud resources. A Blueprint job failure will be typically caused by a Module job failure and the failing module log should be reviewed to identify the cause of the job failure. 
4. Click on the name of a child job to review the job log.  
    - Optional: Click **Show more** to view the full job log. 

## Listing Blueprint from the API
{: #install-blueprint-api}
{: api}


Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about Blueprint update, refer to, [Installing Blueprint](/apidocs/schematics/schematics#create-blueprint) by using API.

Blueprint create API runs `Blueprint install`, and `Blueprint jobs` `APIs` together, to perform the create, and install Blueprint operations.
{: important}

Example

```json
GET /v2/jobs/us-east.JOB.Blueprint-Basic-Test.29bba543/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

Output:

```text
{
    "id": "us-east.JOB.Blueprint-Basic-Test.29bba543",
    "name": "JOB.Blueprint-Basic-Test.blueprint_create_init.1663235859012",
    "description": "Deploys a simple two module blueprint",
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test.eaB.bbb9",
    "command_name": "blueprint_create_init",
    "location": "us-east",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "submitted_at": "2022-09-15T09:57:39.011795189Z",
    "submitted_by": "smulampa@in.ibm.com",
    "start_at": "2022-09-15T09:57:39.011790195Z",
    "end_at": "2022-09-15T09:58:48.146982565Z",
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
            "updated_at": "2022-09-15T09:57:39.011800186Z"
        },
        "system_job_status": {
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "flow_job_status": {
            "status_code": "job_finished",
            "workitems": [
                {
                    "workspace_id": "us-east.workspace.basic-resource-group.d92dd0b6",
                    "workspace_name": "basic-resource-group",
                    "status_code": "job_finished",
                    "updated_at": "2022-09-15T09:58:17.599926716Z"
                },
                {
                    "workspace_id": "us-east.workspace.basic-cos-storage.8e3e7448",
                    "workspace_name": "basic-cos-storage",
                    "status_code": "job_finished",
                    "updated_at": "2022-09-15T09:58:46.838327639Z"
                }
            ],
            "updated_at": "0001-01-01T00:00:00Z"
        }
    },
    "log_summary": {
        "log_start_at": "0001-01-01T00:00:00Z",
        "log_analyzed_till": "0001-01-01T00:00:00Z",
        "repo_download_job": {},
        "workspace_job": {},
        "flow_job": {
            "workitems_completed": 2,
            "workitems": [
                {
                    "workspace_id": "us-east.workspace.basic-resource-group.d92dd0b6",
                    "job_id": "554aceeabf87d9b1b8f9c55c41432e17",
                    "log_url": "https://schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.basic-resource-group.d92dd0b6/runtime_data/IBM-ResourceGroup-61535f6c-5cfd-40/log_store/actions/554aceeabf87d9b1b8f9c55c41432e17"
                },
                {
                    "workspace_id": "us-east.workspace.basic-cos-storage.8e3e7448",
                    "job_id": "4f90836a3291d4949851e7b678d1f4ee",
                    "log_url": "https://schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.basic-cos-storage.8e3e7448/runtime_data/IBM-Storage-38af0396-64ed-48/log_store/actions/4f90836a3291d4949851e7b678d1f4ee"
                }
            ]
        },
        "action_job": {
            "recap": {}
        },
        "system_job": {}
    },
    "updated_at": "0001-01-01T00:00:00Z"
}
```
{: screen}

For more information, about how to diagnose and resolve issues if the list job fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-create-nextsteps}

After displaying the Blueprint jobs in {{site.data.keyword.bpshort}}, the next step in updating the blueprint is to [Update](/docs/schematics?topic=schematics-update-blueprint&interface=api) the configuration.

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 

