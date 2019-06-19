---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-19"

keywords: Schematics, automation, Terraform

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# CLI command reference
{: #schematics-cli-reference}

Refer to these commands when you want to automate the provisioning of {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

To install the CLI, see [Setting up the CLI](). 

## ibmcloud terraform commands
{: commands}

View a list of commands that you can run. 
{: shortdesc}

<table id="manage_workspace" summary="Manage your workspace with ibmcloud terraform workspace commands.">
<caption>Table 1. Available commands to manage your workspace. 
</caption>
 <thead>
 <th colspan="5">Managing your environment</th>
 </thead>
 <tbody>
 <tr>
 <td>[ibmcloud terraform workspace new](#workspace-new)</td>
 <td>[ibmcloud terraform workspace delete](#workspace-delete)</td>
 <td>[ibmcloud terraform workspace list](#workspace-list)</td>
 <td>[ibmcloud terraform workspace show](#workspace-show)</td>
 <td>[ibmcloud terraform workspace update](#workspace-update)</td>
 <td>[ibmcloud terraform workspace statefile](#workspace-statefile)</td>
 </tr>
</tbody></table>

 <table id="update_resources" summary="Update the resources provisioned by your workspace with ibmcloud terraform action commands.">
 <caption>Table 2. Available commands to update resources in your workspace. 
 </caption>
  <thead>
  <th colspan="5">Updating your resources</th>
  </thead>
  <tbody>
  <td>[ibmcloud terraform action apply](#action-apply)</td>
  <td>[ibmcloud terraform action destroy](#action-destroy)</td>
  <td>[ibmcloud terraform action plan](#action-plan)</td>
  </tr></tbody></table>

  <table id="audit_workspace" summary="Auditing activities that ran against your workspace with ibmcloud terraform activity commands.">
  <caption>Table 3. Available commands to audit the activities in your workspace. 
  </caption>
   <thead>
   <th colspan="5">Auditing your environment</th>
   </thead>
   <tbody>
   <td>[ibmcloud terraform logs show-all](#activity-list)</td>
   <td>[ibmcloud terraform logs activity-log](#activity-log)</td>
   <td>[ibmcloud terraform logs activity show](#activity-show)</td>
   </tr></tbody></table>
   
## Workspace commands
{: #schematics-workspace-commands}

### `ibmcloud terraform workspace new`
{: #workspace-new}

Create a workspace in {{site.data.keyword.bplong_notm}} from your configuration. When you create a workspace, a workspace ID is returned.
{: shortdesc}

```
ibmcloud terraform workspace new --file FILE_NAME [--json]
```
{: pre}

#### Command options
{: #workspace-new-options}

<dl>
<dt>`--file FILE_NAME`, `-f FILE_NAME`</dt>
<dd>Required. The relative path to a JSON file on your local machine that is used to pass metadata about your workspace, such as where the source Terraform configuration files are stored in source control. 
<br>
<br>Example JSON with all available values:
<pre class="codeblock">
<code>
{
  "description": "Optional description of the workspace",
  "frozen": false,
  "name": "Name of the workspace",
  "sourceurl": "The URL for the source control repository where your Terraform configuration is stored",
  "tags": [
    "optional_tag_1",
    "optional_tag_2"
  ],
  "variablestore": [
    {
      "name": "visible_string_variable",
      "secure": false,
      "value": "Visible value"
    },
    {
      "name": "secret_string_variable",
      "secure": true,
      "value": "Secured value"
    },
    {
      "name": "visible_map_variable",
      "secure": false,
      "type": "map",
      "value": "{ \"key_1\" = \"value_1\", \"key_2\" = \"value_2\", \"key_3\" = \"value_3\" }"
    },
    {
      "name": "visible_list_variable",
      "secure": false,
      "type": "list",
      "value": "[ \"value_1\", \"value_2\", \"value_3\" ]"
    }
  ]
}
</code></pre></dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Print the CLI output in JSON format.</dd>
</dl>

#### Example
{: #workspace-new-example}
```
ibmcloud terraform workspace new --file configuration.json
```
{: pre}

### `ibmcloud terraform workspace delete`
{: #workspace-delete}

Remove your workspace configuration from {{site.data.keyword.bplong_notm}}. Use this command only if you have a workspace that does not include any running {{site.data.keyword.cloud_notm}} resources. If you delete a workspace that has running {{site.data.keyword.cloud_notm}} resources, you cannot manage your resources with {{site.data.keyword.bplong_notm}} anymore. Instead, you must manually manage each resource separately from the {{site.data.keyword.cloud_notm}} console. 
{: shortdesc}

```
ibmcloud terraform workspace delete --id WORKSPACE_ID [--force]
```
{: pre}

#### Command options
{: #workspace-delete-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve your workspace ID, run <code>ibmcloud terraform workspace list</code>. </dd>
<dt>--force, -f</dt>
<dd>Optional. Force the deletion of a workspace without CLI prompts.</dd>
</dl>

#### Example
{: #workspace-delete-example}
```
ibmcloud terraform workspace delete --id 123456
```
{: pre}

### `ibmcloud terraform workspace list`
{: #workspace-list}

List all workspaces in your {{site.data.keyword.cloud_notm}} account.

```
ibmcloud terraform workspace list [--limit VALUE] [--offset VALUE] [--json]
```
{: pre}

#### Command options
{: #workspace-list-options}

<dl>
<dt>`--limit VALUE`, `-l VALUE`</dt>
<dd>Optional. The maximum number of workspaces that you want to see in your CLI return. </dd>
<dt>`--offset VALUE`, `-m VALUE`</dt>
<dd>Optional. The offset in the list of workspaces.</dd>
<dt>--json, -j</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #workspace-list-example}
```
ibmcloud terraform workspace list --limit 3 --offset 4 --json
```
{: pre}

### `ibmcloud terraform workspace show`
{: #workspace-show}

Retrieve the details of an existing workspace.
{: shortdesc}

```
ibmcloud terraform workspace show --id WORKSPACE_ID [--json]
```
{: pre}

#### Command options
{: #workspace-show-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #workspace-show-example}

```
ibmcloud terraform workspace show --id 123456 --json
```
{: pre}

### `ibmcloud terraform workspace update`
{: #workspace-update}

Update the details for an existing workspace, such as the workspace name or source control URL. To update the number of resources that are allocated in the workspace, see [`ibmcloud terraform action plan`](#action-plan).
{: shortdesc}

```
ibmcloud terraform workspace update --id WORKSPACE_ID --file FILE_NAME [--json]
```
{: pre}

#### Command options
{: #workspace-update-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--file FILE_NAME`, `-f FILE_NAME`</dt>
<dd>Required. The relative path to the JSON file that is used to pass details about your workspace.<br>
<br>Example JSON with all available values:
<pre class="codeblock">
<code>
{
  "description": "Optional description of the workspace",
  "frozen": false,
  "name": "Name of the environment",
  "sourceurl": "The URL for the source control repository where your Terraform configuration is stored",
  "tags": [
    "optional_tag_1",
    "optional_tag_2"
  ],
  "variablestore": [
    {
      "name": "visible_string_variable",
      "secure": false,
      "value": "Visible value"
    },
    {
      "name": "secret_string_variable",
      "secure": true,
      "value": "Secured value"
    },
    {
      "name": "visible_map_variable",
      "secure": false,
      "type": "map",
      "value": "{ \"key_1\" = \"value_1\", \"key_2\" = \"value_2\", \"key_3\" = \"value_3\" }"
    },
    {
      "name": "visible_list_variable",
      "secure": false,
      "type": "list",
      "value": "[ \"value_1\", \"value_2\", \"value_3\" ]"
    }
  ]
}
</code></pre></dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #workspace-update-example}

```
ibmcloud terraform workspace --id 123456 --file configuration.json --json
```
{: pre}


### `ibmcloud terraform workspace statefile`
{: #workspace-statefile}

List all {{site.data.keyword.cloud_notm}} resources that you created in your workspace, including the state of each resource. You can also use this command to list the Terraform `tfstate` file. 
{: shortdesc}

```
ibmcloud terraform workspace statefile --id WORKSPACE_ID [--json] [--wide]
```
{: pre}

#### Command options
{: #workspace-statefile-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
<dt>`--wide`, `-w`</dt>
<dd>Optional. Prints the wide format without cutting string values.</dd>
</dl>

#### Example
{: #workspace-statefile-example}

```
ibmcloud terraform workspace statefile --id 123456 --json
```
{: pre}

## Action commands
{: #action-commands}

### `ibmcloud terraform action apply`
{: #action-apply}

Deploy your workspace configuration. The `apply` command scans and executes the Terraform configurations that are stored in your source control repository.

```
ibmcloud terraform action apply --id WORKSPACE_ID [--force] [--json]
```
{: pre}

#### Command options
{: #action-apply-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--force`, `-f`</dt>
<dd>Optional. Force the execution of this command without user prompts. </dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #action-apply-example}

```
ibmcloud terraform action apply --id 123456 --json
```
{: pre}

### `ibmcloud terraform action destroy`
{: #action-destroy}

Remove your workspace and all {{site.data.keyword.cloud_notm}} resources, even if these resources are currently running. Typically, you use this action to remove temporary workspaces, such as workspaces that you created for testing or quality assurance purposes. 
{: shortdesc}

Use this command with caution. After you run the command, you cannot reverse the removal of your {{site.data.keyword.cloud_notm}} resources. If you used persistent storage, make sure that you created a backup for your data. 
{: important} 

```
ibmcloud terraform action destroy --id WORKSPACE_ID [--force] [--json]
```
{: pre}

#### Command options
{: #action-destroy-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--force`</dt>
<dd>Optional. Force the execution of this command without user prompts. </dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>
</dl>

#### Example
{: #action-destroy-example}

```
ibmcloud terraform action destroy --id 123456 --force --json
```
{: pre}

### `ibmcloud terraform action plan`
{: #action-plan}

Scan the Terraform configuration files in your source control repository and compare this configuration against the {{site.data.keyword.cloud_notm}} resources that are already deployed. The CLI output shows the {{site.data.keyword.cloud_notm}} resources that must be added or removed to achieve the state that is described in your configuration file.   
{: shortdesc}

```
ibmcloud terraform action plan --id WORKSPACE_ID [--file FILE_NAME] [--json]
```
{: pre}

#### Command options
{: #action-plan-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--file FILE_NAME`, `-f FILE_NAME`</dt>
<dd>Optional. The optional JSON file that is used to pass parameters for the plan action. You can pass the parameter <code>sourcesha</code> to reference a specific Git branch for the environment's Terraform configuration. The Git branch must be specified as a head reference such as <code>refs/heads/BRANCH_NAME</code>. If no parameter is specified, the default value is the head of the default branch for the repository.
<br>
<br>Example JSON snippet with value:
<pre class="codeblock">
<code>{
  "sourcesha": "refs/heads/BRANCH_NAME"
}</code></pre></dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #action-plan-example}

```
ibmcloud terraform action plan --id 123456 --file configuration.json --json
```
{: pre}

## Activity commands
{: #activity-commands}

### `ibmcloud terraform activity list`
{: #activity-list}

List the Terraform activities that ran against a workspace.
{: shortdesc}

```
ibmcloud terraform activity list --id WORKSPACE_ID [--limit VALUE] [--offset VALUE] [--json]
```
{: pre}

#### Command options
{: #activity-list-options}

<dl>
<dt>`--id WORKSPACE_ID`, `-i WORKSPACE_ID`</dt>
<dd>Required. The unique identifier of the workspace. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>
<dt>`--limit VALUE`, `-l VALUE`</dt>
<dd>Optional. The maximum number of activities that you want to see in your CLI output.</dd>
<dt>`--offset VALUE`, `-m VALUE`</dt>
<dd>Optional. The offset in the list.</dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #activity-list-example}

```
ibmcloud terraform activity list --id 123456 --limit 20 --offset 50 --json
```
{: pre}

### `ibmcloud terraform activity log`
{: #activity-log}

View detailed activity logs for actions that ran against a workspace.
{: shortdesc}

```
ibmcloud terraform activity log --id ACTIVITY_ID
```
{: pre}

#### Command options
{: #activity-log-options}

<dl>
<dt>`--id ACTIVITY_ID`, `-i ACTIVITY_ID`</dt>
<dd>Required. The ID of the activity for which you want to retrieve the logs. To find the ID of an activity, run <code>ibmcloud terraform activity list --id WORKSPACE_ID</code>. </dd>
</dl>

#### Example
{: #activity-log-example}

```
ibmcloud terraform activity log --id 987654321
```
{: pre}

### `ibmcloud terraform activity show`
{: #activity-show}

Retrieve details about a specific activity that ran against a workspace.
{: shortdesc}

```
ibmcloud terraform activity show --id ACTIVITY_ID [--json]
```
{: pre}

#### Command options
{: #activity-show-options}

<dl>
<dt>`--id ACTIVITY_ID`, `-i ACTIVITY_ID`</dt>
<dd>Required. The ID of the activity for which you want to retrieve more details. To find the ID of an activity, run <code>ibmcloud terraform activity list --id WORKSPACE_ID</code>.</dd>
<dt>`--json`, `-j`</dt>
<dd>Optional. Return the CLI output in JSON format.</dd>
</dl>

#### Example
{: #activity-show-example}

```
ibmcloud terraform activity show --id 987654 --json
```
{: pre}
