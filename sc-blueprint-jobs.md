<staging>---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-25"

keywords: blueprint job, jobs get, jobs list, jobs logs, blueprint jobs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

## Listing Blueprint jobs
{: #list-blueprint-jobs-cli}
{: cli}

To list your Blueprint jobs with the CLI, use the `ibmcloud schematics blueprint job list` command. The commands are interactive and will prompt the user to drill down deeper into the job results. The command takes as input the `<blueprint_id>`. 
{: shortdesc}

**Syntax:**

```sh
ibmcloud schematics blueprint job list -id <blueprint_id>
```
{: pre}

On successful completion the list command returns the list of jobs executed for this Blueprint. This example does not follow the interactive prompt. 

### Blueprint job list 
{: #list-blueprint-output-cli} 

```text        
ID     us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_install       Normal   2022-07-08 13:54:31   us-south.JOB.Blueprint_Complex.357fc181   
2     blueprint_create_init   Normal   2022-07-08 11:03:10   us-south.JOB.Blueprint_Complex.57b42827   
      
Enter Job sequence number to get the Blueprint job summary.(or enter no/n to ignore)> 
```
{: screen}

Following the interactive prompts, you can drill down into the details of a specific Blueprint job, then drill down to review the log summary for operations performed at a module level. Enter the sequence number of the job listed in the prior lines to drill down to the next level. 

```text                 
ID     us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_install       Normal   2022-07-08 13:54:31   us-south.JOB.Blueprint_Complex.357fc181   
2     blueprint_create_init   Normal   2022-07-08 11:03:10   us-south.JOB.Blueprint_Complex.57b42827   
      
Enter Job sequence number to get the Blueprint job summary.(or enter no/n to ignore)> 1
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Job Type                blueprint_install   
Status                  Normal   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_install                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
2     Module_apply        us-south.workspace.terraform_module1.6cef8e6d   job_finished   67a110742e5e3f336262ca3ab994048f   
3     Module_apply        us-south.workspace.terraform_module2.875fda22   job_finished   dced88881344254af903fac251731ed2   
                          
Enter Job sequence number to get Blueprint child job output summary(or enter no/n to ignore)> 
```
{: screen}

Review the following section for the `blueprint job get` command for an explanation of the job output. 


## Viewing Blueprint job results
{: #blueprint-job-get-cli}
{: cli}

To view the summary details of a Blueprint job with the CLI, use the `ibmcloud schematics blueprint job get` command. The command is interative and will prompt the user to drill down deeper into the job results. The command takes as input the `job_id`. The `job_id` is displayed when the `create`, `install`, `update`, `destroy` and `delete` operations are performed. It can also be retrieved using the `blueprint job list` command.  
{: shortdesc}


**Syntax:**

```sh
ibmcloud schematics blueprint job get -id <job_id>
```
{: pre}

On successful completion the job get command returns the summary detail for the Blueprint operation performed, along with the status for all the child jobs executed against the Blueprint modules. Similar to the job list command, it supports drill down to review a summary of the execution results for all child jobs. 


### Blueprint job get 
{: #blueprint-job-get-cli} 

The example here shows job summary output without following the interative prompt.  

```text          
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Blueprint ID            us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0   
Job Type                blueprint_install   
Location                us-south   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
Status                  Normal   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_install                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
2     Module_apply        us-south.workspace.terraform_module1.6cef8e6d   job_finished   67a110742e5e3f336262ca3ab994048f   
3     Module_apply        us-south.workspace.terraform_module2.875fda22   job_finished   dced88881344254af903fac251731ed2   
                          
Enter Job sequence number to get Blueprint child job output summary(or enter no/n to ignore)> 
OK
```
{: screen}

The first section of the job output shows the overall execution status of the Blueprint operation (job), e.g. `create`, `install`, `update`, `destroy` or `delete`. 

The second section has a detailed breakdown of the execution results at a Workspace level. 

Blueprint operations are performed by child `module` jobs operating against each module (Workspace), under the control of a `blueprint` orchestration job.  For Terraform based modules, these are {{site.data.keyword.bpshort}} Workspace jobs. Module (Workspace) jobs contain the detail of the IaC operations performed to deploy and configure cloud resources. A Blueprint job failure will be typically caused by a Module job failure and the failing module log should be reviewed to identify the cause of the job failure. 

### Blueprint job get summary log
{: #blueprint-job-get-drilldown-cli} 

The example here shows job summary output, following the interactive prompt to review the log summary for the module with sequence number `2`. 


```text          
BLUEPRINT JOB DETAILS      
Job ID                  us-south.JOB.Blueprint_Complex.357fc181   
Blueprint ID            us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0   
Job Type                blueprint_install   
Location                us-south   
Start Time              2022-07-08 13:54:31   
End Time                2022-07-08 13:56:42   
Status                  Normal   
                           
SNO   Child Job           Module ID                                       Job Status     Job ID   
1     blueprint_install                                                   job_finished   us-south.JOB.Blueprint_Complex.357fc181   
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

Only the last 15 lines of a child job log are shown. On Terraform failures, these lines typically return sufficient error information to identify the Terraform error causing the Blueprint job failure.  

If it is desired to review the **full** job log output for a child job, the CLI command to view the full job log for a child job is shown at the end of the get job output. Cut and paste this into your terminal window to view the full log. 


## Viewing Blueprint job logs
{: #blueprint-job-log-cli}
{: cli}

The two types of Blueprint child job are, `blueprint` and `module`. 

Full `module` job logs can be reviewed with the `schematics logs` command as in the previous section. 

To view the full `blueprint` orchestration job log with the CLI, use the `ibmcloud schematics blueprint job logs` command. This command provides more detail than the summary log with the `blueprint job get` command. The command takes as input the `job_id`. The `job_id` is displayed when the `create`, `install`, `update`, `destroy` and `delete` commands are executed. It can also be retrieved using the `blueprint job list` command.  
{: shortdesc}


**Syntax:**

```sh
ibmcloud schematics blueprint job logs -id <job_id>
```
{: pre}


### Blueprint job log output
{: #blueprint-job-log-cli} 

```text        
 2022/07/08 13:54:34 -----  New Environment Action  -----
 2022/07/08 13:54:34 Request: environmentId=us-south.ENVIRONMENT.Blueprint_Complex.5448a1c0, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=geetha_sathyamurthy@in.ibm.com, requestID=6aa7b2c7-565f-418d-9596-70d6edf08d05
 2022/07/08 13:54:34 Related Job:  jobID=us-south.JOB.Blueprint_Complex.357fc181
 2022/07/08 13:54:44  --- Ready to execute the environment flow install command --- 
 2022/07/08 13:54:44 Processing WorkItem Entry us-south.workspace.terraform_module1.6cef8e6d
 2022/07/08 13:54:44 Work Item Status for WorkItemID=us-south.workspace.terraform_module1.6cef8e6d is INACTIVE
 2022/07/08 13:55:34 Install activity completed for workitem WorkItemID=us-south.workspace.terraform_module1.6cef8e6d
 2022/07/08 13:55:34 Status for workitem WorkItemID=us-south.workspace.terraform_module1.6cef8e6d is ACTIVE
 2022/07/08 13:55:37 Processing WorkItem Entry us-south.workspace.terraform_module2.875fda22
 2022/07/08 13:55:38 Work Item Status for WorkItemID=us-south.workspace.terraform_module2.875fda22 is INACTIVE
 2022/07/08 13:56:38 Install activity completed for workitem WorkItemID=us-south.workspace.terraform_module2.875fda22
 2022/07/08 13:56:38 Status for workitem WorkItemID=us-south.workspace.terraform_module2.875fda22 is ACTIVE
 2022/07/08 13:56:40 ENVIRONMENT_INSTALL - ENVIRONMENT_SYSTEM_STATEENUM_FULFILMENT_SUCCESS
 2022/07/08 13:56:42  Done with the environment install flow job 

OK
```
{: screen}



## Listing Blueprint jobs
{: #list-blueprint-jobs-ui}
{: ui}

The results of Blueprints operations, `create`, `install`, `update`, `destroy` and `delete` can be reviewed on the **Jobs history** page of a Blueprint. 

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Overview** to view the BLueprint summary, this includes `Recent Job runs` of your Blueprint. 
3. Click **Jobs history** tab view the job logs for all Blueprint and module operations.

The color coding indicates if the job was successful or failed. 

## Viewing Blueprint job results
{: #blueprint-job-get-ui}
{: ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Jobs history** tab view the job logs for all Blueprint and module operations.
3. Navigate to the line of the desired job and click the expand/collapse arrow on the right hand side. 

This will open up a detailed view of the Blueprint job results, which contains a number of child jobs. The color coding of the child jobs will indicate which job log should be reviewed for further information about job failures. 

Blueprint operations are performed by child `module` jobs operating against each module (Workspace), under the control of a `blueprint` orchestration job.  For Terraform based modules, these are {{site.data.keyword.bpshort}} Workspace jobs. Module (Workspace) jobs contain the detail of the IaC operations performed to deploy and configure cloud resources. A Blueprint job failure will be typically caused by a Module job failure and the failing module log should be reviewed to identify the cause of the job failure. 

4. Click on the name of a child job to review the job log.  
    - Optional: Click **Show more** to view the full job log. 