---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-03"

keywords: job stop, schematics interrupt force stop, terminate, force stop

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Stopping or terminating running jobs
{: #interrupt-job}

After invoking a Workspaces job, like a `plan`, an `apply`, or a `destroy`, you may want to stop the running job, or to stop the provisioning of resources. When stopping, or canceling a long running job, it is advisable to first check the job logs to determine whether the job is actually stuck and needs stopping, or if it is performing long running operations that are taking time to complete. 

{{site.data.keyword.bpshort}} provides a number of options to allows users to `(gracefully) stop`, `force-stop`, or `terminate` the running job in order of immediacy and impact of the stop operation. 
{: shortdesc}

## Stop job types
{: #interrupt-types}

The table provides the list of stop job types: 

| Types | Description |
| --- | --- |
| `stop` | Sends an interrupt signal to the running Terraform command. This performs the same function as when running Terraform standalone, where an `interrupt` is sent using `cntl-C` or similar to Terraform. Interrupt signals can be sent multiple times to notify Terraform to stop. Terraform will attempt to gracefully stop the running command (plan, apply, delete) after any executing resource operations have been completed and clean up. {{site.data.keyword.bpshort}} waits for the command to finish and exit. After the command is stopped or finished, state and log files are collected and saved.|
| `force-stop`| Sends a kill signal to the Terraform command running. Use this in the case where you want to kill the Terraform command after seeing that the stop command (interrupts) are not stopping the command. A `force-stop` can be sent as many times as wanted until the command exits and job stops. If it is able to respond to the kill signal, Terraform will stop the running command and resource operations immediately and clean up. After the command is stopped or finished, state file and log files are collected and saved. |
| `terminate` | If the previous operations fail, terminate immediately stops the {{site.data.keyword.bpshort}} job.  The job is marked as `STOPPED` and the workspace unlocked. Any previously saved copies of the logs and `statefile` are preserved. When a job is terminated, the job is killed without collecting any files separately at the end. This option may result in data loss of log file and latest statefile updates and should be used carefully.|
{: caption="Types of job stop" caption-side="bottom"}

Until the job stops, you can send any number of these stop signals. Typically, you should not send more than three signals. If the Terraform does not respond to `stop` requests (interrupt signals), you can always use `force-stop`. If `force-stop` does not respond due to some issue in the job, you can always `terminate` the job to block.
{: important}

## Canceling a job
{: #cancelling}

If the job is in a `pending` state, any type of stop request causes the job to cancel. The `Cancel` button shows up if the job is in a `pending` state, when it can be simply cancel. Cancel removes the job from the pending queue. If the `plan`, `apply`, or `destroy` execution is started in the meanwhile, this will become a stop (interrupt) signal to Terraform. 

## Stopping a running job using the UI
{: #stop-job-ui}
{: ui}

You can follow these steps to stop Workspaces running job by using the console.

1. From the [Workspaces dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace related to the running job.
   
   You can stop or cancel the running job during a plan, an apply, or a destroy execution.
   {: note}

2. Click **Job** tab to view **Stop**, **Force stop**, **Terminate**, and **Cancel** button. 
     
     | Button | Description |
     | --- | --- |
     | Stop  | Removes the job from the pending queue, if it is in pending state. Otherwise, sends an interrupt signal to the Terraform command. |
     | Force stop | Sends a kill signal to the Terraform command running. |
     | Terminate | Terminates the {{site.data.keyword.bpshort}} job and marks the job as STOPPED and unlocks the workspace. This command should be used carefully as it can result in data loss. |
     | Cancel | The `Cancel` button shows up if the job is in `pending` state, when it can be simply canceled. Cancel removes the job from the pending queue. If the `plan`, `apply`, or `destroy` execution is started in the meanwhile, this end up become an interrupt signal to the Terraform execution.|
     {: caption="Stop job options" caption-side="bottom"}

3. Type your `<option>` name in **Type option to confirm** text box.
4. Click **Confirm option** button.

## Stopping a running job using the CLI
{: #stop-job-cli}
{: cli}

Stops a running job for {{site.data.keyword.bplong_notm}} Workspaces.
{: shortdesc}

Syntax

```sh
ibmcloud schematics workspace job stop --id WORKSPACE_ID --job-id JOB_ID [--stop] [--force-stop] [--terminate] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The workspace ID to update. |
| `--job-id` or `--jid` | Required | The job ID of the job. |
| `--stop` | Optional | Removes the job from the pending queue, if it is in pending state. Otherwise, sends an interrupt signal to the Terraform command.|
| `--force-stop` or `--fs` | Optional | Sends a kill signal to the Terraform execution in the engine attempting to immediately stop the execution. |
| `--terminate` or `-t` | Optional | Abruptly kills the engine, marks the job as stopped, and unlocks your workspace. Data is not saved using this flag. |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive mode. |
{: caption="{{site.data.keyword.bpshort}} job stop flags" caption-side="bottom"}

Example


```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --stop --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --force-stop --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --terminate --job-id <JOB_ID>
```
{: pre}

## Stopping a running job through API
{: #stop-job-api}
{: api}

You can use following cURL commands to stop a running job for {{site.data.keyword.bplong_notm}} Workspace. 
{: shortdesc}

### Syntax to stop running jobs
{: #stop-jobs-api}

1. [Set up your REST client](/docs/schematics?topic=schematics-setup-api&interface=api#cs_api) to execute {{site.data.keyword.bpshort}} API.
2. Use the syntax and example to stop the running job.

    Syntax

    ```curl
    curl -X DELETE https://schematics.cloud.ibm.com/v1/workspaces/<wks_id>/actions/{job_id}?signal=interrupt -H "Authorization: <iam_token>"
    ```
    {: codeblock}

    Example

    ```curl
    curl -X DELETE https://schematics.cloud.ibm.com/v2/jobs/{job_id}?signal=interrupt -H "Authorization: <iam_token>"
    ```
    {: codeblock}

3. Use the example to `force-stop` the running job.
   
   Example

   ```curl
   curl -X DELETE https://schematics.cloud.ibm.com/v1/workspaces/<wks_id>/actions/{job_id}?signal=force-stop -H "Authorization: <iam_token>"
   ```
   {: codeblock}

4. Use the example to `terminate` the running job.
   
   Example

   ```curl
   curl -X DELETE https://schematics.cloud.ibm.com/v1/workspaces/<wks_id>/actions/{job_id}?signal=terminate -H "Authorization: <iam_token>"
   ```
   {: codeblock}

   For more information about stopping the running job, see [Stop and delete the running Job](/apidocs/schematics/schematics#delete-workspace-activity) API.
   {: note}


