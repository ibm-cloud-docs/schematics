---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-04"

keywords: schematics, automation, terraform

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning Terraform template by using {{site.data.keyword.bpfull_notm}}
{: #provision-terraform-template}

[{{site.data.keyword.bpfull_notm}}](/docs/schematics?topic=schematics-about-schematics) enables Infrastructure as a Code(IaC) to code the {{site.data.keyword.cloud}} resources with Terraform and use {{site.data.keyword.bpfull_notm}} workspace to automate the provisioning and management of your resources.
{: shortdesc}

[Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about) enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} platform, classic infrastructure, and VPC infrastructure resources by using a high-level scripting language. You can use Terraform to automate your {{site.data.keyword.cloud_notm}} resource provisioning and enable Infrastructure as a code (IaC)
{: shortdesc}

[Terraform template](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples) consists of one or more Terraform configuration files that declares the state you want to achieve for your {{site.data.keyword.cloud_notm}} resources. To successfully work with your resources, you need to configure IBM as your cloud provider and add resources to your Terraform configuration file.
{: shortdesc}

## Provisioning Terraform template by using {{site.data.keyword.bpfull_notm}} UI
{: ui-provisioning}

To provision Terraform template, perform these steps:

### Creating the {{site.data.keyword.bpfull_notm}} workspace
{: create-wks}

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, click Navigation menu
3. Select **Schematics** option.
4. Click **Create workspace** button.
5. Enter the values for **Workspace name**, **Tags**, **Resource group**, **Location**, and **Description** parameters.
6. Click **Create** button.

### Uploading Terraform template from a GitHub repository
{: upload-template}

