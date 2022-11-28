---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: job stop, schematics interrupt force stop, terminate, force stop

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Stopping or terminating running jobs
{: #interrupt-job}

After invoking a job on a {{site.data.keyword.bpshort}} Workspaces like a `plan`, an `apply`, or a `destroy`. You may want to stop the running job, or want to stop provisioning resources. Stopping, cancel a job helps you to know whether the job is stuck, or if the job has lot of wait time. {{site.data.keyword.bpshort}} allows users to `interrupt`, `force-stop`, or `terminate` the running job.
{: shortdesc}

## Stopping job types
{: #interrupt-types}

The table provides the list of interrupting types of the job stop.

| Types | Description |
| --- | --- |
| `interrupt` | Sends an interrupt signal to the Terraform command that you invoke. Typically if you see the job log and click `interrupt` expecting an interrupt signal to be sent. Such interrupt signal can be sent as many times as possible while the job is running. {{site.data.keyword.bpshort}} waits for the command to finish and exit. After the command is stopped or finished, state and log files are collected and saved.|
| `force-stop`| Sends a kill signal to the Terraform command running. In case you want to force kill the Terraform command after seeing the interrupts not stopping the command. A `force-stop` can be sent as many times as wanted until the command exits and job stops. After the command is stopped or finished, state file and log files are collected and saved. |
| `terminate` | This terminates the job in backend and mark the job as `STOPPED` and unlocks the workspace. The {{site.data.keyword.bpshort}} saves log and `statefile` to the backend periodically while the job is running. If a job is terminated, the job is killed without collecting any files separately at the end. |
{: caption="Types of job interruption" caption-side="bottom"}

Until the job stops, you can send any number of these stop signals. Typically, you should not send more than three signals. If the Terraform does not respond to `interrupt` signals, you can always use `force-stop`. If `force-stop` does not respond due to some issue in the job, you can always `terminate` the job to block.
{: important}

## Canceling a job
{: #cancelling}

If the job is in a `pending` state, any type of stop signal causes the job to cancel. The `Cancel` button shows up if the job is in a `pending` state, when it can be simply cancel. Cancel removes the job from the pending queue. If the `plan`, `apply`, or `destroy` execution is started in the meanwhile, this end up become an interrupt signal to the Terraform execution.

## Stopping a running job through UI
{: #stop-job-ui}
{: ui}

You can follow these steps to stop the {{site.data.keyword.bpshort}} Workspaces running job by using {{site.data.keyword.cloud_notm}} console.

1. From the [{{site.data.keyword.bpshort}} Workspaces dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to  the running job.
   
   You can stop or cancel the running job during a plan, an apply, or a destroy execution.
   {: note}

2. Click **Job** tab to view **Interrupt**, **Force stop**, **Terminate**, and **Cancel** button. 
     
     | Button | Description |
     | --- | --- |
     | Interrupt  | Removes the job from the pending queue, if it is in pending state. Otherwise, sends an interrupt signal to the Terraform command. |
     | Force stop | Sends a kill signal to the Terraform command running. |
     | Terminate | Terminates the job from backend and marks the job as STOPPED and unlocks the workspace. The {{site.data.keyword.bpshort}} saves the log and the `statefile` to the backend periodically while the job is running.|
     | Cancel | The `Cancel` button shows up if the job is in `pending` state, when it can be simply cancel. Cancel removes the job from the pending queue. If the `plan`, `apply`, or `destroy` execution is started in the meanwhile, this end up become an interrupt signal to the Terraform execution.|
     {: caption="Stop job options" caption-side="bottom"}

3. Type your `<option>` name in **Type option to confirm** text box.
4. Click **Confirm option** button.

## Stopping a running job through CLI
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
| `--interrupt` | Optional | Removes the job from the pending queue, if it is in pending state. Otherwise, sends an interrupt signal to the Terraform command.|
| `--force-stop` or `--fs` | Optional | Sends a kill signal to the Terraform execution in the engine attempting to immediately stop the execution. |
| `--terminate` or `-t` | Optional | Abruptly kills the engine, marks the job as stopped, and unlocks your workspace. Data is not saved using this flag. |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive mode. |
{: caption="{{site.data.keyword.bpshort}} job stop flags" caption-side="bottom"}

Example

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --force-stop --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --interrupt --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --terminate --job-id <JOB_ID>
```
{: pre}

## Stopping a running job through API
{: #stop-job-api}
{: api}

You can use following CURL commands to stop a running job for {{site.data.keyword.bplong_notm}} Workspace. 
{: shortdesc}

### Syntax to stop running jobs
{: #stop-jobs-api}

1. [Set up your REST client](/docs/schematics?topic=schematics-setup-api&interface=api#cs_api) to execute {{site.data.keyword.bpshort}} API.
2. Use the syntax and example to interrupt the running job.

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
   curl -X DELETE https://schematics.cloud.ibm.com/v1/workspaces/<wks_id>/actions/{job_id}?signal=force-stop -H "Authorization: <iam_token>"
   ```
   {: codeblock}

   For more information about stopping the running job, see [Stop and delete the running Job](/apidocs/schematics/schematics#delete-workspace-activity) API.
   {: note}


