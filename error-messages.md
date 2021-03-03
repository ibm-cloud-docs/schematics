---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-03"

keywords: error message, message id, ansible error code, schematics error code

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}


# Troubleshooting errors
{: #handling-error}

When Schematics actions or workspace receives a non-zero return code from a command, API, UI or failed during setting up the infrastructure, by default the setup halts executing. Schematics provides resolution to handle such errors and help you get the expected behavior, output.
{: shortdesc}

This is not the complete list of error messages. Some messages are created by other systems, such as IAM authentication errors messages to the Schematics action.
{: note}

## Action error messages
{: #action-errmsg}

The error messages are categorized based on the error type, error code, and error messages. The following four categories specify the type of error that you can receive. The error message indicates the reason for an error to ease to resolve.
{: shortdesc}

- [Parameter error](#param-error)
- [Service error](#svc-error)
- [State error](#state-error)
- [Job error](#job-error)

### Parameter error
{: #param-error}

The parameter error represents that all the required or optional Schematics action parameters has configured with the right input.
{: shortdesc}

#### Message
{: #param-msg}

- Action name cannot be empty.
- Action name should contain maximum of 65 character. {{.Count}} character found.
- Action name should be unique. {{.Name}} already exists.
- Bad Request. Error occurred while processing the {{.Method}} request.
- Action ID is correct.
- The requested action cannot be located. Check that the action ID is correct and try your request again.

#### Resolve
{: #param-resolve}

You should read the error message carefully to rectify. You can check out the required parameter is unique, has rightly provided in the payload, and you need to follow the maximum and minimum character limit to process your parameter value.

###  Service error
{: #svc-error}

The Schematics action instance and resources are overloaded or down for maintenance. 

#### Message
{: #svc-msg}

- Internal Service Error occurred during {{.Method}} request, Please wait a few minutes and try again. If you still encounter this problem contact the IBM Cloud support.

#### Resolve
{: #svc-resolve}

- Check the action logs. For more information, refer to [log commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job)
- Wait for few minutes and retry your action processing.
- Check you have the required permissions such as [IAM access or key](/docs/schematics?topic=schematics-action-setup).
- Check the firewall IP are enabled.
- Check if your [{{site.data.keyword.cloud_notm}} environment notification](/docs/get-support?topic=get-support-viewing-notifications){: external} is in maintenance.


### State error
{: #state-error}

While processing the create, delete, update action, the {{site.data.keyword.bpshort}} will run the Jobs are sent to pending state. The state flow and error message depends based on the template administrator. Following are the messages you can get in the process flow.

#### Message
{: #state-msg}

- Action is ready, Job cannot be executed. Check the Job status.
- Action is in queue. Wait until the action completes.
- Action is in queue. Wait until the action completes update.
- Action failed to complete. Please wait few minutes and try again.
- Action is ready for execution.
- Action is ready for execution. The configuration and settings cannot be edited.
- Action is in draft. Please add a Git repository.
- Action is ready, Job cannot be executed. Check the Job status.
- Action state is {{.State}} and could not process the request
- The requested object cannot be located. Check that the ID is correct and try your request again.
- Playbook not selected to create a Job. Please select a playbook name.
- Unable to delete the action. Action current status: {{.Status}}.

#### Resolve
{: #state-resolve}

- Refer the [{{site.data.keyword.bpshort}} action state diagram](/docs/schematics?topic=schematics-action-setup#action-state-diagram) to understand the {{site.data.keyword.bpshort}} action and job flow.
- Check the logs. For more information, refer to [log commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job)

### Job error
{: #job-error}

The Schematics jobs displays a list of jobs and their state as successful, or failed, or active (running) job.

#### Message
{: #state-jobmsg}

- Job processing error.
- Unable to delete the job. Job current status: {{.Status}}
- The Job was temporarily locked by {{.User}} at {{.Time}}. Please wait until any current Job execution have completed.
- Playbook not selected to create a Job. Please select a playbook name.

#### Resolve
{: #state-jobresolve}

- Check the input configuration and settings.
- Check the job configuration timeout, if the job timeout is reached 60 minutes, the status is updated and error is returned.
- For more information, about the {{site.data.keyword.bpshort}} action state diagram, see [State diagram flow](/docs/schematics?topic=schematics-action-setup#action-state-diagram).


You can also contact IBM support by opening a case or post a message in the Slack channel. To learn about opening an IBM support case, or about Slack channel, see [Getting help and support](/docs/schematics?topic=schematics-schematics-help).
{: note}

