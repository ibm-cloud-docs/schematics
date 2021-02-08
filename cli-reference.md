---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-05"

keywords: schematics cli reference, schematics commands, schematics cli, schematics reference, cli

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

As of 31 March 2020, the {{site.data.keyword.bpshort}} command syntax changed from `ibmcloud terraform` to `ibmcloud schematics`. You can continue to use `ibmcloud terraform` commands as an alias, but note that this alias might become unsupported in future CLI versions. 
{: important}

## General commands
{: #schematics-general-commands}

Use these general commands to find help and version information for the {{site.data.keyword.bplong_notm}} CLI plug-in. 
{: shortdesc}

### `ibmcloud schematics help`
{: #schematics-help-cmd}

View the supported {{site.data.keyword.bplong_notm}} CLI commands. 
{: shortdesc}

```
ibmcloud schematics help
```
{: pre}

</br>
**Command options:** none 

### `ibmcloud schematics version`
{: #schematics-version}

List the version of the Terraform CLI and {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform that the {{site.data.keyword.bpshort}} CLI uses. 
{: shortdesc}

```
ibmcloud schematics version
```
{: pre}

</br>
**Command options:** none


## Workspace commands	
{: #schematics-workspace-commands}	

Review the commands that you can use to set up and work with your {{site.data.keyword.bplong_notm}} workspace. 
{: shortdesc}

### `ibmcloud schematics workspace action`
{: #schematics-workspace-action}

Retrieve all activities for a workspace, including the user ID of the person who initiated the action, the status, and a timestamp. 
{: shortdesc}

When you create a Terraform execution plan, or apply your Terraform template with {{site.data.keyword.bpshort}}, a {{site.data.keyword.bpshort}} action is automatically created and assigned an action ID. You can use the action ID to retrieve the logs of this action by using the [`ibmcloud schematics logs`](#schematics-logs) command.  

```
ibmcloud schematics workspace action --id WORKSPACE_ID [--act-id ACTION_ID] [--json]
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to retrieve workspace activities. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--act-id <em>ACTION_ID</em></code></dt>
<dd>Optional. Enter the ID of a action that you want to retrieve.  </dd>

<dt><code>--json</code></dt>
<dd>Optional. Return the CLI output in JSON format.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace action --id myworkspace-a1aa1a1a-a11a-11
```
{: pre}

### `ibmcloud schematics workspace delete`
{: #schematics-workspace-delete}

Delete a workspace from your {{site.data.keyword.cloud_notm}} account. The deletion of your workspace does not remove any {{site.data.keyword.cloud_notm}} resources that you provisioned with this workspace. You can access and work with your resources from the {{site.data.keyword.cloud_notm}} dashboard directly, but you cannot use {{site.data.keyword.bplong_notm}} to manage your resources after you delete the workspace. 

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again. The table describes the delete workspace and destroy resources with the action.
{: shortdesc}

Decide if you want to delete the workspace, any associated resources, or both. This action cannot be undone. If you remove the workspace and keep the resources, you need to manage the resources with the resource list or CLI.
{: note}

<table>
	<tr>
		<th>Action</th><th>Delete workspace</th><th>Delete all associated resources</th></tr>
	<tr>
		<td>Delete workspace</td><td>True</td><td>False</td></tr>
	<tr>
		<td>Delete only resources</td><td>False</td><td>True</td></tr>
	<tr>
		<td>Delete workspace and the resources provisioned by workspace</td><td>True</td><td>True</td></tr>
	<tr>
		<td>Resources destroyed using CLI or resource list, and want to delete workspace</td><td>True</td><td>False</td></tr>
</table> <br>

```
ibmcloud schematics workspace delete --id WORKSPACE_ID [--force]
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace that you want to remove. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--force</code>, <code>-f</code></dt>
<dd>Optional. Force the deletion of your workspace without CLI prompts. </dd>

</dl>

**Example:**

```
ibmcloud schematics workspace delete --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics workspace get`	
{: #schematics-workspace-get}	

Retrieve the details of an existing workspace, including the values of all input variables.	
{: shortdesc}	

```
ibmcloud schematics workspace get --id WORKSPACE_ID [--json]
```
{: pre}

**Command options:**
 
<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace, for which you want to retrieve the details. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
<dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics workspace get --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}

### `ibmcloud schematics workspace import`
{: #schematics-workspace-import}

You can import the existing resource with an valid address from the workspace ID and import it into your Terraform state. You need to ensure one resource can be imported to only one Terraform resource address. Otherwise, you may see unwanted behavior from Terraform.
{: shortdesc}

```
ibmcloud schematics workspace import --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETER>
```
{: pre}

</br>

**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to import an instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to import. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the address parameter for the command.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace import --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}



### `ibmcloud schematics workspace list`	
{: #schematics-workspace-list}	

List the workspaces in your {{site.data.keyword.cloud_notm}} account and optionally, show the details for your workspace.	

```
ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--json]
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
ibmcloud schematics workspace list --json
```
{: pre}

### `ibmcloud schematics workspace new`	
{: #schematics-workspace-new}	

Create an {{site.data.keyword.bplong_notm}} workspace that points to your Terraform template in GitHub or GitLab. If you want to provide your Terraform template by uploading a tape archive file (`.tar`), you can create the workspace without a connection to a GitHub repository and then use the [`ibmcloud schematics workspace upload`](#schematics-workspace-upload) command to provide the template.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

To create a workspace, you must specify your workspace settings in a JSON file. Make sure that the JSON file follows the structure as outlined in this command. 
{: note}

```
ibmcloud schematics workspace new --file FILE_PATH [--state STATE_FILE_PATH] [--json]
```
{: pre}
 
**Command options:**

<dl>	
 <dt><code>--file <em>FILE_PATH</em></code>, <code>-f <em>FILE_PATH</em></code></dt>	
<dd>Required. The relative path to a JSON file on your local machine that is used to configure your workspace. 	
<br>Example JSON for using a GitHub or GitLab repository:	
<pre class="codeblock">	
<code>{
  "name": "&lt;workspace_name&gt;>",
  "type": [
    "&lt;terraform_version&gt;"
  ],
  "location": "&lt;location&gt;",
  "description": "&lt;workspace_description&gt;",
  "tags": [],
  "template_repo": {
    "url": "&lt;github_source_repo_url&gt;"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "&lt;terraform_version&gt;",
      "env_values":[
      {
        "VAR1":"&lt;val1&gt;"
      },
      {
        "VAR2":"&lt;val2&gt;"
      }
      ],
      "variablestore": [
        {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "string",
          "secure": true,
          "description":"&lt;description&gt;"
        },
        {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "bool",
          "secure": false,
          "description":"&lt;description&gt;"
        },
    {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "list(string);",
          "secure": false,
         "description":"&lt;description&gt;"
        },
    {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "map(number)",
          "secure": false,
          "description":"&lt;description&gt;"
        },
    {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "tuple([string, list(string), number, bool])",
          "secure": false,
         "description":"&lt;description&gt;"
        },
    {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "any",
          "secure": false,
          "description":"&lt;description&gt;"
        }
      ]
    }
  ],
  "githubtoken": "&lt;github_personal_access_token&gt;"
}
</code></pre></br>

Now, in template_repo, you can also provide `url` with more parameters as shown in the block.
  <pre class="codeblock">	
  <code>"url": "https://github.com/IBM-Cloud/terraform-provider-ibm",
     "branch": "master;",
     "datafolder": “examples/ibm-vsi”,
     "release": "v1.8.0" </code></pre>
{: note}

Example JSON for uploading a `.tar` file later:

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
     "url": "&lt;github_source_repo_url&gt;"
   },
  "template_data": [
    {
      "folder": ".",
      "type": "&lt;terraform_version&gt;",
      "env_values":[
      {
        "VAR1":"&lt;val1&gt;"
      },
      {
        "VAR2":"&lt;val2&gt;"
      }
      ],
      "variablestore": [
        {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "string",
          "secure": true,
	        "description":"&lt;description&gt;"
        },
        {
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "bool",
          "secure": false,
	        "description":"&lt;description&gt;"
        },
      	{
          "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "list(string)",
          "secure": false,
	        "description":"&lt;description&gt;"
        },
	      {
	        "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "map(number)",
          "secure": false,
	        "description":"&lt;description&gt;"
        },
	      {
	       "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "tuple([string, list(string), number, bool])",
          "secure": false,
	        "description":"&lt;description&gt;"
        },
	      {
	        "name": "&lt;variable_name_x&gt;",
          "value": "&lt;variable_value_x&gt;",
          "type": "any",
          "secure": false,
	        "description":"&lt;description&gt;"
        }
      ]
    }
  ]
}
</code></pre>
  <table>
   <caption>JSON file component description</caption>
   <col style="width:30%">
	 <col style="width:70%">
   <thead>
     <th>Parameter</th>
     <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;workspace_name&gt;</code></td>
   <td>Required. Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). </td>
   </tr>
     <tr>
       <td><code>&lt;terraform_version&gt;</code></td>
       <td>Optional. The Terraform version that you want to use to run your Terraform code. Enter <code>Terraform_v0.12</code> to use Terraform version 0.12, and <code>terraform_v0.11</code> to use Terraform version 0.11. If no value is specified, the Terraform config files are run with Terraform version 0.11. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
     </tr>
    <tr>
       <td><code>&lt;location&gt;</code></td>
       <td>Optional. Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} actions run and where your workspace data is stored. If you do not enter a location, {{site.data.keyword.bpshort}} determines the location based on the {{site.data.keyword.cloud_notm}} region that you targeted. To view the region that you targeted, run <code>ibmcloud target --output json</code> and look at the <em>region</em> field. To target a different region, run <code>ibmcloud target -r &lt;region&gt;</code>. If you enter a location, make sure that the location matches the {{site.data.keyword.cloud_notm}} region that you targeted.   </td>
     </tr>
   <tr>
   <td><code>&lt;workspace_description&gt;</code></td>
   <td>Optional. Enter a description for your workspace. </td>
   </tr>
   <tr>
   <td><code>&lt;github_source_repo_url&gt;</code></td>
     <td>Optional. Enter the link to your GitHub repository. The link can point to the <code>master</code> branch, a different branch, or a subdirectory. If you choose to create your workspace without a GitHub repository, your workspace is created with a <strong>draft</strong> state. To connect your workspace to a GitHub repository later, you must use the <code>ibmcloud schematics workspace update</code> command. If you plan to provide your Terraform template by uploading a tape archive file (`.tar`), leave the URL empty, and use the [<code>ibmcloud schematics workspace upload</code>](#schematics-workspace-upload) command after you created the workspace. <br>Cloning GitHub repository in {{site.data.keyword.bplong_notm}} is allowed only to the listed extension files. The files having more than 500 KB in size, invalid image extension, and the extension files such as `.php5` `.pht` `.phtml` `.shtml` `.asa` `.cer` `.asax` `.swf` `.xap` `.tfstate` `.tfstate.backup` are considered as vulnerable files when cloning.
      -	Allowed extension: `.tf` `.tfvars` `.md` `.yaml` `.sh` `.txt` `.yml` `.html` `.tf` `.json` `.gitignore` `license` `.js` `.pub` `.service` `_rsa`
      -	Blocked extension: `.php5` `.pht` `.phtml` `.shtml` `.asa` `.cer` `.asax` `.swf` `.xap` `.tfstate` `.tfstate.backup`
      -	Allowed image extension: `.tif` `.tiff` `.gif` `.png` `.bmp` `.jpg` `.jpeg` </td>
   </tr>
    <tr>
     <td><code>&lt;env_values&gt;</code></td>
     <td>Optional. A list of environment variables that you want to apply during the execution of a bash script or Terraform action. This field must be provided as a list of key-value pairs. Each entry will be a map with one entry where `key is environment variable name` and `value is value`. You can define environment variables for IBM Cloud catalog offerings that are provisioned by using a bash script.
 files.</td>
     </tr>
    <tr>
     <td><code>&lt;variable_name&gt;</code></td>
     <td>Optional. Enter the name for the input variable that you declared in your Terraform configuration files.</td>
     </tr>
      <tr>
      <td><code>&lt;variable_type&gt;</code></td>
      <td>Optional. `Terraform v0.11` supports `string`, `list`, `map` data type. For more information, about the syntax, see [Configuring input variables](https://www.terraform.io/docs/configuration-0-11/variables.html). <br> `Terraform v0.12` additionally, supports `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`. For more information, about the syntax to use the complex data type, see [Configuring variables](https://www.terraform.io/docs/configuration/variables.html#type-constraints).</td>
      </tr>
      <tr>
     <td><code>&lt;variable_value&gt;</code></td>
     <td>Optional. Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information, about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable). <br> **Example**<br><pre class="codeblock">	
<code>"variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))"
                },
      ]</code></pre></td>
     </tr>
      <tr>
      <td><code>&lt;secure&gt;</code></td>
      <td>Optional. Set the <code>secure</code> parameter to <strong>true</strong>. By default, this parameter is set to <strong>false</strong>.</td>
      </tr>
      <tr>
      <td><code>&lt;val1&gt;</code></td>
      <td>Optional. In the payload you can provide an environment variable that can execute in your workspace during plan, apply or destroy stage. Also values are encrypted and stored in cos.</td>
      </tr>
      </tbody></table></dd>
<dt><code>--state <em>STATE_FILE_PATH</em></code></dt>
<dd>Optional. The relative path to an existing Terraform statefile on your local machine. To create the Terraform statefile: <ol><li>Show the content of an existing Terraform statefile by using the [`ibmcloud terraform state pull`](#state-pull) command.</li><li>Copy the content of the statefile from your CLI output in to a file on your local machine that is named <code>terraform.tfstate</code>.</li><li>Use the relative path to the file in the <code>--state</code> command parameter.</li></ol></dd>
<dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Print the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics workspace new --file myfile.json
```
{: pre}



### `ibmcloud schemates workspace output`
{: #schematics-output}

Displays all the instance or resource output of the workspace. You can provide output `NAME`, to print only the value of that output.
{: shortdesc}

```
ibmcloud schematics workspace output --id <WORKSPACE_ID> --options <FLAGS> --name <PARAMETER>
```
{: pre}

</br>

**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to print an instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to import. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--name</code></dt>
<dd>Optional. Provide the name of the parameter to print.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace output --id myworkspace-asdff1a1a-42145-11 --name null_resource.sleep  
```
{: pre}



### `ibmcloud schematics refresh`
{: #schematics-refresh}

Perform a Schematics refresh action against your workspace. A refresh action validates the {{site.data.keyword.cloud_notm}} resources in your account against the state that is stored in the Terraform statefile of your workspace. If differences are found, the Terraform statefile is updated accordingly. 
{: shortdesc}

```
ibmcloud schematics refresh --id WORKSPACE_ID [--json]
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that you want to refresh. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics refresh --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics state list`
{: #state-list}

List the {{site.data.keyword.cloud_notm}} resources that are documented in your Terraform statefile (`terraform.tfstate`).  
{: shortdesc}	

```
ibmcloud schematics state list --id WORKSPACE_ID
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to list the {{site.data.keyword.cloud_notm}} resources that are documented in the Terraform statefile. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics state list --id myworkspace-a1aa1a1a-a11a-11  
```
{: pre}


### `ibmcloud schematics workspace taint`
{: #schematics-workspace-taint}

Manually marks an instance or resources as tainted, by forcing the resources to be recreated on the next apply. Taint modifies the state file, but not the infrastructure in your workspace. When you perform next plan the changes will show as recreated and in the next apply the change is implemented.
{: shortdesc}

```
ibmcloud schematics workspace taint --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETER>
```
{: pre}

</br>

**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to re-create the instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to show. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the address parameter for the command.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace taint --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}


### `ibmcloud schematics workspace untaint`
{: #schematics-workspace-untaint}

Manually marks an instance or resources as untainted, by forcing the resources to be restored on the next apply. When you perform next plan the changes will show as restored and in the next apply the change is implemented.
{: shortdesc}

```
ibmcloud schematics workspace untaint --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETER>
```
{: pre}

</br>

**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to restore the instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to show. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the address parameter for the command.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace untaint --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}



### `ibmcloud schematics workspace update`	
{: #schematics-workspace-update}	

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud schematics plan`](#schematics-plan) command.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

If you provided your Terraform template by uploading a tape archive file (`.tar`) and you want to update your template, you must use the [`ibmcloud schematics workspace upload`](#schematics-workspace-upload) command.
{: note}

```
ibmcloud schematics workspace update --file FILE_NAME --id WORKSPACE_ID [--json]
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
  "type": "&lt;terraform_version&gt;",
  "description": "&lt;workspace_description&gt;",
  "tags": [],
  "resource_group": "&lt;resource_group&gt;",
  "workspace_status": {
    "frozen": "&lt;true_or_false&gt;"
  },
  "template_repo": { 
    "url": "&lt;source_repo_url&gt;"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "&lt;terraform_version&gt;",
      "env_values":[
      {
        "VAR1":"&lt;val1&gt;"
      },
      {
        "VAR2":"&lt;val2&gt;"
      }
      ],
      "variablestore": [
        {
          "name": "&lt;variable_name1&gt;",
          "value": "&lt;variable_value1&gt;",
          "type": "&lt;variable_type1&gt;",
          "secure": true,
	  "use_default": true        },
        {
          "name": "&lt;variable_name2&gt;",
          "value": "&lt;variable_value2&gt;",
          "type": "&lt;variable_type2&gt;",
          "secure": false,
	  "use_default": true
	  }
      ]
    }
  ],
  "githubtoken": "&lt;github_personal_access_token&gt;"
}
</code></pre>

Now, in template_repo, you can also update `url` with more parameters as shown in the block.
  <pre class="codeblock">	
  <code>"url": "https://github.com/IBM-Cloud/terraform-provider-ibm",
     "branch": "master;",
     "datafolder": “examples/ibm-vsi”,
     "release": "v1.8.0" </code></pre>
{: note}
 
<table>
   <caption>JSON file component description</caption>
   <col style="width:30%">
   <col style="width:70%">
   <thead>
   <th>Parameter</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>Optional. Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). If you update the name of the workspace, the ID of the workspace does not change. </td>
   </tr>
     <tr>
       <td><code>type</code> and <code>template_date.type</code></td>
       <td>Optional. The Terraform version that you want to use to run your Terraform code. Enter <code>Terraform_v0.12</code> to use Terraform version 0.12, and <code>Terraform_v0.11</code> to use Terraform version 0.11. If no value is specified, the Terraform config files are run with Terraform version 0.11. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
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
   <td><code>template_repo.datafolder</code></td>
   <td>Optional. Enter the GitHub or GitLab folder that points to your Terraform configuration files.  </td>
   </tr>
    <tr>
   <td><code>template_repo.release</code></td>
   <td>Optional. Enter the GitHub or GitLab release that points to your Terraform configuration files.  </td>
   </tr>
     <tr>
   <td><code>&lt;github_source_repo_url&gt;</code></td>
     <td>Optional. Enter the link to your GitHub repository. The link can point to the <code>master</code> branch, a different branch, or a subdirectory. </td>
     <tr>
     <td><code>&lt;template_data.variablestore.name&gt;</code></td>
     <td>Optional. Enter the name for the input variable that you declared in your Terraform configuration files.</td>
     </tr>
      <tr>
      <td><code>&lt;template_data.variablestore.type&gt;</code></td>
      <td>Optional. `Terraform v0.11` supports `string`, `list`, `map` data type. For more information, about the syntax, see [Configuring input variables](https://www.terraform.io/docs/configuration-0-11/variables.html). <br> `Terraform v0.12` additionally, supports `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`. For more information, about the syntax to use the complex data type, see [Configuring variables](https://www.terraform.io/docs/configuration/variables.html#type-constraints).</td>
      </tr>
      <tr>
     <td><code>&lt;template_data.variablestore.value&gt;</code></td>
     <td>Optional. Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You can override the default values of `.tfvars` by setting `use_default` parameter as `true`. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information, about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable), as shown in the example.<br><pre class="codeblock">	
<code>"variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))",
		    "use_default":true
                },
      ]</code></pre></td>
     </tr>
      <tr>
      <td><code>&lt;template_data.variablestore.secure&gt;</code></td>
      <td>Optional. Set the <code>secure</code> parameter to <strong>true</strong>. By default, this parameter is set to <strong>false</strong>.</td>
      </tr>
      <tr>
      <td><code>&lt;template_data.variablestore.use_default&gt;</code></td>
      <td>Optional. Set the <code>use_default</code> parameter to <strong>true</strong> to override the default `.tfvars` parameter. By default, this parameter is set to <strong>false</strong>.</td>
      </tr>
      <tr>
      <td><code>&lt;env_values.val1&gt;</code></td>
      <td>Optional. In the payload you can provide an environment variables, and customized variables that can execute in your workspace during plan, apply or destroy stage. Also values are encrypted and stored in COS.</td>
      </tr>	
   </tbody></table></dd>	
  <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that you want to update. To retrieve the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
  <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics workspace update --id myworkspace-a1aa1a1a-a11a-11 --file myfile.json --json
```
{: pre}

### `ibmcloud schematics workspace upload`
{: #schematics-workspace-upload}

Provide your Terraform template by uploading a tape archive file (`.tar`) to your {{site.data.keyword.bpshort}} workspace.
{: shortdesc}

Before you begin, make sure that you [created your workspace](#schematics-workspace-new) without a link to a GitHub or GitLab repository.
{: important}

```
ibmcloud schematics workspace upload --id WORKSPACE_ID --file PATH_TO_FILE --template TEMPLATE_ID [--output]
```
{: pre}

</br>
**Command options:**

<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace where you want to upload your tape archive file (`.tar`). To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
 <dt><code>--file <em>PATH_TO_FILE</em></code></dt>	
<dd>Required. Enter the full file path on your local machine where your `.tar` file is stored. </dd>	
 <dt><code>--template <em>TEMPLATE_ID</em></code></dt>	
<dd>Required. The unique identifier of the Terraform template for which you want to show the content of the Terraform statefile. To find the ID of the template, run <code>ibmcloud schematics workspace get --id &lt;workspace_ID&gt;</code> and find the template ID in the <strong>Template Variables for:</strong> field of your CLI output. </dd>

<dt><code>--output</code></dt>
<dd>Return the CLI output in JSON format.</dd>
</dl>

**Example 1:**

```
ibmcloud schematics workspace upload --id myworkspace-a1aa1a1a-a11a-11 --file /Users/myuser/Documents/mytar/vpc.tar --template 250d6e9f-d71b-4c

```
{: pre}

 Create the `TAR` file of your template repo using the `TAR` command given `tar -cvf vpc.tar $TEMPLATE_REPO_FOLDER`
 {: note}


## {{site.data.keyword.cloud_notm}} resource management commands
{: #schematics-resource-commands}

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}}.

### `ibmcloud schematics apply`	
{: #schematics-apply}	

Scan and run the infrastructure code of your Terraform template that your workspace points to. When you apply a Terraform template, your resources are provisioned, modified, or removed in {{site.data.keyword.cloud_notm}}.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}

Your workspace must be in an **Inactive**,  **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} apply action. For more information, about workspace state, see [Workspace state diagram](/docs/schematics?topic=schematics-workspace-setup#workspace-state-diagram).
{: note}

While your infrastructure code runs in {{site.data.keyword.bplong_notm}}, you cannot make any changes to your workspace.
{: note}

```
ibmcloud schematics apply --id WORKSPACE_ID [--target RESOURCE] [--var-file TFVARS_FILE_PATH] [--force] [--json]
```
{: pre}

</br>
**Command options:**

<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source control repository that you want to apply in {{site.data.keyword.cloud_notm}}. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
 <dt><code>--force</code>, <code>-f</code></dt>	
<dd>Optional. Force the execution of this command without user prompts. </dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
  
<dt><code>--target <em>RESOURCE</em></code>, <code>-t <em>RESOURCE</em></code></dt>
<dd>Optional. Target the creation of a specific resource of your Terraform configuration file by entering the Terraform resource address, such as <code>ibm_is_instance.vm1</code>. All other resources that are defined in your configuration file remain uncreated or unupdated. To target the creation of multiple resources, use the following syntax: <code>--target &lt;resource1&gt; --target &lt;resource2&gt; </code>. If the targeted resource specifies the <code>count</code> attribute and no index is specified in the resource address, such as <code>ibm_is_instance.vm1[1]</code>, all instances that share the same resource name are targeted for creation. For more information about how to use the Terraform target feature, see [Resource targeting](https://www.terraform.io/docs/commands/plan.html#resource-targeting). </dd>

<dt><code>--var-file <em>TFVARS_FILE_PATH</em></code>, <code>--vf <em>TFVARS_FILE_PATH</em></code></dt>
<dd>Optional. The file path to the <code>terraform.tfvars</code> file that you created on your local machine. Use this file to store sensitive information, such as the {{site.data.keyword.cloud_notm}} API key or credentials to connect to {{site.data.keyword.cloud_notm}} classic infrastructure in the format <code>&lt;key&gt;=&lt;value&gt;</code>. All key value pairs that are defined in this file are automatically loaded into Terraform when you initialize the Terraform CLI. To specify multiple <code>tfvars</code> files, specify <code>--var-file TFVARS_FILE_PATH1 --var-file TFVARS_FILE_PATH2</code>.</dd>

</dl>	

**Example:**

```
ibmcloud schematics apply --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_instance.vm1 --var-file ./terraform.tfvars
```
{: pre}


### `ibmcloud schematics destroy`	
{: #schematics-destroy}	

Remove the {{site.data.keyword.cloud_notm}} resources that you provisioned with your {{site.data.keyword.bpshort}} workspace, even if these resources are active.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

Use this command with caution. After you run the command, you cannot reverse the removal of your {{site.data.keyword.cloud_notm}} resources. If you used persistent storage, make sure that you created a backup for your data
{: important} 	

Your workspace must be in an **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} destroy action. 
{: note}

```
ibmcloud schematics destroy --id WORKSPACE_ID [--target RESOURCE] [--force] [--json]
```
{: pre}

</br>
**Command options:** 

<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source repository that specifies the {{site.data.keyword.cloud_notm}} resources that you want to remove. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
 <dt><code>--force</code>, <code>-f</code></dt>	
<dd>Optional. Force the execution of this command without user prompts. </dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	

<dt><code>--target <em>RESOURCE</em></code></dt>
<dd>Optional. Target the deletion of a specific resource by entering the Terraform resource address, such as <code>ibm_is_instance.vm1</code>. All other resources in your workspace remain unchanged. To target the deletion of multiple resources, use the following syntax: <code>--target &lt;resource1&gt; --target &lt;resource2&gt; </code>. If the targeted resource specifies the <code>count</code> attribute and no index is specified in the resource address, such as <code>ibm_is_instance.vm1[1]</code>, all instances that share the same resource name are targeted for deletion. Also, if the targeted resource can only be deleted if dependent resources are deleted, such as a VPC can only be deleted if the attached subnet is deleted, then all dependent resources are targeted for deletion as well. For more information about how to use the Terraform target feature, see [Resource targeting](https://www.terraform.io/docs/commands/plan.html#resource-targeting). </dd>

</dl>	

**Example:**

```
ibmcloud schematics destroy --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_vpc.myvpc
```
{: pre}

### `ibmcloud schematics logs`	
{: #schematics-logs}	

Retrieve the Terraform log files for a {{site.data.keyword.bpshort}} workspace or a specific action ID. Use the log files to troubleshoot Terraform template issues or issues that occur during the resource provisioning, modification, or deletion process. 
{: shortdesc}	

```
ibmcloud schematics logs --id WORKSPACE_ID [--act-id ACTION_ID]
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to retrieve Terraform log files. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
 <dt><code>--act-id ACTION_ID</code></dt>	
<dd>Optional. The ID of an action for which you want to retrieve Terraform logs. To find a list of action IDs, run <code>ibmcloud schematics workspace action --id WORKSPACE_ID</code>.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics logs --id myworkspace-a1aa1a1a-a11a-11 --act-id 9876543121abc1234cdst
```
{: pre}

### `ibmcloud schematics output`
{: #schematics-output}

Retrieve a list of Terraform output values. You define output values in your Terraform template to include information that you want to make accessible for other Terraform templates.
{: shortdesc}

```
ibmcloud schematics output --id WORKSPACE_ID
```
{: pre}

**Command options:**
<dl>	
 <dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace for which you want to list Terraform output values. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
  </dl>
  
**Example:**

```
ibmcloud schematics output --id myworkspace3_2-31cf7130-d0c4-4d
```
{: pre}

### `ibmcloud schematics plan`	
{: #schematics-plan}	

Scan the Terraform template in your source repository and compare this template against the {{site.data.keyword.cloud_notm}} resources that are already deployed. The CLI output shows the {{site.data.keyword.cloud_notm}} resources that must be added, modified, or removed to achieve the state that is described in your configuration file.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

Your workspace must be in an **Inactive**, **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} plan action. 
{: note}

During the creation of the Terraform execution plan, you cannot make any changes to your workspace. 
{: note}

```
ibmcloud schematics plan --id WORKSPACE_ID [--json]
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code>, <code>-i <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that points to the Terraform template in your source repository that you want to scan. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	
 <dt><code>--json</code>, <code>-j</code></dt>	
<dd>Optional. Return the CLI output in JSON format.</dd>	
</dl>	

**Example:**

```
ibmcloud schematics plan --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}



## Terraform commands
{: #tf-cmds}

You can run a bunch of Terraform commands and manipulate the {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}} API or CLI. The Schematics provides one generic API `commands` for each sub-command.
{: shortdesc}

You can see the `Commands` UI support only to display the state of the workspace. The complete commands support to be released shortly.
{: important}

The table provides the summary of commands supported by the new `commands` API.

|Command | Description| 
|------|  ------|
|`show`| Inspects Terraform state or plan.|
|`output`| Reads an output from a Terraform state file.|
|`import`| Imports an existing infrastructure into Terraform.|
|`taint`|	 Mark a resource for recreation. |
|`untaint`|Do not mark a resource as tainted.|
|`state`|	An advanced state management command to write sub commands to remove or move `rm && mv`.|

### Commands 
{: :#cmds}

The `Commands` API supports 
- Executing the group of Terraform commands by using the JSON file for your workspace command requirements.
- The access control `plan`, `apply`, `destroy`, or `refresh` are applicable for `Commands API`. 

Select your region where the workspace is created, and use the following syntax to run the commands API.

```
ibmcloud schematics commands --id <WORKSPACE_ID> --options <FLAGS> --file <JSON file>
```

**Command options**

`--id <WORKSPACE_ID>`
  Required. The unique ID of the workspace for which you want to run the commands. To find the of your workspace, run `ibmcloud schematics workspace list`.

`--options <FLAGS>`
  Optional. The command-line flags are all optional. Some of the option flags are **-lock=true, -state=path, -allow-missing, -backup-path**. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html).

`--file <JSON file>`
  Required. Contains the address of resource to be executed.

  **Sample Test.JSON file**
  
  ```
  {
    "commands": [
        {
            "command": "state show",
            "command_params": "data.tempxx_file.test",
            "command_name": "Test1",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "taint",
            "command_params": "null_resourcexx.sleep",
            "command_name": "Test2",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "untaint",
            "command_params": "null_resourcexx.sleep",
            "command_name": "Test3",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "state list",
            "command_params": "",
            "command_name": "Test4",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "state rm",
            "command_params": "data.tempxx_file.test",
            "command_name": "Test5",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "output",
            "command_params": "",
            "command_name": "Test6",
            "command_desc": "Checking state output",
            "command_onerror": "continue"
        }
    ],
}
  ```

  The table provides the list of key parameters of the JSON file for the `Commands` API, either by CLI or the API.

  | Key | Required / Optional |Description |
  | ------ | -------- | ---------- |
  |`command`| Required |Provide the command. Supported commands are `show`,`taint`, `untaint`, `state`, `import`, `output`.|
  |`command_params`| Required | The address parameters for the command name for `CLI`, such as resource name, absolute path of the file name. **Note** For API, you have to send option flag and address parameter in `command_params`.|
  |`command_name`| Optional | The name for the command block.|
  |`command_desc`| Optional | The text to describe the command block.|
  |`command_onError`| Optional |  Instruction to continue or break in case of error in the command. |
  |`command_dependsOn`|Optional| Dependency on the previous commands.|
  |`command_status`| Not required | Displays the command executed status, either `success` or `failure`|

**Example**

```
ibmcloud schematics commands --id cli-sleepy-0bedc51f-c344-50 --file /<userdir>/Test.JSON
```

## Terraform statefile commands
{: #statefile-cmds}

Review the commands that you can use to work with the Terraform statefile (`terraform.tfstate`) for a workspace.
{: shortdesc}

You can import an existing Terraform statefile during the creation of your workspace. For more information, see the [`ibmcloud workspace new`](#schematics-workspace-new) command. 
{: note}

### `ibmcloud schematics state pull`
{: #state-pull}

Show the content of the Terraform statefile (`terraform.tfstate`) for a specific Terraform template in your workspace.  
{: shortdesc}	

```
ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID
```
{: pre}

</br>
**Command options:**

<dl>	
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>	
<dd>Required. The unique identifier of the workspace that stores the Terraform template for which you want to show the content of the Terraform statefile. To find the ID of a workspace, run <code>ibmcloud schematics workspace list</code>.</dd>	

<dt><code>--template <em>TEMPLATE_ID</em></code></dt>
<dd>Required. The unique identifier of the Terraform template for which you want to show the content of the Terraform statefile. To find the ID of the template, run <code>ibmcloud schematics workspace get --id &lt;workspace_ID&gt;</code> and find the template ID in the <strong>Template Variables for:</strong> field of your CLI output. </dd>
</dl>	

**Example:**

```
ibmcloud schematics state pull --id myworkspace-a1aa1a1a-a11a-11 --template a1aa11a1-11a1-11
```
{: pre}



### `ibmcloud schematics workspace state show`
{: #schematics-workspace-show}

Provides the readable output from a state or plan of a workspace as Terraform sees it. You can use to ensure the current state and planned operations are executing as expected. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

```
ibmcloud schematics workspace state show --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETERS>
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to show. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to show. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the address parameter for the command.  </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace show --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep 
```
{: pre}

### `ibmcloud schematics workspace state mv`
{: #schematics-wks_statemv}

Moves an instance or resources from the Terraform state. For example, if you move an instance from the state, the Schematics workspace instance continues running, but `Terrfaorm plan` cannot  see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

```
ibmcloud schematics workspace state mv --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETERS> --destination <DESTINATION>
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to move an instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to move. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the source address parameter for the command.   </dd>

<dt><code>--destination</code></dt>
<dd>Optional. Provide the destination parameter for the command.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace state mv --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep 
```
{: pre}


### `ibmcloud schematics workspace state rm`
{: #schematics-wks_staterm}

Removes an instance or resources from the Terraform state. For example, if you remove an instance from the state, the Schematics workspace instance continues running, but `Terrfaorm plan` cannot see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

```
ibmcloud schematics workspace state rm --id <WORKSPACE_ID> --options <FLAGS> --address <PARAMETER> 
```
{: pre}

</br>
**Command options:**

<dl>
<dt><code>--id <em>WORKSPACE_ID</em></code></dt>
<dd>Required. The unique identifier of the workspace for which you want to remove the instance or resource. To find the ID of your workspace, run <code>ibmcloud schematics workspace list</code>.
   </dd>

<dt><code>--options <em>FLAGS</em></code></dt>
<dd>Optional. Enter the option flag that you want to remove. For more information, about the flags, refer [Command option flags](https://www.terraform.io/docs/commands/untaint.html) </dd>

<dt><code>--address</code></dt>
<dd>Optional. Provide the address parameter for the command.   </dd>

</dl>

**Example:**
```
ibmcloud schematics workspace state rm --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep --destination null_resource.slept 
```
{: pre}

