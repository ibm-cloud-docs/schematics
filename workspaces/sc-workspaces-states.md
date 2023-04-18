---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-18"

keywords: schematics workspaces, schematics workspace vs github repo, schematics workspace access, schematics freeze workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Workspace operational states
{: #wks-state}

## Workspace state overview
{: #states-overview}

Review the states that a workspace can have in the following table. You might not see all states in the {{site.data.keyword.cloud_notm}} console. Some states are only visible when using the command-line or API.
{: shortdesc}

| State | Description |
| ----- | ----- |
| `Active` | After you successfully ran your infrastructure code with {{site.data.keyword.bplong_notm}} by applying your Terraform execution plan, the state of your workspace changes to **Active**. |
| `Connecting` | {{site.data.keyword.bpshort}} tries to connect to the template in your source repo. If successfully connected, the template is downloaded and metadata, such as input parameters, is extracted. After the template is downloaded, the state of the workspace changes to **Scanning**. |
| `Draft` | The workspace is created without a reference to a `GitHub`, `GitLab`, or `Bitbucket` repository.  |
| `Failed` | If errors occur during the execution of your infrastructure code in {{site.data.keyword.bplong_notm}}, your workspace state is set to **Failed**. To troubleshoot errors, open the logs on the workspace **Activity** page. |
| `Inactive` | The {{site.data.keyword.bpshort}} template was scanned successfully and the workspace creation is complete. You can now start running {{site.data.keyword.bpshort}} plan and apply job to provision the {{site.data.keyword.cloud_notm}} resources that you specified in your template. If you have an **Active** workspace and decide to remove all your resources, your workspace is set to **Inactive** after all your resources are removed. |
| `Inprogress` | When you instruct {{site.data.keyword.bplong_notm}} to run your infrastructure code by applying your Terraform execution plan, the state of your workspace changes to `Inprogress`. |
| `Scanning` | The download of the {{site.data.keyword.bpshort}} template is complete and vulnerability scanning started. If the scan is successful, the workspace state changes to **Inactive**. If errors in your template are found, the state changes to **Template Error**. |
| `Stopped` | The {{site.data.keyword.bpshort}} plan, apply, or destroy job are stopped manually. |
| `Template_Error` | The {{site.data.keyword.bpshort}} template contains errors and cannot be processed.|
{: caption="Workspace state overview" caption-side="bottom"}

## Workspace state diagram and manipulative job
{: #workspace-state-diagram}

The state of a workspace indicates if you have successfully created a Terraform execution plan and applied to provision your resources in the {{site.data.keyword.cloud_notm}} account. The table represents the state and the workspace job.
{: shortdesc}

| workspace | State diagram | Description |
| -- | -- | --|
| `Create workspace`| ![Create workspace state](../images/createworkspace.png) | The workspace is created without a reference to `GitHub`, `GitLab`, or `Bitbucket` to the draft state. From the draft state you can connect to the infrastructure template in your source repository. From connecting state, the template is processed successfully to reach Inactive state (Final state) or template parsing may fail and reach failed state. From inactive state, when you do an apply, and if it results in one resource then, state enters active state and if they destroy, state enters destroy state. you can maintain at least one resource in the state file by apply job, to move the workspace into active state. The {{site.data.keyword.bpshort}} [stores](/docs/schematics?topic=schematics-general-faq#persist-file) the user-defined file for running the subsequent Terraform commands. Then, you can destroy all the resources to make your workspace in an inactive state. |
|`Delete workspace` | ![Delete workspace state](../images/deleteworkspace.png) | When you perform delete workspace on an inactive, active or failed state. From these state, the template is parsed successfully to reach an inactive state or template parsed can fail and reach failed state. If you delete at least one resource, the plan and apply job executes to destroy the resource from the active state. |
| `Plan and apply job` | ![Plan and apply action state](../images/applyplan.png) | When you perform the plan or apply job on active, inactive, and failed state. Your workspace is in in progress and locked state. And the job is performed, if it is success, your workspace is in active state, if it contains at least one resource, your workspace is in an inactive state, on failure workspace is in failed state. The {{site.data.keyword.bpshort}} [stores](/docs/schematics?topic=schematics-general-faq#persist-file) the user-defined file for running the subsequent Terraform commands. |
| `Destroy job` | ![Destroy action state](../images/destroyworkspace.png) | The destroy job performs when your workspace is in an inactive, active or failed state. From these state, the destroy job connects to parse the template from your source repository and workspace gets into in progress unlocked state. From   state if you destroy, resource reaches failed state.|
{: caption="Workspace state diagram" caption-side="bottom"}

## Creating an auto deployment to the {{site.data.keyword.bplong_notm}}
{: #create-deploy-to-schematics}

{{site.data.keyword.bplong_notm}} now supports an efficient way to share your Git repository in a cloned copy of the code in a new Git repository to deploy to {{site.data.keyword.cloud_notm}} without affecting your original code. For more information about deploy to {{site.data.keyword.cloud_notm}}, see [automating the deployment to the {{site.data.keyword.bpshort}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-create_deploy_to_schematics).


## Reviewing the {{site.data.keyword.bpshort}} job details
{: #job-logs}

Use the {{site.data.keyword.bpshort}} job page in the console to find the history of all {{site.data.keyword.bpshort}} activities, such as downloading your `template`, `plan`, `apply`, and to see the logs of the jobs. The jobs are created when you run your templates. You can also see the count of the resources that are in `plan`, or `apply` jobs that are in **added**, **modified**, or **destroyed** status. For more information about job queue process, see [Execution process of the {{site.data.keyword.bpshort}} job queue](/docs/schematics?topic=schematics-job-queue-process#about-job-queue).

In the job log you can see a message such as: 

- **Activity triggered. Waiting for the logs**. This means the job is in pending status and yet to be processed. 

- **Your job was submitted and is in queue, at position x out of y**. Here `x` is the position of your job in the pending queue and `y` is a total pending jobs. The available resources in {{site.data.keyword.bpshort}} backend are equally distributed to the pending jobs. In case you are running a huge number of jobs, you can view the position increase along with the total.
