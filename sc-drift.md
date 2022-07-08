---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-18"

keywords: schematics drifting, drift, infrastructure as code, schematics workspace drift

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Detecting drift in {{site.data.keyword.bpshort}}
{: #drift-note}

{{site.data.keyword.bplong}} is the {{site.data.keyword.cloud_notm}} automation tool enables to deploy, manage, and manipulate infrastructure resources with Terraform based Workspaces by using the well known `declarative` Infrastructure as Code (IaC) concept. However, if Terraform is deployed as declared, does not mean that Terraform stays as declared. This change in the infrastructure state is called `drift`. When the state of your infrastructure differs from the state that is defined in your template configuration. Then, the state file of your deployed workspace is not synchronized with your infrastructure resources that are deployed, such workspace is said to be drifted.
{: shortdesc}

Drift can happen for many reasons within the context of your configuration:
- When add, or remove resources. 
- When change resource definitions. 
- External to your template configuration, drift occurs when changes are made manually. For example, from a command-line operation on a Cloud resources, or manual modification of the resources from a Cloud console. 
- Can occur through other automation tools.

## Example drift scenario
{: #drift-scenario}

A VSI instance is provisioned by using {{site.data.keyword.bplong_notm}} and wanted configuration templates. A DevOps cloud user can modify the provisioned VSI configuration by logging in to the Cloud console and modifying the boot volume of an instance or adding Ethernet interface. By doing that, your infrastructure deployment gets `drifted`.

{{site.data.keyword.bplong_notm}} enables safe and predictably [manage the resource lifecycle](/docs/schematics?topic=schematics-manage-lifecycle) of your infrastructure by using Terraform. Drift occurs when the real-world state of your infrastructure differs from the state that is defined in your Terraform template configuration. 

Terraform cannot detect drift of resources and their associated attributes that are not managed by using Terraform. For example, Terraform do not detect changes in a virtual machine that results of installing applications locally or by using a configuration management tool like `Chef` or `Ansible`.

## Drift detection in {{site.data.keyword.cloud_notm}}
{: #drift-in-ibm}

Drift detection for your Terraform automation Workspaces is possible in {{site.data.keyword.bplong_notm}}. You can use following three methods to check the drift detection.

## Drift detection through {{site.data.keyword.bpshort}} UI
{: #drift-ui}
{: ui}

You can initiate detecting drift for Workspaces from the {{site.data.keyword.bpshort}} Workspaces job page. This event initiates a job to detect drift for the workspace and its specific resources. The drift detection job is `in progress` or `completed` with the appropriate status such as `failure` or `success`. In order to know the details of the drift job, you need to check the drift job log for the drift status.

### Viewing detect drift through UI
{: #drift-view-ui}

Use the following steps to view the detect drift.

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to view detect drift.
2. Select and open your workspace.
3. Click **Actions** tab.
4. Select **Detect drift** option to view the drift job is in `completed` with a `success` status or `in progress` with a `failure` status. The sample success example and failure example job log are shown in the screen capture.

    The success example job log in order to identify the drift details.
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

    The failure example job log in order to identify the drift details.
    ```text
    2022/04/13 13:05:46 -----  Terraform Commands  -----
    2022/04/13 13:05:46 Could not execute job: Error : Drift cannot be executed since state file doesn't exist. Please run terraform apply to generate state file.
    ```
    {: screen}

## Drift detection through {{site.data.keyword.bpshort}} CLI
{: #drift-cli}
{: cli}

You can initiate detecting drift from the create Workspaces command line. This initiate a job to detect drift for the workspace and its specific resources. The drift detection job is `in progress` or `completed` with the appropriate status such as `failure` or `success`. In order to know the details of the drift job, you need to check the drift job log for the drift status. Use the following commands to view the detect drift.
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