1. Login to your [GitHub](https://github.com/) account.
2. Navigate to one of the [Terraform sample templates repository](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket) by using the sample Terraform templates.
3. From the workspace settings page, scroll to view the **Import from Terraform template** panel.
4. From the Import your Terraform template panel, insert the URL of the GitHub or GitLab repository that hosts your Terraform configuration files.
5. Select **Terraform version** from the drop down list.
6. Click **Save Template information** to view the **Variables** panel.
    This will load a page with the configuration variables and its values. Values of the variables can be edited, so that a new Terraform template can be generated or applied.
    {: note}

7. Provide the values for the variables as described in the table.
    <table>
	<tr>
		<th>Name</th><th style="width:150px">Value</th>
	</tr>
	<tr>
        <td><code>iaas_classic_api_key</code></td>
        <td>Enter the API key to access {{site.data.keyword.cloud_notm}} classic infrastructure. For more information for how to create an API key and retrieve it, refer to [Managing classic infrastructure API keys](/docs/account?topic=account-classic_keys).</td></tr>
	<tr><td><code>iaas_classic_username</code></td><td>Enter the username to access {{site.data.keyword.cloud_notm}} classic infrastructure. To get the details including the username of a classic infrastructure API key after you create it, go to Manage > Access (IAM) > Users, then select the user's name.</td></tr>
        <tr>
        <td><code>ibm cloud_api_key</code></td>
        <td>Enter your {{site.data.keyword.cloud_notm}} API Key, refer to [{{site.data.keyword.cloud_notm}} API key](https://cloud.ibm.com/iam#/apikeys) for details.</td></tr>
        <tr>
        <td><code>resource_group_name</code></td>
        <td>The default value is <strong>Default</strong>.</td>
        </tr>
    </table>
8. Click **Save Changes**.


### Executing the plan and apply actions
{: execute-action}

1. From the workspace page,  click **Apply Plan**, to create execution plan for your configuration files.
2. Click Activity option to observe the activities performed on your workspace.
    While an activity is executing, you can click **Stop** to edit any configuration for any changes.
    {: note}

    The workspace will pass through multiple state such as Inactive, In progress, Failed, and Active.
    {: note}

### Viewing the configured file and analyzing the logs
{: view-confg-file}

1. Click **View log** from the plan status panel to view the Terraform provisioning process during the execution plan.
2. Click **Resources** from the workspace page to view the configuration files.

This completes the end to end flow to provision the Terraform template by using {{site.data.keyword.bpfull_notm}} UI.

## Provisioning Terraform template by using {{site.data.keyword.bpfull_notm}} CLI
{: cli-provisioning}

To provision a Terraform template using Schematics CLI, you need perform the given steps: 
{: shortdesc}

### Prerequisites
{: prov-prereq}

1. Install {{site.data.keyword.cloud_notm}} command-line and Schematics plug-ins, refer to [Setting up the command-line and Schematics](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli) for details.
2. Check an update is available for the {{site.data.keyword.bplong_notm}} command-line plug-in. If an update is available, you find an **Update available** notification in your command-line output.

    ```
    ibmcloud plugin list | grep schematics
    ```
    {: pre}

    Example output:

    ```
    schematics                      1.4.1        Update available
    ```
    {: screen}

### Creating Workspace
{: cli_create_wks}

Create an {{site.data.keyword.bplong_notm}} workspace that points to your Terraform template in GitHub or GitLab. If you want to provide your Terraform template by uploading a tape archive file (`.tar`), you can create the workspace without a connection to a GitHub repository and then use the [ibmcloud schematics workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command to provide the template. 
{: shortdesc}

To create a workspace, you must specify your workspace settings in a JSON file. Then provide the relative path to a JSON file on your local machine that is used to configure your workspace. Refer to[Schematics-workspace-new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) for details.
{: note}

Example JSON by using a GitHub or GitLab repository:
<pre class="codeblock">
<code>{
    "name": "&lt;workspace_name&gt;",
    "type": [
        "&lt;terraform_version&gt;"
    ],
    "location": "&lt;location&gt;",
    "description": "&lt;workspace_description&gt;",
    "tags": [],
    "template_repo": {
        "url": "&lt;GitHub_source_repo_url&gt;"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "&lt;terraform_version&gt;",
        "variablestore": [
        {
          "name": "&lt;variable_name1&gt;",
          "value": "&lt;variable_value1&gt;",
          "type": "&lt;variable_type1&gt;",
          "secure": true
	  "description":"&ltdescription&gt"
        },
        {
          "name": "&lt;variable_name2&gt;",
          "value": "&lt;variable_value2&gt;",
          "type": "&lt;variable_type2&gt;",
          "secure": false
	  "description":"&ltdescription&gt"
        }
        ]
    }
    ],
    "githubtoken": "&lt;github_personal_access_token&gt;"
}
</code></pre>

**Syntax**

```
ibmcloud schematics workspace new --file FILE_PATH [--state STATE_FILE_PATH] [--json]
```
{: pre}

**Example**

```
ibmcloud schematics workspace new --file myfile.json
```
{: pre}

Example output for creating the workspace.
<pre class="codeblock">
<code>
Creation Time   Tue Jul 28 13:00:02
Description     terraform workspace
Frozen          true
Frozen By       user@.ibm.com
Frozen At       Tue Jul 28 13:00:01
ID              test-6569f475-b61c-4a
Name            test
Status          DRAFT

Template Variables for: examples-e82a61a6-695c-44
Name          Value
sample_var    THIS IS AN IBM CLOUD TERRAFORM CLI DEMO
sleepy_time   10195

OK
</code></pre>

### Executing Plan and Apply
{: cli-plan-apply}

Execute the plan command to view the activity details of your workspace.
{: shordesc}

**Syntax:**

```
ibmcloud schematics plan --id <WORK_SPACE_ID>
```
{: pre}

Execute the apply command to view the activity details of your workspace.

**Syntax:**

```
ibmcloud schematics apply --id <WORK_SPACE_ID>
```
{: pre}

### Analyzing logs and activities
{: cli-logs}

Retrieve the Terraform log file for your workspace to troubleshoot provisioning issues if any.

```
ibmcloud schematics logs --id <WORK_SPACE_ID>
```
{: pre}

### Fetching state files and output values
{: cli-output}

Fetch the list of {{site.data.keyword.cloud_notm}} resources that are documented in your Terraform state file for a specific Terraform template in your workspace.

The state files are generated only after the plan and apply commands are successful.
{: note}

**Syntax:**

```
ibmcloud schematics state list --id <WORK_SPACE_ID>
```
{: pre}

To show the content of the Terraform state file for a specific Terraform template in your workspace

**Syntax:**

```
ibmcloud schematics state pull --id <WORKSPACE_ID> --template <TEMPLATE_ID>
```
{: pre}

To retrieve a list of Terraform output values. You define output values in your Terraform template to include information that you want to make accessible for other Terraform templates.

**Syntax:**

```
ibmcloud schematics  output --id WORKSPACE_ID
```
{: pre}

This completes the end to end flow to provision the Terraform template by using {{site.data.keyword.bpfull_notm}} CLI.



