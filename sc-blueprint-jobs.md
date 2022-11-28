---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: blueprint job, jobs get, jobs list, jobs logs, blueprint jobs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# List blueprint jobs
{: #list-blueprint-jobs}

To list your blueprint jobs with the CLI, use the `ibmcloud schematics blueprint job list` command. The commands are interactive and prompt the user to drill down deeper into the job results. The command takes as input the `<blueprint_ID>`. 
{: shortdesc}

For all the blueprint commands, syntax, and option flag details, see [blueprint commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

## Viewing blueprint job results through UI
{: #blueprint-job-get-ui}
{: ui}

You can follow these steps to view the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. Click your blueprint that is listed in the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external} to view the blueprint details.
2. Click **Jobs history** tab view that the job logs for all blueprint and module operations.
3. Click the job to `expand` or `collapse` arrow on the right side. 

    The detailed view of the blueprint job result can be seen, which contains a number of child jobs. The color coding of the child jobs indicate which job log must be reviewed for further information about job failures. 

    Blueprint operations are performed by child `module` jobs operating against each module (workspace), under the control of a `blueprint` orchestration job.  For Terraform based modules, these are {{site.data.keyword.bpshort}} Workspace jobs. Module (workspace) jobs contain the detail of the IaC operations performed to deploy and configure cloud resources. A blueprint job failure will be typically caused by a Module job failure and the failing module log should be reviewed to identify the cause of the job failure. 

4. Click on the name of a child job to review the job log.  
    - Optional: Click **Show more** to view the full job log. 

## Listing blueprint jobs through CLI
{: #list-blueprint-cli}
{: cli}

Lists all the blueprint jobs, the command is shown in the code block.

```sh
ibmcloud schematics blueprint job list -id <blueprint_ID>
```
{: pre}

The list command returns the list of jobs that runs for this blueprint. This example does not follow the interactive prompt. 

```text        
ID     Blueprint_Basic.eaB.08d1   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_install       Normal   2022-11-18 10:33:41   us-east.JOB.Blueprint_Basic.a14e1e36   
2     blueprint_create_init   Normal   2022-11-18 10:28:30   us-east.JOB.Blueprint_Basic.4cad1d5b   
      
Enter Job sequence number to get the blueprint job summary.(or enter no/n to ignore)> 
```
{: screen}

Following the interactive prompts, you can drill down into the details of a specific blueprint job, then drill down to review the log summary for the operations set at a module level. Enter the sequence number of the job listed in the prior lines to drill down to the next level. 

```text                 
ID     Blueprint_Basic.eaB.08d1   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_install       Normal   2022-11-18 10:33:41   us-east.JOB.Blueprint_Basic.a14e1e36   
2     blueprint_create_init   Normal   2022-11-18 10:28:30   us-east.JOB.Blueprint_Basic.4cad1d5b   
      
Enter Job sequence number to get the blueprint job summary.(or enter no/n to ignore)> 1
BLUEPRINT JOB DETAILS      
Job ID                  us-east.JOB.Blueprint_Basic.a14e1e36   
Job Type                blueprint_install   
Status                  Normal   
Start Time              2022-11-18 10:33:41   
End Time                2022-11-18 10:37:17   
                           
SNO   Child Job           Module ID                                         Job Status     Job ID   
1     blueprint_install                                                     job_finished   us-east.JOB.Blueprint_Basic.a14e1e36   
2     Module_apply        us-east.workspace.basic-resource-group.99503dea   job_finished   de32dba8d99cbfc10bd41e619032237e   
3     Module_apply        us-east.workspace.basic-cos-storage.99a35e10      job_finished   a7326e407a3621dcbead909caf3cdf70   
                          
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 
OK
```
{: screen}


## Viewing blueprint job results through CLI
{: #view-blueprint-job-get-cli}

Review the following section for the `blueprint job get` command for an explanation of the job output. 

To view the summary details of a blueprint job with the CLI, use the `ibmcloud schematics blueprint job get` command. The command is interactive and prompts the user to drill down deeper into the job results. The command takes as input the `job_id`. The `job_id` is displayed when the `Create`, `Apply`, `Update`, `Destroy`, and `Delete` operations are set. It can also be retrieved by using the `blueprint job list` command.  
{: shortdesc}

```sh
ibmcloud schematics blueprint job get -id <job_id>
```
{: pre}

On successful completion, job get command returns the summary detail for the blueprint operation set, along with the status for all the child jobs ran against the blueprint modules. Similar to the job list command, it supports drill down to review a summary of the execution results for all child jobs. 


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
                          
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 
OK
```
{: screen}

The first section of the job output shows the overall execution status of the blueprint operation (job), such as, `config create`, `run apply`, `config update`, `run destroy`, or `config delete`. 

The second section has a detailed breakdown of the job execution results at a module level. 

Blueprint operations are run separately against each `module` in a blueprint and the results aggregated as the status of the overall blueprint operation. The module jobs operate under the control of a `blueprint` orchestration job. A module job contain the results of the Terraform operations performed by that module to deploy and configure cloud resources. A blueprint job failure is typically caused by a failing module job. The failing module log must be reviewed to identify the cause of the blueprint job failure. 

### Blueprint job get summary log
{: #blueprint-job-get-drilldown-cli} 

The example here shows job summary output, following the interactive prompt to review the log summary for the module with sequence number `2`. 


```text          
BLUEPRINT JOB DETAILS      
Job ID                  us-east.JOB.Blueprint_Basic.a14e1e36   
Job Type                blueprint_install   
Status                  Normal   
Start Time              2022-11-18 10:33:41   
End Time                2022-11-18 10:37:17   
                           
SNO   Child Job           Module ID                                         Job Status     Job ID   
1     blueprint_install                                                     job_finished   us-east.JOB.Blueprint_Basic.a14e1e36   
2     Module_apply        us-east.workspace.basic-resource-group.99503dea   job_finished   de32dba8d99cbfc10bd41e619032237e   
3     Module_apply        us-east.workspace.basic-cos-storage.99a35e10      job_finished   a7326e407a3621dcbead909caf3cdf70   
                          
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 2
                 
Module ID     us-east.workspace.basic-resource-group.99503dea   
Status        job_finished   
Log Summary   (last few lines)..........   
              orm Refresh  -----   
                  
               2022/11/18 10:34:55 Starting command: terraform1.0 refresh -state=terraform.tfstate -var-file=schematics.tfvars -no-color   
               2022/11/18 10:34:55 Starting command: terraform1.0 refresh -state=terraform.tfstate -var-file=schematics.tfvars -no-color   
               2022/11/18 10:35:06 Terraform refresh |    
               2022/11/18 10:35:06 Terraform refresh | Outputs:   
               2022/11/18 10:35:06 Terraform refresh |    
               2022/11/18 10:35:06 Terraform refresh | resource_group_id = "aac37f57b20142dba1a435c70aeb12df"   
               2022/11/18 10:35:06 Terraform refresh | resource_group_name = "Default"   
               2022/11/18 10:35:06 Command finished successfully.   
                  
               2022/11/18 10:35:06 -----  Terraform OUTPUT  -----   
                  
               2022/11/18 10:35:06 Starting command: terraform1.0 output -no-color -json   
               2022/11/18 10:35:06 Starting command: terraform1.0 output -no-color -json   
               2022/11/18 10:35:08 Command finished successfully.   
               2022/11/18 10:35:14 Done with the workspace action   
                 
                 
Use ibmcloud schematics logs --id us-east.workspace.basic-resource-group.99503dea --act-id de32dba8d99cbfc10bd41e619032237e to review the full job output 
OK
```
{: screen}

Only the last 15 lines of a child job log are shown. On Terraform failures, these lines typically return sufficient error information to identify the Terraform error causes the blueprint job failure.  

If wanted to review the **full** job log output for a child job. The CLI command views the full job log for a child job that is shown at the end of the get job output. Cut and paste into your command line to view the full log. 

### Viewing blueprint job logs
{: #blueprint-job-log-cli}

The two types of blueprint child job are, `blueprint` and `module`. 

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
 2022/11/18 10:33:45 -----  New blueprint Action  -----
 2022/11/18 10:33:45 Request: blueprintId=Blueprint_Basic.eaB.08d1, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=schematics@in.ibm.com, requestID=1f97fde7-4921-4a46-bb29-296fbe25df44
 2022/11/18 10:33:45 Related Job:  jobID=us-east.JOB.Blueprint_Basic.a14e1e36
 2022/11/18 10:33:57  --- Ready to execute the blueprint flow install command --- 
 2022/11/18 10:33:57 Processing Module Entry us-east.workspace.basic-resource-group.99503dea
 2022/11/18 10:33:59 Module Status for ModuleID=us-east.workspace.basic-resource-group.99503dea is INACTIVE
 2022/11/18 10:35:26 Install activity completed for module ModuleID=us-east.workspace.basic-resource-group.99503dea
 2022/11/18 10:35:28 Status for module ModuleID=us-east.workspace.basic-resource-group.99503dea is ACTIVE
 2022/11/18 10:35:30 Processing Module Entry us-east.workspace.basic-cos-storage.99a35e10
 2022/11/18 10:35:31 Module Status for ModuleID=us-east.workspace.basic-cos-storage.99a35e10 is INACTIVE
 2022/11/18 10:37:11 Install activity completed for module ModuleID=us-east.workspace.basic-cos-storage.99a35e10
 2022/11/18 10:37:13 Status for module ModuleID=us-east.workspace.basic-cos-storage.99a35e10 is ACTIVE
 2022/11/18 10:37:16 ENVIRONMENT_INSTALL - ENVIRONMENT_SYSTEM_STATEENUM_FULFILMENT_SUCCESS
 2022/11/18 10:37:17  Done with the blueprint install flow job 

OK
```
{: screen}


## List blueprint jobs through UI
{: #list-blueprint-jobs-ui}
{: ui}

The results of blueprint operations, `config create`, `run apply`, `config update`, `run destroy`, and `config delete` can be reviewed on the **Jobs history** page of a blueprint. 

1. Click your blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the blueprint details.
2. Click **Overview** to view the blueprint summary that includes `Recent Job runs` of your blueprint. 
3. Click **Jobs history** tab view that the job logs for all blueprint and module operations.

The color coding indicates whether the job was successful or failed. 

## Listing blueprint through API
{: #list-blueprint-api}
{: api}


Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about blueprint config update, refer to, [Installing blueprint](/apidocs/schematics/schematics#create-blueprint) by using API.

Blueprint create API runs `blueprint run apply`, and `blueprint jobs` `APIs` together, to perform the config create, and blueprint run apply operations.
{: important}

Example

```json
GET /v2/jobs/us-south.JOB.Blueprint-Basic-Test-API.b1a7c5b8/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

### Verifying blueprint job results through API 
{: #bp-verify-jobs-api}

Verify that the blueprint jobs is success as shown in the output.
{: shortdesc}

Output

```text
{
    "id": "us-south.JOB.Blueprint-Basic-Test-API.b1a7c5b8",
    "name": "JOB.Blueprint-Basic-Test-API.blueprint_install.1669214160013",
    "description": "Deploys a simple two module blueprint",
    "command_object": "blueprint",
    "command_object_id": "Blueprint-Basic-Test-API.soB.347a",
    "command_name": "blueprint_install",
    "location": "us-south",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "submitted_at": "2022-11-23T14:36:00.013519689Z",
    "submitted_by": "test@in.ibm.com",
    "start_at": "2022-11-23T14:36:00.013512412Z",
    "end_at": "2022-11-23T14:39:03.495863886Z",
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
            "updated_at": "2022-11-23T14:36:00.013526696Z"
        },
        "system_job_status": {
            "updated_at": "0001-01-01T00:00:00Z"
        },
        "flow_job_status": {
            "status_code": "job_finished",
            "workitems": [
                {
                    "workspace_id": "us-south.workspace.basic-resource-group.483b1ea2",
                    "workspace_name": "basic-resource-group",
                    "job_id": "a1dc8ad75a0e779796c0d2a97e12e9b1",
                    "status_code": "job_finished",
                    "updated_at": "2022-11-23T14:36:16.567915615Z"
                },
                {
                    "workspace_id": "us-south.workspace.basic-cos-storage.535f45d6",
                    "workspace_name": "basic-cos-storage",
                    "job_id": "a9c02050ed5b8d06e524757016beebae",
                    "status_code": "job_finished",
                    "updated_at": "2022-11-23T14:37:27.596469934Z"
                }
            ],
            "updated_at": "2022-11-23T14:39:03.495851031Z"
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
                    "workspace_id": "us-south.workspace.basic-resource-group.483b1ea2",
                    "job_id": "a1dc8ad75a0e779796c0d2a97e12e9b1",
                    "log_url": "https://schematics.cloud.ibm.com/v1/workspaces/us-south.workspace.basic-resource-group.483b1ea2/runtime_data/IBM-DefaultResourceGroup-f3ffa820-efbb-4f/log_store/actions/a1dc8ad75a0e779796c0d2a97e12e9b1"
                },
                {
                    "workspace_id": "us-south.workspace.basic-cos-storage.535f45d6",
                    "job_id": "a9c02050ed5b8d06e524757016beebae",
                    "resources_add": 3,
                    "log_url": "https://schematics.cloud.ibm.com/v1/workspaces/us-south.workspace.basic-cos-storage.535f45d6/runtime_data/IBM-Storage-08ca6efb-7af6-40/log_store/actions/a9c02050ed5b8d06e524757016beebae"
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

For more information, about how to diagnose and resolve issues if the list job fails, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-job-nextsteps}

- After displaying the blueprint jobs in {{site.data.keyword.bpshort}}, the next step in maintaining the environment is to [update](/docs/schematics?topic=schematics-update-blueprint) the configuration.

- Looking for blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the `Readme` files of the examples for further customization and usage for each sample. 
