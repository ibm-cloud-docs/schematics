---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-12"

keywords: schematics drifting, drift, infrastructure as code, schematics workspace drift

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Detecting drift in {{site.data.keyword.bpshort}}
{: #drift-note}

{{site.data.keyword.bplong}} is the {{site.data.keyword.cloud_notm}} automation tool that enables users to deploy, manage, and manipulate infrastructure resources with Terraform based Workspaces using well known `declarative` Infrastructure as Code (IaC) concepts. However, when a Terraform config is deployed and resources created, it does not mean that the resources stay as declared by the config. Any change in the infrastructure state is called `drift`. It occurs when the configuration of your deployed infrastructure differs from the desired state that is defined in your template configuration. 

Drift can occur for many reasons. The most frequent cause is changes applied manually outside of Terraform automation. The Terraform state file of your deployed workspace is no longer synchronized with your deployed infrastructure resources, and the workspace is said to have drifted.
{: shortdesc}

Drift can happen for many reasons within the context of your configuration:
- Adding, or removing resources from the Template configuration without applying the changes. 
- Changing template resource definitions. 
- External to your template configuration, drift occurs when changes are made manually. For example, from a command-line operation on a Cloud resource, or change via the Cloud console. 
- Can occur through other automation tools.

## Example drift scenario
{: #drift-scenario}

A VSI instance is provisioned using {{site.data.keyword.bplong_notm}} and a configuration template. A DevOps cloud user can modify the provisioned VSI configuration by logging in to the Cloud console and modifying the boot volume of an instance or adding an Ethernet interface. These changes result in your infrastructure deployment having `drifted`.

{{site.data.keyword.bplong_notm}} enables you to predictably [manage the resource lifecycle](/docs/schematics?topic=schematics-manage-lifecycle) of your infrastructure by using Terraform. Drift occurs when the real-world state of your infrastructure differs from the state that is defined in your Terraform template configuration. 

Terraform cannot detect drift in resources and attributes that are not managed or configured using Terraform. For example, Terraform cannot not detect changes in a virtual machine that results from installing applications locally or by using configuration management tools like `Chef` or `Ansible`.

## Drift detection in {{site.data.keyword.cloud_notm}}
{: #drift-in-ibm}

Drift detection for your Terraform automation Workspaces is possible in {{site.data.keyword.bplong_notm}}. You can use following three methods to check the drift detection.

## Drift detection through {{site.data.keyword.bpshort}} UI
{: #drift-ui}
{: ui}

You can initiate drift detection for Workspaces from the {{site.data.keyword.bpshort}} Workspaces job page. This initiates a job to detect drift for the workspace and its deployed resources. During execution the drift detection job will `in progress`, on completion it will have a of `failure` or `success`. To review the details of the drift job, you need to check the drift job log for the drift status.

### Viewing detect drift through UI
{: #drift-view-ui}

Use the following steps to view the drift job log.

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want check for drift. 
2. Select and open your workspace.
3. Click **Actions** tab.
4. Select **Detect drift** option to initiate the detect drift job. 
5. During execution the status will show `in progress` moving to a `success` or a job `failure` status on completion. 
6. The drift status can only be determined by reviewing the output of the job log on when the job is in a `success` state.  
  A sample success job executation with detected drift is shown in the screen capture.

    Review the success job log to identify the drift details.
    ```text
    2022/04/19 10:10:44 -----  Terraform DRIFT  -----
    2022/04/19 10:10:44 Starting command: terraform-drift-cli drift
    2022/04/19 10:10:44 Terraform Drift | configuration drift identfied
    2022/04/19 10:10:44 Terraform Drift | resource         operation   attribute   drift value
    2022/04/19 10:10:44 Terraform Drift | ibm_is_vpc.vpc   +           tags        schematics:us-east.workspace.myworkspace-drift-demo.bfdd0e2d
    2022/04/19 10:10:44 Terraform Drift | ibm_is_vpc.vpc   +           tags        tag:new1
    2022/04/19 10:10:44 Terraform Drift | ibm_is_vpc.vpc   +           tags        tag:new2
    2022/04/19 10:10:44 Terraform Drift |
    2022/04/19 10:10:44 Command finished successfully.
    2022/04/19 10:10:52 Done with the workspace action
    ```
    {: screen}

  If the job fails, review the cause of the failure in the log and correct the error condition before rerunning the job. 

    A failure example job log identifing the cause of the drift job failure. 
    ```text
    2022/04/13 13:05:46 -----  Terraform Commands  -----
    2022/04/13 13:05:46 Could not execute job: Error : Drift cannot be executed since state file doesn't exist. Please run terraform apply to generate state file.
    ```
    {: screen}

## Drift detection through {{site.data.keyword.bpshort}} CLI
{: #drift-cli}
{: cli}

You can initiate detecting drift from the create Workspaces command line. This initiate a job to detect drift for the workspace and its specific resources. The drift detection job is `in progress` or `completed` with the appropriate status such as `failure` or `success`. Instead to know the details of the drift job, you need to check the drift job log for the drift status. Use the following commands to view the detect drift.
{: shortdesc}

### Creating and viewing the detect drift through CLI
{: #drift-view-cli}
{: cli}

You can follow these steps to detect the drift in {{site.data.keyword.bpshort}} Workspaces through command line.
{: shortdesc}

1. [Create the {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).
2. [Get your workspace ID](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get).
3. Execute the [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan).
4. Fetch the [`ibmcloud schematics job logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).
5. Execute the [`ibmcloud schematics apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply).
6. Execute the [`ibmcloud schematics job run`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-run-job) to create a job in {{site.data.keyword.bpshort}} workspace.

    ```sh
    ibmcloud schematics job run --command-object workspace --command-object-id <workspace_id> --command-name drift
    ```
    {: pre}

7. Execute the [`ibmcloud schematics workspace action`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-action) to retrieve all activities of your workspace.

    ```sh
    ibmcloud schematics workspace action --id <workspace_id>
    ```
    {: pre}

8. Execute the [`ibmcloud schematics job logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job) to retrieve the detailed logs of a job to view the drift details.

    ```sh
    ibmcloud schematics logs --id <workspace_id> --act-id <Job_id>
    ```
    {: pre}

## Creating and viewing the detect drift through API 
{: #drift-api}
{: api}

Review the CURL commands to create and view the drift through API.
{: shortdesc}

1. Retrieve your IAM access [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to authenticate with the {{site.data.keyword.bplong_notm}}.
2. [Create the workspace](/apidocs/schematics/schematics#create-workspace). As part of the payload you need to add the following drift configuration.

    ```terraform
    {
        commands: [
            {
            command: 'drift',
            command_name: 'drift command',
            command_desc: 'command to detect drift in workspace resources',
            },
        ],
        operation_name: 'drift',
        description: 'command to detect drift in workspace resources',
        }
    ```
    {: codeblock}

3. [Get the workspace details](/apidocs/schematics/schematics#get-workspace).
4. Execute the [{{site.data.keyword.bpshort}} job plan](/apidocs/schematics/schematics#plan-workspace-command).
5. Fetch the [`ibmcloud schematics job logs`](/apidocs/schematics/schematics#get-template-activity-log).
6. Execute the [`ibmcloud schematics apply`](/apidocs/schematics/schematics#apply-workspace-command).
7. Execute the [`ibmcloud schematics job run`](/apidocs/schematics/schematics#run-workspace-commands) to create a job in {{site.data.keyword.bpshort}} workspace.
8. Execute the [`ibmcloud schematics workspace action`](/apidocs/schematics/schematics#create-job) to retrieve all activities of your workspace.
9. Fetch the [`ibmcloud schematics job logs`](/apidocs/schematics/schematics#get-template-activity-log).
