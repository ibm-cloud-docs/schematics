---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-11"

keywords: schematics cli reference, schematics commands, schematics cli, schematics reference

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
{:external: target="_blank" .external}


# CLI command reference	
{: #schematics-cli-reference}	

Refer to these commands when you want to automate the provisioning of {{site.data.keyword.cloud_notm}} resources. 	
{: shortdesc}	

To install the CLI, see [Setting up the CLI](/docs/schematics?topic=schematics-setup-cli). 	
{: tip}

## General commands
{: #schematics-general-commands}

Use these general commands to find help and version information for the {{site.data.keyword.bplong_notm}} CLI plug-in. 
{: shortdesc}

### `ibmcloud terraform help`
{: #schematics-help-cmd}

View the supported {{site.data.keyword.bplong_notm}} CLI commands. 
{: shortdesc}

```
ibmcloud terraform help
```
{: pre}

</br>
**Command options:** none

### `ibmcloud terraform version`
{: #schematics-version}

List the version of the Terraform CLI and {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform that the {{site.data.keyword.bpshort}} CLI uses. 
{: shortdesc}

```
ibmcloud terraform version
```
{: pre}

</br>
**Command options:** none
   	
## Workspace commands	
{: #schematics-workspace-commands}	

Review the commands that you can use to set up and work with your {{site.data.keyword.bplong_notm}} workspace. 
{: shortdesc}

### `ibmcloud terraform workspace action`
{: #schematics-workspace-action}

Retrieve all activities for a workspace, including the user ID of the person who initiated the action, the status, and a timestamp. 
{: shortdesc}

When you create a Terraform execution plan, or apply your Terraform template with {{site.data.keyword.bpshort}}, a {{site.data.keyword.bpshort}} action is automatically created and assigned an action ID. You can use the action ID to retrieve the logs of this action by using the [`ibmcloud terraform logs`](#schematics-logs) command.  

```
ibmcloud terraform workspace action --id WORKSPACE_ID [--act-id ACTION_ID] [--json]
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to retrieve workspace activities. To find the ID of your workspace, run <code>ibmcloud terraform workspace list</code>.
   </dd>

<dt><code>--act-id <em>ACTION_ID</em></code></dt>
<dd>Optional. Enter the ID of a action that you want to retrieve.  </dd>

<dt><code>--json</code></dt>
<dd>Optional. Return the CLI output in JSON format.   </dd>

</dl>

**Example:**
```
ibmcloud terraform workspace action --id 12345
```
{: pre}

### `ibmcloud terraform workspace delete`
{: #schematics-workspace-delete}

Delete a workspace from your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

The deletion of your workspace does not remove any {{site.data.keyword.cloud_notm}} resources that you provisioned with this workspace. You can access and work with your resources from the {{site.data.keyword.cloud_notm}} dashboard directly, but you cannot use {{site.data.keyword.bplong_notm}} to manage your resources after you delete the workspace. 
{: important}

```
ibmcloud terraform workspace delete --id WORKSPACE_ID [--force]
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace that you want to remove. To find the ID of your workspace, run <code>ibmcloud terraform workspace list</code>.
   </dd>

<dt><code>--force</code>, <code>-f</code></dt>
<dd>Optional. Force the deletion of your workspace without CLI prompts. </dd>

</dl>

**Example:**

```
ibmcloud terraform workspace delete --id workspace1234 
```
{: pre}

### `ibmcloud terraform workspace get`	
{: #schematics-workspace-get}	

Retrieve the details of an existing workspace, including the values of all input variables.	
{: shortdesc}	

```
ibmcloud terraform workspace get --id WORKSPACE_ID [--json]
```
{: pre}

**Command options:**
 
<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace, for which you want to retrieve the details. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
<dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform workspace get --id 1234 --json
```
{: pre}

### `ibmcloud terraform workspace list`	
{: #schematics-workspace-list}	

List the workspaces in your {{site.data.keyword.cloud_notm}} account and optionally, show the details for your workspace.	

```
ibmcloudd terraform workspace list [--limit LIMIT] [--offset OFFSET] [--json]
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--limit <em>LIMIT</em></code></dt>
  <dd>Optional. The maximum number of workspaces that you want to list. The number must be a positive integer starting from 1. 
   </dd>

<dt><code>--offset <em>OFFSET</em></code></dt>
<dd>Optional. The position of the workspace in the list of workspaces. For example, if you have three workspaces in your account, the command returns these workspaces as a list with three elements. To see a specific workspace in this list, you must enter the position number that the workspace has in the list. To list the first workspace in the list, enter <code>0</code>. To list the second workspace, enter <code>1</code> and so forth. Negative numbers are not supported and are ignored. </dd>

<dt><code>--json</code></dt>
<dd>Optional. Show detailed information about a workspace in JSON format. </dd>
</dl>

**Example:** 

```
ibmcloud terraform workspace list --json
```
{: pre}

### `ibmcloud terraform workspace new`	
{: #schematics-workspace-new}	

Create an {{site.data.keyword.bplong_notm}} workspace that points to your Terraform template in GitHub.  
{: shortdesc}	

To create a workspace, you must specify your workspace settings in a JSON file. Make sure that the JSON file follows the structure as outlined in this command. 
{: note}

```
ibmcloud terraform workspace new --file FILE_PATH [--state STATE_FILE_PATH] [--json]
```
{: pre}
 
**Command options:**

<dl>	
 <dt><code>--file <em>FILE_PATH</em></code>, <code>-f <em>FILE_PATH</em></code></dt>	
<dd>Required. The relative path to a JSON file on your local machine that is used to configure your workspace. 	
<br>Example JSON:	
<pre class="codeblock">	
<code>{	
  "name": "&lt;workspace_name&gt;",
  "type": [
    "&lt;terraform_version&gt;"
  ],
  "description": "&lt;workspace_description&gt;",
  "tags": [],
  "template_repo": {
    "url": "&lt;github_source_repo_url&gt;"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "&lt;terraform_version&gt;",
      "variablestore": [
        {
          "name": "&lt;variable_name1&gt;",
          "value": "&lt;variable_value1&gt;"
        },
        {
          "name": "&lt;variable_name2&gt;",
          "value": "&lt;variable_value2&gt;"
        }
      ]
    }
  ],
  "githubtoken": "&lt;github_personal_access_token&gt;"
}
</code></pre>
  <table>
   <caption>JSON file component description</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the JSON file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;workspace_name&gt;</code></td>
   <td>Required. Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). </td>
   </tr>
     <tr>
       <td><code>&lt;terraform_version&gt;</code></td>
       <td>Optional. The Terraform version that you want to use to run your Terraform code. Enter <code>terraform_v0.12</code> to use Terraform version 0.12, and <code>terraform_v0.11</code> to use Terraform version 0.11. If no value is specified, the Terraform config files are run with Terraform version 0.11. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
     </tr>
   <tr>
   <td><code>&lt;workspace_description&gt;</code></td>
   <td>Optional. Enter a description for your workspace. </td>
   </tr>
   <tr>
   <td><code>&lt;github_source_repo_url&gt;</code></td>
     <td>Required. Enter the link to your GitHub repository. The link can point to the <code>master</code> branch, a different branch, or a subdirectory. </td>
   </tr>
    <tr>
      <td><code>&lt;variable_name&gt; </br> &lt;variable_value&gt;</code></td>
      <td>Optional. Enter the name and value for the input variables that you declared in your Terraform configuration files. All variables that you enter in this section must be already declared in your Terraform configuration files. For more information about how to declare variables in a configuration file, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#configure-variables). </td>
     </tr></tbody></table></dd>	
<dt><code>--state <em>STATE_FILE_PATH</em></code></dt>
<dd>Optional. The relative path to an existing Terraform statefile on your local machine. To download a Terraform statefile, use the [`ibmcloud terraform state pull`](#state-pull) command. </dd>
<dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Print the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform workspace new --file myfile.json
```
{: pre}

### `ibmcloud terraform workspace update`	
{: #schematics-workspace-update}	

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud terraform plan`](#schematics-plan) command.	
{: shortdesc}	

```
ibmcloud terraform workspace update --id WORKSPACE_ID --file FILE_NAME [--json]
```
{: pre}

**Command options:**

<dl>	
  <dt><code>--file <em>FILE_NAME</em></code></dt>
  <dd>Required. The relative path to a JSON file on your local machine that includes the updated parameters for your workspace. 	
<br>Example JSON:	
<pre class="codeblock">	
<code>{
  "name": "&lt;workspace_name&gt;",
  "type": "&lt;terraform_version&gt;"
  "description": "&lt;workspace_description&gt;",
  "tags": [],
  "resource_group": "&lt;resource_group&gt;",
  "workspace_status": {
    "frozen": &lt;true_or_false&gt;
  },
  "template_repo": { 
    "url": "&lt;source_repo_url&gt;", 
    "branch" : "&lt;branch&gt;",
    "release": "&lt;release&gt;"
  },
  "template_data": [
    {
      "folder": "&lt;gh_folder&gt;",
      "type": "terraform_v0.12",
      "variablestore": [
        {
          "name": "&lt;variable_name1&gt;",
          "value": "&lt;variable_value1&gt;"
        },
        {
          "name": "&lt;variable_name2&gt;",
          "value": "&lt;variable_value2&gt;"
        }
      ]
    }
  ],
  "githubtoken": "&lt;github_personal_access_token&gt;"
}
</code></pre>
<table>
   <caption>JSON file component description</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the JSON file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>Optional. Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). If you update the name of the workspace, the ID of the workspace does not change. </td>
   </tr>
     <tr>
       <td><code>type</code> and <code>template_date.type</code></td>
       <td>Optional. The Terraform version that you want to use to run your Terraform code. Enter <code>terraform_v0.12</code> to use Terraform version 0.12, and <code>terraform_v0.11</code> to use Terraform version 0.11. If no value is specified, the Terraform config files are run with Terraform version 0.11. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
     </tr>
   <tr>
   <td><code>description</code></td>
   <td>Optional. Enter a description for your workspace. </td>
   </tr>
   <tr>
   <td><code>tags</code></td>
   <td>Optional. Enter tags that you want to associate with your workspace. Tags can help you find your workspace more easily. </td>
   </tr>
     <tr>
   <td><code>resource_group</code></td>
   <td>Optional. Enter the resource group where you want to provision your workspace. </td>
   </tr>
   <tr>
   <td><code>workspace_status</code></td>
   <td>Optional. Freeze or unfreeze a workspace. If a workspace is frozen, changes to the workspace are disabled.  </td>
   </tr>
   <tr>
   <td><code>template_repo.url</code></td>
   <td>Optional. Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored.  </td>
   </tr>
     <tr>
   <td><code>template_repo.branch</code></td>
   <td>Optional. Enter the GitHub or GitLab branch where your Terraform configuration files are stored.  </td>
   </tr>  
    <tr>
   <td><code>template_repo.release</code></td>
   <td>Optional. Enter the GitHub or GitLab release that points to your Terraform configuration files.  </td>
   </tr>
    <tr>
      <td><code>template_data.variablestore.name </br>template_data.variablestore.value</code></td>
      <td>Optional. Enter the name and value for the input variables that you declared in your Terraform configuration files. All variables that you enter in this section must be already declared in your Terraform configuration files. For more information about how to declare variables in a configuration file, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#configure-variables). </td>
     </tr></tbody></table></dd>	
  <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that you want to update. To retrieve the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
  <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform workspace update --id 1234 --file myfile.json --json
```
{: pre}

## {{site.data.keyword.cloud_notm}} resource management commands
{: #schematics-resource-commands}

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}}.

### `ibmcloud terraform apply`	
{: #schematics-apply}	

Scan and run the infrastructure code of your Terraform template that your workspace points to. When you apply a Terraform template, your resources are provisioned, modified, or removed in {{site.data.keyword.cloud_notm}}.   
{: shortdesc}

```
ibmcloud terraform apply --id WORKSPACE_ID [--target RESOURCE] [--force] [--json]
```
{: pre}

</br>
**Command options:**

<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source control repository that you want to apply in {{site.data.keyword.cloud_notm}}. To find the ID of your workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
 <dt><code>--force</code>, <code>-f</code></dt>	
<dd>Optional. Force the execution of this command without user prompts. </dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
  
<dt><code>--target <em>RESOURCE</em></code></dt>
<dd>Optional. Target the creation of a specific resource of your Terraform configuration file by entering the Terraform resource address, such as <code>ibm_is_instance.vm1</code>. All other resources that are defined in your configuration file remain uncreated or unupdated. To target the creation of multiple resources, use the following syntax: <code>--target &lt;resource1&gt; --target &lt;resource2&gt; </code>. If the targeted resource specifies the <code>count</code> attribute and no index is specified in the resource address, such as <code>ibm_is_instance.vm1[1]</code>, all instances that share the same resource name are targeted for creation. For more information about how to use the Terraform target feature, see [Resource targeting](https://www.terraform.io/docs/commands/plan.html#resource-targeting). </dd>

</dl>	

**Example:**

```
ibmcloud terraform apply --id 1234 --json --target ibm_is_instance.vm1
```
{: pre}


### `ibmcloud terraform destroy`	
{: #schematics-destroy}	

Remove the {{site.data.keyword.cloud_notm}} resources that you provisioned with your {{site.data.keyword.bpshort}} workspace, even if these resources are active. 
{: shortdesc}	

Use this command with caution. After you run the command, you cannot reverse the removal of your {{site.data.keyword.cloud_notm}} resources. If you used persistent storage, make sure that you created a backup for your data
{: important} 	

```
ibmcloud terraform destroy --id WORKSPACE_ID [--target RESOURCE] [--force] [--json]
```
{: pre}

</br>
**Command options:** 

<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source repository that specifies the {{site.data.keyword.cloud_notm}} resources that you want to remove. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
 <dt><code>--force</code>, <code>-f</code></dt>	
<dd>Optional. Force the execution of this command without user prompts. </dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	

<dt><code>--target <em>RESOURCE</em></code></dt>
<dd>Optional. Target the deletion of a specific resource by entering the Terraform resource address, such as <code>ibm_is_instance.vm1</code>. All other resources in your workspace remain unchanged. To target the deletion of multiple resources, use the following syntax: <code>--target &lt;resource1&gt; --target &lt;resource2&gt; </code>. If the targeted resource specifies the <code>count</code> attribute and no index is specified in the resource address, such as <code>ibm_is_instance.vm1[1]</code>, all instances that share the same resource name are targeted for deletion. Also, if the targeted resource can only be deleted if dependent resources are deleted, such as a VPC can only be deleted if the attached subnet is deleted, then all dependent resources are targeted for deletion as well. For more information about how to use the Terraform target feature, see [Resource targeting](https://www.terraform.io/docs/commands/plan.html#resource-targeting). </dd>

</dl>	

**Example:**

```
ibmcloud terraform destroy --id 1234 --json --target ibm_is_vpc.myvpc
```
{: pre}

### `ibmcloud terraform logs`	
{: #schematics-logs}	

Retrieve the Terraform log files for a {{site.data.keyword.bpshort}} workspace or a specific action ID. Use the log files to troubleshoot Terraform template issues or issues that occur during the resource provisioning, modification, or deletion process. 
{: shortdesc}	

```
ibmcloud terraform logs --id WORKSPACE_ID [--act-id ACTION_ID]
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to retrieve Terraform log files. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
 <dt><code>--act-id ACTION_ID</code></dt>	
<dd>Optional. The ID of an action for which you want to retrieve Terraform logs. To find a list of action IDs, run <code>ibmcloud terraform workspace action --id WORKSPACE_ID</code>.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform logs --id workspace-abcd12345yt --act-id 9876543121abc1234cdst
```
{: pre}

### `ibmcloud terraform plan`	
{: #schematics-plan}	

Scan the Terraform template in your source repository and compare this template against the {{site.data.keyword.cloud_notm}} resources that are already deployed. The CLI output shows the {{site.data.keyword.cloud_notm}} resources that must be added, modified, or removed to achieve the state that is described in your configuration file.   	
{: shortdesc}	

```
ibmcloud terraform plan --id WORKSPACE_ID [--json]
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source repository that you want to scan. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform plan --id 1234 --json
```
{: pre}

## Terraform statefile commands
{: #statefile-cmds}

Review the commands that you can use to work with the Terraform statefile (`terraform.tfstate`) for a workspace.
{: shortdesc}

You can import an existing Terraform statefile during the creation of your workspace. For more information, see the [`ibmcloud workspace new`](#schematics-workspace-new) command. 
{: note}

### `ibmcloud terraform state list`
{: #state-list}

Show the content of the Terraform statefile (`terraform.tfstate`) for a workspace.  
{: shortdesc}	

```
ibmcloud terraform state list --id WORKSPACE_ID
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to show the Terraform statefile content. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	
</dl>	

**Example:**

```
ibmcloud terraform state list --id 1234
```
{: pre}

### `ibmcloud terraform state pull`
{: #state-pull}

Download the Terraform statefile (`terraform.tfstate`) for a workspace to our local machine.  
{: shortdesc}	

```
ibmcloud terraform state pull --id WORKSPACE_ID --template TEMPLATE_ID
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to download the Terraform statefile to your local machine. To find the ID of a workspace, run <code>ibmcloud terraform workspace list</code>.</dd>	

<dt><code>--template <em>TEMPLATE_ID</em></code></dt>
<dd>Required. The unique identifier of the Terraform template that you used. </dd>
</dl>	

**Example:**

```
ibmcloud terraform state pull --id 1234 --template 9876
```
{: pre}





