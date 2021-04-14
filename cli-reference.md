---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-14"

keywords: schematics command line reference, schematics commands, schematics command line, schematics reference, command line

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


# {{site.data.keyword.bplong_notm}} CLI	
{: #schematics-cli-reference}	

Refer to these commands when you want to automate your {{site.data.keyword.bplong_notm}} workspaces and actions.
{: shortdesc}	

To install the CLI, see [Setting up the CLI](/docs/schematics?topic=schematics-setup-cli) and to set up {{site.data.keyword.bplong_notm}} plug-in, see [{{site.data.keyword.bpshort}} plug-in installation](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin)
{: tip}

## General commands
{: #schematics-general-commands}

Use these general commands to find help and version information for the {{site.data.keyword.bplong_notm}} command line plug-in. 
{: shortdesc}

### `ibmcloud schematics help`
{: #schematics-help-cmd}

View the supported {{site.data.keyword.bplong_notm}} command line commands. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics help [command]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--help` or `-h` | Required | Lists the supported commands. |
| `command` | Optional | Specify the name of the command to fetch the command details. |
{: caption="Schematics help flags" caption-side="top"}

**Example**

```
ibmcloud schematics help 
```
{: pre}

### `ibmcloud schematics version`
{: #schematics-version}

List the versions of all supported open source projects in {{site.data.keyword.bpshort}}, such as the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, Ansible, Helm, and Kubernetes that are used to run {{site.data.keyword.bpshort}} actions on {{site.data.keyword.cloud_notm}} resources.
{: shortdesc}

**Syntax**

```
ibmcloud schematics version [--output OUTPUT] [--json JSON_FILE]
```
{: pre}


**Command options** 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--json` or `-j` | Deprecated | Returns the CLI output in JSON format. |
| `--output` or `-o` | Optional | Returns the CLI output in JSON format. Currently only `JSON` file format is supported. |
{: caption="Schematics version flags" caption-side="top"}

**Example**

```
ibmcloud schematics version --output json > "<filename.json>"
```
{: pre}


## Workspace commands	
{: #schematics-workspace-commands}	

Review the commands that you can use to set up and work with your {{site.data.keyword.bplong_notm}} workspace. 
{: shortdesc}

### `ibmcloud schematics workspace action`
{: #schematics-workspace-action}

Retrieve all activities for a workspace, including the user ID of the person who initiated the action, the status, and a timestamp. 
{: shortdesc}

When you create a Terraform execution plan, or apply your Terraform template with {{site.data.keyword.bpshort}}, a {{site.data.keyword.bpshort}} action is automatically created and assigned an action ID. You can use the action ID to retrieve the logs of this action by using the [`ibmcloud schematics logs`](#schematics-logs) command.

**Syntax**

```
ibmcloud schematics workspace action --id WORKSPACE_ID [--act-id ACTION_ID] [--output OUTPUT][--json]
```
{: pre}


**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to retrieve workspace activities. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--act-id` or `-a` | Optional | Enter the ID of a action that you want to retrieve. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
{: caption="Schematics workspace action flags" caption-side="top"}

**Example**
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
		<td>Resources destroyed using command line or resource list, and want to delete workspace</td><td>True</td><td>False</td></tr>
</table> 

**Syntax**

```
ibmcloud schematics workspace delete --id WORKSPACE_ID [--force]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace that you want to remove. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--force` or `-f` | Optional | Force the deletion of your workspace without command line prompts. |
{: caption="Schematics workspace delete flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace delete --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics workspace get`	
{: #schematics-workspace-get}	

Retrieve the details of an existing workspace, including the values of all input variables.	
{: shortdesc}	

**Syntax**

```
ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace, for which you want to retrieve the details. To find the ID of a workspace, run `ibmcloud schematics workspace list` command. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics workspace get flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace get --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}

### `ibmcloud schematics workspace import`
{: #schematics-workspace-import}

You can import the existing resource with an valid address from the workspace ID and import it into your Terraform state. You need to ensure one resource can be imported to only one Terraform resource address. Otherwise, you may see unwanted behavior from {{site.data.keyword.bpshort}}.
{: shortdesc}

**Syntax**

```
ibmcloud schematics workspace import --id WORKSPACE_ID --options OPTIONS --address ADDRESS --resourceID RESOURCE_ID
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to import an instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--options` or `-o` | Required | The command line flags. |
| `--address` or `-adr` | Required | Provide the address of the resource name you want to import.|
| `--resourceID` or `-rid` | Required | Provide the resource ID that you need to import in the file. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics workspace import flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace import --id WID --address ibm_iam_access_group.accgrp --resourceID AccessGroupId-xxxxxx-xxxx-xxx-xxx-xxxx
```
{: pre}


### `ibmcloud schematics workspace list`	
{: #schematics-workspace-list}

List the workspaces in your {{site.data.keyword.cloud_notm}} account and optionally, show the details for your workspace.	

**Syntax**

```
ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--output] [--region] [--json]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer starting from 1, maximum is 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the workspace in the list of workspaces. For example, if you have three workspaces in your account, the command returns these workspaces as a list with three elements. To see a specific workspace in this list, you must enter the position number that the workspace has in the list. To list the first workspace in the list, enter `0`. To list the second workspace, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--region` or `-r` | Optional | Specify the region, such as **eu, us, eu-gb, eu-de, us-south,** or **us-east**.|
{: caption="Schematics workspace list flags" caption-side="top"}

**Example** 

```
ibmcloud schematics workspace list --limit 10 --offset 20 --json
```
{: pre}

### `ibmcloud schematics workspace new`	
{: #schematics-workspace-new}	

Create an {{site.data.keyword.bplong_notm}} workspace that points to your Terraform template in GitHub or GitLab. If you want to provide your Terraform template by uploading a tape archive file (`.tar`), you can create the workspace without a connection to a GitHub repository and then use the [`ibmcloud schematics workspace upload`](#schematics-workspace-upload) command to provide the template.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The location can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

To create a workspace, you must specify your workspace settings in a JSON file. Make sure that the JSON file follows the structure as outlined in this command. 
{: note}

**Syntax**

```
ibmcloud schematics workspace new --file FILE_NAME --state STATE_FILE_PATH [--github-token GITHUB_TOKEN][--output OUTPUT][--json]
```
{: pre}
 
**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--file` or `-f` | Optional | The relative path to a JSON file on your local machine that is used to configure your workspace. For more information, about the sample JSON file with the details, see [JSON file create template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-create-template).|
| `--state` | Optional | The relative path to an existing Terraform statefile on your local machine. To create the Terraform statefile: **1.** Show the content of an existing Terraform statefile by using the [`ibmcloud terraform state pull`](#state-pull) command. **2.** Copy the content of the statefile from your command line output in to a file on your local machine that is named `terraform.tfstate`. **3.** Use the relative path to the file in the `--state` command parameter.|
| `--github-token` or `-g` | Optional |  Enter the functional personal access tokens for HTTPS Git operations. For example, `--github-token ${FUNCTIONAL_GIT_KEY}`.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics workspace create flags" caption-side="top"}

#### Create file template in JSON format
{: #json-file-create-template}

You can create the JSON as shared in the `example.json` file for workspace creation and pass the file path along with the file name in `--file` flag. The description of all the parameters of example.json is described in the table. 

**example.json**

```
{
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
}
```
{: codeblock}

**Example JSON for uploading in a `.tar` file later.**

```
{	
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
```
{: codeblock}

   <table>
    <caption>JSON file component description</caption>
   <thead>
    <th style="width:50px">Parameter</th>
    <th style="width:200px">Required / Optional</th>
    <th style="width:250px">Description</th>
  </thead>
  <tbody>
    <tr>
   <td>`workspace_name`</td>
   <td>Optional</td>
   <td>Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace).</td>
   </tr>
   <tr>
   <td>`terraform_version`</td>
   <td>Optional</td>
   <td>The Terraform version that you want to use to run your Terraform code. Enter `Terraform_v0.12` to use Terraform version 0.12, and similarly terraform_v0.11, terraform_v0.13, terraform_v0.14. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
   </tr>
  <tr>
   <td>`location`</td>
   <td>Optional</td>
   <td>Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} actions run and where your workspace data is stored. If you do not enter a location, {{site.data.keyword.bpshort}} determines the location based on the {{site.data.keyword.cloud_notm}} region that you targeted. To view the region that you targeted, run `ibmcloud target --output json` and look at the `region` field. To target a different region, run `ibmcloud target -r &lt;region&gt;`. If you enter a location, make sure that the location matches the {{site.data.keyword.cloud_notm}} region that you targeted.</td>
   </tr>
   <tr>
   <td>`description`</td>
   <td>Optional</td>
   <td>Enter a description for your workspace.</td>
   </tr>
      <td>`template_repo.url`</td>
   <td>Optional</td>
   <td>Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored.</td>
   </tr>
    <tr>
    <td>`template_repo.branch`</td>
 <td>Optional</td>
  <td>Enter the GitHub or GitLab branch where your Terraform configuration files are stored.  <strong>Note</strong> Now, in template_repo, you can also update url with more parameters as shown in the block. <pre class="codeblock"><code>"url": "https://github.com/IBM-Cloud/terraform-provider-ibm",
     "branch": "master;",
     "datafolder": “examples/ibm-vsi”,
     "release": "v1.8.0"</code></pre></td></tr>
    <tr>
   <td>`template_repo.datafolder`</td>
   <td>Optional</td>
   <td>Enter the GitHub or GitLab branch where your Terraform configuration files are stored.</td>
   </tr>
    <tr>
   <td>`template_repo.release`</td>
   <td>Optional</td>
   <td>Enter the GitHub or GitLab release that points to your Terraform configuration files.</td>
   </tr>
   <tr>
   <td>`github_source_repo_url`</td>
   <td>Optional</td>
   <td>Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. If you choose to create your workspace without a GitHub repository, your workspace is created with a **draft** state. To connect your workspace to a GitHub repository later, you must use the `ibmcloud schematics workspace update` command. If you plan to provide your Terraform template by uploading a tape archive file (`.tar`), leave the URL empty, and use the [ibmcloud schematics workspace upload](#schematics-workspace-upload) command after you created the workspace. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-faqs#clone-file-extension) for cloning.</td>
   </tr>
  <tr>
   <td>`env_values`</td>
   <td>Optional</td>
   <td>A list of environment variables that you want to apply during the execution of a bash script or Terraform action. This field must be provided as a list of key-value pairs. Each entry will be a map with one entry where `key = variable name` and `value = value`. You can define environment variables for {{site.data.keyword.cloud_notm}} catalog offerings that are provisioned by using a bash script files.</td>
   </tr>
   <tr>
   <td>`variable_name`</td>
   <td>Optional</td>
   <td>Enter the name for the input variable that you declared in your Terraform configuration files.</td>
   </tr>
   <tr>
   <td>`variable_type`</td>
   <td>Optional</td>
   <td>`Terraform v0.11` supports `string`, `list`, `map` data type. `Terraform v0.12` additionally, supports `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`.</td>
   </tr>
  <tr>
   <td>`variable_value`</td>
   <td>Optional</td>
   <td>Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information, about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable). **Example** <pre class="codeblock"><code>
       "variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))"
                },
      ]</code></pre></td>
   </tr>
   <tr>
   <td>`secure`</td>
   <td>Optional</td>
   <td>Set the `secure` parameter to **true**. By default, this parameter is set to **false**.</td>
   </tr>
   <tr>
   <td>`val1`</td>
   <td>Optional</td>
   <td>In the payload you can provide an environment variable that can execute in your workspace during plan, apply or destroy stage. Also values are encrypted and stored in COS.</td>
   </tr>
  </tbody></thead></table>

**Example**

```
ibmcloud schematics workspace new --file example.json
```
{: pre}


### `ibmcloud schemates workspace output`
{: #schematics-output}

Displays all the instance or resource output of the workspace. You can provide output `NAME`, to print only the value of that output. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics workspace output --id WORKSPACE_ID --options OPTIONS --name OUTPUT_NAME
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to print an instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list`.|
| `--options` or `-o` | Optional | Enter the command line flags. |
| `--name` or `-n` | Optional | Specify the parameter name to print. |
{: caption="Schematics workspace output flags" caption-side="top"}

**Example**
```
ibmcloud schematics workspace output --id myworkspace-asdff1a1a-42145-11 --name null_resource.sleep  
```
{: pre}


### `ibmcloud schematics refresh`
{: #schematics-refresh}

Perform an {{site.data.keyword.cloud_notm}} refresh action against your workspace. A refresh action validates the {{site.data.keyword.cloud_notm}} resources in your account against the state that is stored in the Terraform statefile of your workspace. If differences are found, the Terraform statefile is updated accordingly. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics refresh --id WORKSPACE_ID [--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that you want to refresh. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics refresh flags" caption-side="top"}

**Example**

```
ibmcloud schematics refresh --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics state list`
{: #state-list}

List the {{site.data.keyword.cloud_notm}} resources that are documented in your Terraform statefile (`terraform.tfstate`).  
{: shortdesc}	

**Syntax**

```
ibmcloud schematics state list --id WORKSPACE_ID
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list the {{site.data.keyword.cloud_notm}} resources that are documented in the Terraform statefile. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
{: caption="Schematics state list flags" caption-side="top"}

**Example**

```
ibmcloud schematics state list --id myworkspace-a1aa1a1a-a11a-11  
```
{: pre}


### `ibmcloud schematics workspace taint`
{: #schematics-workspace-taint}

Manually marks an instance or resources as tainted, by forcing the resources to be recreated on the next apply. Taint modifies the state file, but not the infrastructure in your workspace. When you perform next plan the changes will display as recreated, and in the next apply the change is implemented.
{: shortdesc}

**Syntax**

```
ibmcloud schematics workspace taint --id WORKSPACE_ID [--options FLAGS] [--address PARAMETER]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to recreate the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to show.  |
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as taint.|
{: caption="Schematics workspace taint flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace taint --id myworkspace-lalalalalalala-11 --address null_resource.sleep  
```
{: pre}


### `ibmcloud schematics workspace untaint`
{: #schematics-workspace-untaint}

Manually marks an instance or resources as untainted, by forcing the resources to be restored on the next apply. When you perform next plan the changes will show as restored and in the next apply the change is implemented.
{: shortdesc}

**Syntax**

```
ibmcloud schematics workspace untaint --id WORKSPACE_ID [--options FLAGS] [--address PARAMETER]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to recreate the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to show.  |
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as untaint.|
{: caption="Schematics workspace untaint flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace untaint --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}

### `ibmcloud schematics workspace update`	
{: #schematics-workspace-update}	

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud schematics plan`](#schematics-plan) command.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The region can be **us-east, us-south, eu-gb**, or **eu-de** region. You need to wait before calling the command again.
{: shortdesc}	

If you provided your Terraform template by uploading a tape archive file (`.tar`) and you want to update your template, you must use the [`ibmcloud schematics workspace upload`](#schematics-workspace-upload) command.
{: note}

**Syntax**

```
ibmcloud schematics workspace update --id WORKSPACE_ID --file FILE_NAME [--github-token GITHUB_TOKEN][--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to update the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Optional | The relative path to a JSON file on your local machine that includes the updated parameters for your workspace. For more information, about the sample JSON file with the details, see [JSON file update template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-update-template).|
| `--github-token` or `-g` | Optional |  Enter the GitHub token value to access private Git repository.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics workspace update flags" caption-side="top"}

#### Update file template in JSON format
{: #json-file-update-template}

You can create the JSON as shared in the `example.json` file for workspace update and pass the file path along with the file name in `--file` flag. The description of all the parameters of example.json is described in the table. 

**example.json**

```
{
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
}
```
{: codeblock}

<table>
   <thead>
    <th style="width:50px">Parameter</th>
    <th style="width:200px">Required / Optional</th>
    <th style="width:250px">Description</th>
  </thead>
  <tbody>
   <tr>
   <td>`name`</td>
   <td>Optional</td>
   <td>Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). If you update the name of the workspace, the ID of the workspace does not change. </td>
   </tr>
   <tr>
   <td>`type`</td>
   <td>Optional</td>
   <td>The Terraform version that you want to use to run your Terraform code. Enter `Terraform_v0.14` to use Terraform version 0.14, `Terraform_v0.13` to use Terraform version 0.13, `Terraform_v0.12` to use Terraform version 0.12, and `Terraform_v0.11` to use Terraform version 0.11. Make sure that your Terraform config files are compatible with the Terraform version that you specify.</td>
   </tr>
    <tr>
   <td>`description`</td>
   <td>Optional</td>
   <td>Enter a description for your workspace.</td>
   </tr>
    <tr>
   <td>`tags`</td>
   <td>Optional</td>
   <td>Enter tags that you want to associate with your workspace. Tags can help you find your workspace more easily.</td>
   </tr>
    <tr>
   <td>`resource_group`</td>
   <td>Optional </td>
   <td>Enter the resource group where you want to provision your workspace.</td>
   </tr>
    <tr>
   <td>`workspace_status` </td>
   <td>Optional</td>
   <td>Freeze or unfreeze a workspace. If a workspace is frozen, changes to the workspace are disabled.</td>
   </tr>
    <tr>
   <td>`template_repo.url`</td>
   <td>Optional</td>
   <td>Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored.</td>
   </tr>
    <tr>
    <td>`template_repo.branch`</td>
 <td>Optional</td>
  <td>Enter the GitHub or GitLab branch where your Terraform configuration files are stored.  <strong>Note</strong> Now, in template_repo, you can also update url with more parameters as shown in the block. <pre class="codeblock"><code>"url": "https://github.com/IBM-Cloud/terraform-provider-ibm",
     "branch": "master;",
     "datafolder": “examples/ibm-vsi”,
     "release": "v1.8.0"</code></pre></td></tr>
    <tr>
   <td>`template_repo.datafolder`</td>
   <td>Optional</td>
   <td>Enter the GitHub or GitLab branch where your Terraform configuration files are stored.</td>
   </tr>
    <tr>
   <td>`template_repo.release`</td>
   <td>Optional</td>
   <td>Enter the GitHub or GitLab release that points to your Terraform configuration files.</td>
   </tr>
    <tr>
   <td>`github_source_repo_url`</td>
   <td>Optional</td>
   <td>Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory.</td>
   </tr>
    <tr>
   <td>`template_data.variablestore.name`</td>
   <td>Optional</td>
   <td>Enter the name for the input variable that you declared in your Terraform configuration files.</td>
   </tr>
    <tr>
   <td>`template_data.variablestore.type`</td>
   <td>Optional</td>
   <td>`Terraform v0.11` supports `string`, `list`, `map` data type.  <br> `Terraform v0.12` additionally, supports `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`.</td>
   </tr>
    <tr>
   <td>`template_data.variablestore.value`</td>
   <td>Optional</td>
   <td>Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You can override the default values of `.tfvars` by setting `use_default` parameter as `true`. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information, about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable) <pre class="codeblock"><code>"variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))",
		                "use_default":true
                },</code></pre></td>
   </tr>
    <tr>
   <td>`template_data.variablestore.secure`</td>
   <td>Optional</td>
   <td>Set the `secure` parameter to **true**. By default, this parameter is set to **false**.</td>
   </tr>
    <tr>
   <td>`template_data.variablestore.use_default`</td>
   <td>Optional</td>
   <td>Set the `use_default` parameter to **true** to override the default `.tfvars` parameter. By default, this parameter is set to **false**.</td>
   </tr>
   <tr>
   <td>`env_values.val1`</td>
   <td>Optional</td>
   <td>In the payload you can provide an environment variables, and customized variables that can execute in your workspace during plan, apply or destroy stage. Also values are encrypted and stored in COS.</td>
   </tr>
    <tr>
   <td>`github_source_repo_url`</td>
   <td>Optional</td>
   <td>Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory.</td>
   </tr>
    <tr>
   <td>`github_source_repo_url`</td>
   <td>Optional</td>
   <td>Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory.</td>
   </tr>
  </tbody></thead></table>

**Example**

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

**Syntax**

```
ibmcloud schematics workspace upload  --id WORKSPACE_ID --file FILE_NAME --template TEMPLATE_ID [--output OUTPUT][--json]
```
{: pre}

</br>

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace where you want to upload your tape archive file (`.tar`). To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Required | Enter the full file path on your local machine where your `.tar` file is stored.|
| `--template` or `-tid` | Required |  The unique identifier of the Terraform template for which you want to show the content of the Terraform statefile. To find the ID of the template, run `ibmcloud schematics workspace get --id &lt;workspace_ID&gt;` and find the template ID in the **Template Variables for:** field of your command line output.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics workspace upload flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace upload --id myworkspace-a1aa1a1a-a11a-11 --file /Users/myuser/Documents/mytar/vpc.tar --template 25111111-0000-4c

```
{: pre}

 Create the `TAR` file of your template repo by using the `TAR` command given `tar -cvf vpc.tar $TEMPLATE_REPO_FOLDER`
 {: note}


## {{site.data.keyword.cloud_notm}} resource management commands
{: #schematics-resource-commands}

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}}.

### `ibmcloud schematics apply`	
{: #schematics-apply}	

Scan and run the infrastructure code of your Terraform template that your workspace points to. When you apply a Terraform template, your resources are provisioned, modified, [persisted](/docs/schematics?topic=schematics-faqs#persist-file), or removed in {{site.data.keyword.cloud_notm}}.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}

Your workspace must be in an **Inactive**,  **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} apply action. For more information, about workspace state, see [Workspace state diagram](/docs/schematics?topic=schematics-workspace-setup#workspace-state-diagram).
{: note}

While your infrastructure code runs in {{site.data.keyword.bplong_notm}}, you cannot make any changes to your workspace.
{: important}

**Syntax**

```
ibmcloud schematics apply --id WORKSPACE_ID [--target RESOURCE] [--var-file TFVARS_FILE_PATH] [--force] [--json]
ibmcloud schematics apply --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--var-file PATH_TO_VARIABLES_FILE] [--force] [--output OUTPUT][--json]
```
{: pre}


**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source control repository that you want to apply in {{site.data.keyword.cloud_notm}}. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the creation of a specific resource of your Terraform configuration file by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources that are defined in your configuration file remain uncreated or unupdated. To target the creation of multiple resources, use the following syntax: `--target &lt;resource1&gt; --target &lt;resource2&gt;`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for creation.|
| `--var-file` or `-vf` | Optional |  The file path to the `terraform.tfvars` file that you created on your local machine. Use this file to store sensitive information, such as the {{site.data.keyword.cloud_notm}} API key or credentials to connect to {{site.data.keyword.cloud_notm}} classic infrastructure in the format `&lt;key&gt;=&lt;value&gt;`. All key value pairs that are defined in this file are automatically loaded into Terraform when you initialize the Terraform CLI. To specify multiple `tfvars` files, specify `--var-file TFVARS_FILE_PATH1 --var-file TFVARS_FILE_PATH2`.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics apply flags" caption-side="top"}

**Example**

```
ibmcloud schematics apply --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_instance.vm1 --var-file ./terraform.tfvars
```
{: pre}


### `ibmcloud schematics destroy`	
{: #schematics-destroy}	

Remove the {{site.data.keyword.cloud_notm}} resources that you provisioned with your {{site.data.keyword.bpshort}} workspace, even if these resources are active.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

Use this command with caution. After you run the command, you cannot reverse the removal of your {{site.data.keyword.cloud_notm}} resources. If you use persistent storage, make sure that you created a backup for your data
{: important} 	

Your workspace must be in an **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} destroy action. 
{: note}

**Syntax**

```
ibmcloud schematics destroy --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--force] [--output OUTPUT][--json]
```
{: pre}


**Command options** 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that specifies the {{site.data.keyword.cloud_notm}} resources that you want to remove. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the deletion of a specific resource by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources in your workspace remain unchanged. To target the deletion of multiple resources, use the following syntax: `--target &lt;resource1&gt; --target &lt;resource2&gt;`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for deletion. Also, if the targeted resource can only be deleted if dependent resources are deleted, such as a VPC can only be deleted if the attached subnet is deleted, then all dependent resources are targeted for deletion as well.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics destroy flags" caption-side="top"}

**Example**

```
ibmcloud schematics destroy --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_vpc.myvpc
```
{: pre}

### `ibmcloud schematics logs`	
{: #schematics-logs}	

Retrieve the Terraform log files for a {{site.data.keyword.bpshort}} workspace or a specific action ID. Use the log files to troubleshoot Terraform template issues or issues that occur during the resource provisioning, modification, or deletion process. 
{: shortdesc}	

**Syntax**

```
ibmcloud schematics logs --id WORKSPACE_ID [--act-id ACTION_ID]
```
{: pre}


**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to retrieve Terraform log files. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--act-id` or `-1` | Optional | The ID of an action for which you want to retrieve Terraform logs. To find a list of action IDs, run `ibmcloud schematics workspace action --id WORKSPACE_ID` command. |
{: caption="Schematics logs flags" caption-side="top"}

**Example**

```
ibmcloud schematics logs --id myworkspace-a1aa1a1a-a11a-11 --act-id 9876543121abc1234cdst
```
{: pre}

### `ibmcloud schematics output`
{: #schematics-output2}

Retrieve a list of Terraform output values. You define output values in your Terraform template to include information that you want to make accessible for other Terraform templates.
{: shortdesc}

**Syntax**

```
ibmcloud schematics output --id WORKSPACE_ID[--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list Terraform output values. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics output flags" caption-side="top"}
  
**Example**

```
ibmcloud schematics output --id myworkspace3_2-31cf7130-d0c4-4d
```
{: pre}

### `ibmcloud schematics plan`	
{: #schematics-plan}	

Scan the Terraform template in your source repository and compare this template against the {{site.data.keyword.cloud_notm}} resources that are already deployed. The command line output shows the {{site.data.keyword.cloud_notm}} resources that must be added, modified, [persisted](/docs/schematics?topic=schematics-faqs#persist-file), or removed to achieve the state that is described in your configuration file.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

Your workspace must be in an **Inactive**, **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} plan action. 
{: note}

During the creation of the Terraform execution plan, you cannot make any changes to your workspace. 
{: note}

**Syntax**

```
ibmcloud schematics plan --id WORKSPACE_ID [--output OUTPUT] [--json]
```
{: pre}


**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that you want to scan. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics output flags" caption-side="top"}

**Example**

```
ibmcloud schematics plan --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}


## Action commands
{: #schematics-action-commands}

Review the commands that you want to create, update, list, delete and work with your {{site.data.keyword.bplong_notm}} actions.
{: shortdesc}

### Inventory host groups
{: #inventory-host-grps}

{{site.data.keyword.bplong_notm}} supports inventory host groups to group the applications hostname such as web server, database server, Operating System, region, or network. 

A host group is a collection of hosts that you can run your Ansible playbook against. A condition defines either a workspace or a query within a workspace. For instance, you can run your inventory against all the hosts in your `development` workspace, or against all hosts with a `webserver` tag in your `development` workspace.  The host groups can be defined by using  **Static inventory** or **Dynamic inventory** method. 

**Static inventory** allows to create the collection of hosts that you can run your ansible playbook against. A condition defines either a workspace or a query within a workspace. For instance, you can run your inventory against all the hosts in your `dev` workspace, or against all hosts with a `webserver` tag in your `dev` workspace. You can also add multiple conditional target resources for your workspaces to execute. 

**Dynamic inventory** allows to create the collection of hosts in a inventory file that defines the hosts and group of hosts upon which your playbook operates. The hostnames and IP addresses must be provided in an `hosts.ini` file. Follow the syntax and example for the `INI` file format that can be used in the `create` and `update` actions commands as `--TARGET-FILE <ABSOLUTE_PATH with FILE_NAME>` argument.

  **Syntax**
   ```
    [hostgroupname1]
    <IPaddress1> 
    <IPaddress2> 
    [hostgroupname2]
    <IPaddress1>
   ```
  {: codeblock}

  **Example** 

   ```
    [webserverhost]
    178.54.68.78
    187.54.68.78
    [dbhost]
    174.45.86.87
   ```
    {: codeblock}

  | Target | Description| 
  |------|  ------|
  |`hostgroupname1`| The application hostname. For example, Web Server host application name as `[webserverhost]`, database hostname as `[dbhost]`, in a single word. **Note** System validates and throws an error if a space is provided in the host group name.|
  |`IPaddress`|The IP addresses of the hostname.|
  {: caption="Inventory host group parameters" caption-side="top"}

You can set the proxy between a SSH client and the {{site.data.keyword.cloud_notm}} inventory resources where you want to run an Ansible playbook in the **IBM cloud resource inventory SSH key** field. This set up adds a layer of security to your {{site.data.keyword.cloud_notm}} resources, and minimize the surface of potential vulnerabilities. **Note** Currently {{site.data.keyword.bplong_notm}} actions supports only `one SSH key` for all virtual server instances.
{: note}

### `ibmcloud schematics action create`
{: #schematics-create-action}

Create an {{site.data.keyword.bplong_notm}} action to run an Ansible playbook on a single target host or a group of target hosts. You use Ansible playbooks to perform cloud operations or install software on cloud resources. To try out this capability or to get started, use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. You can create an action by using a payload file or the command's interactive mode.
{: shortdesc}


**Syntax**

```
ibmcloud schematics action create --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--credential CREDENTIAL_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--inventory INVENTORY_ID] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--github-token GITHUB_ACCESS_TOKEN] [--output OUTPUT] [--file FILE_NAME ] [--json] [--no-prompt]
```
{: pre}


**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | A unique name for the action. |
| `--description` or `-d` | Optional | The short description for an action.|
| `--location,` or `-l` | Required | The geography or location where you want to create the action, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`. The geography or location determines where your action runs and where your action data is stored. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). Make sure that you can store data in this location as you cannot change the location after the action is created.|
| `--resource-group` or `-r` | Required | The name of the resource group where you want to create the action. |
| `--template` or `-tr` | Optional | The URL to the Git repository where your Ansible playbook is stored.|
| `--playbook-name or --pn` | Optional| The name of the Ansible playbook. |
| `--credentials` or `-C` | Optional | The file path to the private SSH key that you want to use to access your target host, such as `~/.ssh/id_rsa`.|
| `--bastion` or `-b` | Optional | The IP address of the bastion host.|
| `--inventory` or `-y` | Optional | The ID of the resource inventory that you want to use in your action. To list existing inventories, run `ibmcloud schematics inventory list`. |
| `--input` or `--in` | Optional | The input variables for your action. Input variables must be entered as key-value pairs, such as `--input mykey=myvalue`. To specify multiple input variables, use multiple `--input` flags in your command. You can also store your input variables in a file and reference this file by using the `--input-file` command option.|
|`--input-file` or `--if`|Optional | The path to a file where you specified all your input variables. Input variables must be specified as key-value pairs in JSON format. |
| `--env` or `-e` | Optional | The environment variables for an action. Environment variables must be entered as key-value pairs, such as `--env mykey=myvalue`. To provide multiple environment variables, use multiple `--env` flags in your command.|
| `--env-file` or `-E`| Optional | The path to a file where you specified all environment variables for an action. Environment variables must be specified as key-value pairs in JSON format. |
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--file` or `-f` | Required | The path to the JSON payload file containing the definition of the action that you want to create. For more information, see [Using a payload file](#create-action-payload). |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive mode. |
{: caption="Schematics action create flags" caption-side="top"}

**Example**

```
ibmcloud schematics action create --name start-vsi --location us-south --resource-group default --template https://github.com/Cloud-Schematics/ansible-is-instance-actions --playbook-name stop-vsi-playbook.yml --input instance_ip=172.4.5.0
```
{: pre}

#### Using a payload file
{: #create-action-payload}

Create a JSON file that includes the details for the action that you want to create, such as the ID, name, and description. Then, use the `--file` command option to create your action from your payload file.
{: shortdesc}

**Syntax**

```
{
  "name": "<ACTION_NAME>",
  "description": "<DESCRIPTION>",
  "location": "<LOCATION>",
  "resource_group": "<RESOURCE_GROUP>",
   "source": {
       "source_type" : "git",
       "git" : {
            "git_repo_url": "<YOUR_REPOSITORY>"
       }
  },
  "command_parameter": "<PLAYBOOK_NAME>",
  "tags": [
    "<ACTION_TAGS>"
  ],
  "source_readme_url": "stringtype",
  "source_type": "GitHub"
}
```
{: pre}

```
ibmcloud schematics action create --file <FILE_NAME>
```
{: pre}

**Example**

```
ibmcloud schematics action create --file sample.json
```
{: pre}

#### Using the interactive mode
{: #create-action-interactive}

Instead of entering the command options or using a payload file, you can use the command's interactive mode to create an action. By default, the action is created with minimal user input. To add more information to your action, you can update the action later.  
{: shortdesc}

1. Initiate the interactive mode by running the command without command options. 
   ```
   ibmcloud schematics action create 
   ```
   {: pre}
2. Enter a name for your action and press the return key. 
3. Enter the resource group where you want to create the action and press the return key.
4. Enter the location where you want to create the action, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`. Then, press the return key. The location determines where your action runs and where your action data is stored. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). Make sure that you can store data in this location as you cannot change the location after the action is created.
5. Enter the URL to the GitHub repository where your Ansible playbook is stored. Then, press the return key.
6. If applicable, enter the personal access token that you want to use to access your GitHub repository. Then, press the return key. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-faqs#clone-file-extension) for cloning.
7. Enter the name of the Ansible playbook that you want to run and press the return key. 
8. Review the details of the action that was created for you. 


### `ibmcloud schematics action update`
{: #schematics-update-action}

Update the information of an existing {{site.data.keyword.bplong_notm}} action by using an action ID. 
{: shortdesc}


**Syntax**

```
ibmcloud schematics action update --id ACTION_ID [--name ACTION_NAME] [--description DESCRIPTION] [--template GIT_TEMPLATE_REPO] [--credential CREDENTIAL_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--inventory] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--file FILE_NAME ] [--github-token GITHUB_ACCESS_TOKEN] [--no-prompt] [--output OUTPUT] [--json]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to update. |
| `--name` or `-n` | Optional | A new unique name for your action. |
| `--description` or `-d` | Optional | The short description for an action.|
| `--template` or `-tr` | Optional | The URL to the Git repository where your Ansible playbook is stored.|
| `--credentials` or `-C` | Optional | The file path to the private SSH key that you want to use access your target host, such as `~/.ssh/id_rsa`.|
| `--bastion` or `-b` | Optional | The IP address of the bastion host.|
| `--inventory` or `-y` | Optional | The ID of the resource inventory that you want to use in your action. To list existing inventories, run `ibmcloud schematics inventory list`. |
| `--input` or `--in` | Optional | The input variables for your action. Input variables must be entered as key-value pairs, such as `--input mykey=myvalue`. To specify multiple input variables, use multiple `--input` flags in your command. You can also store your input variables in a file and reference this file in the `--input-file` command option.|
|`--input-file` or `--if`|Optional | The path to a file where you specified all your input variables. Input variables must be specified as key-value pairs in JSON format. |
| `--env` or `-e` | Optional | The environment variables for an action. Environment variables must be entered as key-value pairs, such as `--env mykey=myvalue`. To provide multiple environment variables, use multiple `--env` flags in your command.|
| `--env-file` or `-E`| Optional | The path to a file where you specified all environment variables for an action. Environment variables must be specified as key-value pairs in JSON format. |
| `--file` or `-f` | Optional | Path to the JSON payload file containing the definition of the action to update. For more information, see [Using the payload file](#create-action-payload). Note that parameters, such as the location or resource group cannot be updated after the action is created.|
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-faqs#clone-file-extension) for cloning.|
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="Schematics action update flags" caption-side="top"}


**Example**

```
ibmcloud schematics action update --id us-south.workspace.101010101 --description "This is my description" 
```
{: pre}


### `ibmcloud schematics action get`
{: #schematics-get-action}

Retrieve the detailed information of an existing {{site.data.keyword.bplong_notm}} action by using an action ID. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics action get --id ACTION_ID [--profile PROFILE] [--output OUTPUT] [--json] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
| `--no-prompt` | Optional |Set this flag to run the command without the interactive mode. |
{: caption="Schematics action get flags" caption-side="top"}

**Example**

```
ibmcloud schematics action get --id us-south.workspace.101010101 -p summary 
```
{: pre}

### `ibmcloud schematics action list`
{: #schematics-list-action}

Retrieve a list of all {{site.data.keyword.bpshort}} actions in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics action list [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT] 
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional | The maximum number of actions that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the action in the list of actions from where you want to start listing your actions. For example, if you have three actions in your account, the command returns these actions as a list with three elements. To retrieve all actions, you must enter position number 0. To retrieve actions number 2 and 3 and leave out action number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of actions. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional |The depth of information that is returned. Supported values are `ids`, and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="Schematics action list flags" caption-side="top"}

**Example**

```
ibmcloud schematics action list --profile ids
```
{: pre}

### `ibmcloud schematics action delete`
{: #schematics-delete-action}

Delete a {{site.data.keyword.bpshort}} action. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics action delete --id ACTION_ID [--force][--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="Schematics action delete flags" caption-side="top"}

**Example**

```
ibmcloud schematics action delete --id us-south.workspace.101010101
```
{: pre}


### `ibmcloud schematics action upload`
{: #schematics-upload-action}

You can upload a tape archive file (`.tar`) from your local file system to an {{site.data.keyword.bplong_notm}} action. Enter the full file path on your local machine where your `.tar` file is stored. Create the `.tar` file of your template repo by using the `TAR` command given `tar -cvf mytestactionupload.tar $TEMPLATE_REPO_FOLDER` command.
{: shortdesc}

**Syntax**

```
ibmcloud schematics action upload --id ACTION_ID --file FILE_NAME [--no-prompt] [--output OUTPUT]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | ID of an action that you want to upload. |
| `--file` or `-f` | Required | Path of the `TAR` file to upload for an action.|
| `--no-prompt` | Optional | Set this flag to stop interactive command line session. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="Schematics action upload flags" caption-side="top"}

**Example**

```
ibmcloud schematics action upload --id us.ACTION.testphase1.2eddf83a --file <FILE_PATH>/mytestactionupload.tar
```
{: pre}


## Job commands
{: #schematics-job-commands}

Review the commands to create, update, list, and delete {{site.data.keyword.bplong_notm}} jobs.
{: shortdesc}

### `ibmcloud schematics job run`
{: #schematics-run-job}

Create a job in {{site.data.keyword.bplong_notm}} to run the Ansible playbook in your {{site.data.keyword.bpshort}} action. You can create a job by using a payload file or the command's interactive mode. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics job run --command-object COMMAND_OBJECT_TYPE --command-object-id COMMAND_OBJECT_ID --command-name COMMAND_NAME [--playbook-name PLAYBOOK_NAME] [--command-options COMMAND_OPTIONS] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--output OUTPUT] [--file FILE_NAME ] [--no-prompt]
```
{: pre}


**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--command-object` or `-c` | Required | The name of the {{site.data.keyword.bpshort}} automation resource. Currently, only `action` is supported. |
| `--command-object-id` or `-cid` | Required | The ID of the {{site.data.keyword.bpshort}} action where you want to run the job. |
| `--command-name,` or `-n` | Required | The command that you want to run for your action. Supported values are `ansible_playbook_check`, and `ansible_playbook_run`.|
| `--playbook-name` or `-pn` | Optional | The name of the Ansible playbook that you want to run.  |
| `--command-options` or `-co` | Optional | The command line options for the command.|
| `--input` or `--in` | Optional | The input variables for an action. **Note** This flag can be set multiple times, and must be in a format `--inputs foo=bar`.|
|`--input-file` or `--if`|Optional | Input variables for an action. Provide the JSON file path that contains input variables.|
| `--env` or `-e` | Optional | The environment variables for an Action. **Note** This flag can be set multiple times, and must be in a format `--env-variables foo=bar`.|
| `--env-file` or `-E`| Optional | The environment variables for an action. Provide JSON file path that contains environment variables. |
| `--result-format` or `-f` | Optional |The result response output in JSON format.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of the new job. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="Schematics job run flags" caption-side="top"}

If the action contains the playbook name, you need to add the playbook name, so that the action playbook name will take the precedence. If you need to override the playbook name through the job, then, you have to create an action with the new playbook name.
{: note}

#### Using the payload file
{: #job-run-payload}

You can provide a payload file to specify certain parameters for the `job run` command. Then, you pass the file name to the command by using the `--file` command option.
{: shortdesc}

**Syntax**

```
{
  "command_object": "<COMMAND_OBJECT>",
  "command_object_id": "<COMMAND_OBJECT_ID>",
  "command_name": "<COMMAND_NAME>",
  "command_parameter": "<PLAYBOOK_NAME>"
}
```
{: codeblock}

**Example**

```
{
  "command_object": "action",
  "command_object_id": "us-east.ACTION.Example-11110000011",
  "command_name": "ansible_playbook_check",
  "command_parameter": "site.yml"
}
```
{: codeblock}

```
ibmcloud schematics job run --file sample.json
```
{: pre}


#### Using the interactive mode
{: #job-create-interactive}

Instead of entering your job details by using command options or a payload file, you can use the interactive mode for the command. This mode prompts you to enter the required values to create a job in {{site.data.keyword.bpshort}}. 
{: shortdesc}

1. Enter the command to create the job without any command options. 
   ```
   ibmcloud schematics job run
   ```
   {: pre}

2. When prompted to `Enter command-object>`, enter `action` and press the return key. 
3. When prompted to `Enter command-object-id>`, enter the action ID details and press the return key.
4. When prompted to `Enter command-name>`, enter `ansible_playbook_run` or `ansible_playbook_check`, and press the return key.
5. Review the CLI output for the job that was created for you.


### `ibmcloud schematics job update`
{: #schematics-update-job}

Create a job by copying the settings of an existing job, and run the job in {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics job update --id JOB_ID [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an existing job that you want to copy and run again. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command line session. |
{: caption="Schematics job update flags" caption-side="top"}

**Example**

```
ibmcloud schematics job update --id  us-east.JOB.yourjob_ID_1231 
```
{: pre}

### `ibmcloud schematics job get`
{: #schematics-get-job}

Retrieve the information of an existing {{site.data.keyword.bplong_notm}} job by using a job ID.
{: shortdesc}

**Syntax**

```
ibmcloud schematics job get --id JOB_ID [--profile PROFILE] [--output OUTPUT] [--json] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the job ID that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve job details without an interactive command line session. |
{: caption="Schematics job get flags" caption-side="top"}

**Example**

```
ibmcloud schematics job get --id us-east.JOB.yourjob_ID_1231 --profile detailed
```
{: pre}

### `ibmcloud schematics job list`
{: #schematics-list-job}

Retrieve a list of all {{site.data.keyword.bpshort}} jobs that ran against a target hosts through {{site.data.keyword.bpshort}} action. The job displays a list of jobs with the status as `in_progess`, `success`, or `failed`.
{: shortdesc}

**Syntax**

```
ibmcloud schematics job list --resource-type RESOURCE_TYPE --id RESOURCE_ID [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT] [--all] [--no-prompt]
```
{: pre}

**Command options**

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--resource-type` or `-rt` | Required | The name of the {{site.data.keyword.bpshort}} resource. Only `action` is supported.|
| `--id` or `-i` | Required | The ID of the {{site.data.keyword.bpshort}} action for which you want to list jobs. |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the job in the list of jobs from where you want to start listing your jobs. For example, if you have three jobs in your account, the command returns these jobs as a list with three elements. To retrieve all jobs, you must enter position number 0. To retrieve job number 2 and 3 and leave out job number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of jobs. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional | The depth of information that is returnd. Supported values are `ids` or `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported.|
| `--all` or `-A` | Optional | Lists all the jobs including the {{site.data.keyword.bpshort}}-internal jobs.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command line session. |
{: caption="Schematics job list flags" caption-side="top"}

**Example**

```
ibmcloud schematics job list --resource-type action --id us-south.ACTION.interactive.aaa1a111 --profile ids --output json
```
{: pre}


### `ibmcloud schematics job logs`
{: #schematics-logs-job}

Retrieve the detailed logs of a job that ran for your {{site.data.keyword.bpshort}} action. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics job logs --id JOB_ID [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` | Required | The ID of the job for which you want to retrieve detailed logs.  |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive command line session. |
{: caption="Schematics job logs flags" caption-side="top"}

**Example**

```
ibmcloud schematics job logs --id us-east.JOB.yourjob_ID_1231 
```
{: pre}

### `ibmcloud schematics job delete`
{: #schematics-delete-job}

Delete a job for a {{site.data.keyword.bpshort}} action. 
{: shortdesc}

You cannot delete or stop a running job. To remove a job, you must wait for the job to complete.  
{: note}

**Syntax**

```
ibmcloud schematics job delete --id JOB_ID [--force] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id` | Required | The ID of the job that you want to delete. |
| `--force` or `-f` | Optional | To force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive command line session. |
{: caption="Schematics job delete flags" caption-side="top"}

**Example**

```
ibmcloud schematics job delete --id us-east.JOB.yourjob_ID_1231 
```
{: pre}

## Resource query commands
{: #rq-commands}

Dynamically build resource inventories by using resource queries. Resource queries help you to retrieve your target hosts from existing {{site.data.keyword.bplong_notm}} workspaces. For more information, about resource queries and conditions, see [Creating resource inventories for Schematics actions](/docs/schematics?topic=schematics-inventories-setup).
{: shortdesc}

### `ibmcloud schematics resource query create`
{: #schematics-create-rq}

Create a resource query in {{site.data.keyword.bplong_notm}} that you can use to build your resource inventory. You can create a resource query by using a payload file or the command's interactive mode.
{: shortdesc}

**Syntax**

```
ibmcloud schematics resource-query create --name RESOURCE_QUERY_NAME [--type RESOURCE_QUERY_TYPE] [--query-file QUERY_FILE_PATH] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name` or `-n` | Required | The unique name for a resource query. |
| `--type` or `-t` | Optional | The type of resource that you want to retrieve. Supported values are `vsi`.  |
|`--query-file` or `-f` | Optional | The path to the JSON file where you specified the details of your resource query. To find a list of supported queries, see [Supported resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries). |
| `--file` or `-f` | Optional | The path to the JSON file that specifies the details of the resource query that you want to create. |
| `--output` or `-o` | Optional | Return the command line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the resource query without an interactive command line session. |
{: caption="Schematics resource query create flags" caption-side="top"}

#### Using the payload file
{: #inv-create-payload}

You can provide a payload file to specify certain parameters for the `resource_query create` command. Then, you pass the file name to the command by using the `--file` command option. For a list of supported resource queries, see [Supported resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries).
{: shortdesc} 

**Syntax**

```
[{
   "query_type": "workspaces",
   "query_condition": [
   {
     "name": "workspace-id",
     "value": "<WORKSPACE_ID>",
     "description": "string"
   },
   {
     "name": "resource-name",
     "value": "<RESOURCE_NAME>",
     "description": "string"
   } 
  ]
}]
```
{: codeblock}

**Example**

```
[{
   "query_type": "workspaces",
   "query_condition": [
   {
     "name": "workspace-id",
     "value": "us-east.workspace.ID1231",
     "description": "string"
   },
   {
     "name": "resource-name",
     "value": "tf00vpc-pubpriv-frontend-vsi",
     "description": "string"
   } 
  ]
}]
```
{: codeblock}

```
ibmcloud schematics resource-query create --name myquery --type vsi --query-file queries.json
```
{: pre}


#### Using the interactive mode
{: #inv-create-interactive}

Instead of entering your resource query details by using the command options or a payload file, you can use the interactive mode for the command. This mode prompts you to enter the required values to create a resource query in {{site.data.keyword.bpshort}}. 
{: shortdesc}

1. Enter the command to create the resource query without any command options. 
   ```
   ibmcloud schematics resource-query create 
   ```
   {: pre}

2. Enter a name for your resource query and press the return key.
3. Enter the path to your payload file. For a sample payload file, see [Using the payload file](#inv-create-payload). Then, press the return key.
4. Review the details of the resource query that was created for you. 


### `ibmcloud schematics resource query delete`
{: #schematics-delete-resource-query}

Delete the resource resource query definition by using the resource query ID from the {{site.data.keyword.bplong_notm}} service. Note you can delete the location and region, resource group from where your inventory was created. Also, make sure your IP addresses are in the [allowlist](/docs/schematics?topic=schematics-allowed-ipaddresses). 
{: shortdesc}

**Syntax**

```
ibmcloud schematics resource-query delete --id ID [--force] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of a resource query that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="Schematics resource query delete flags" caption-side="top"}

**Example**

```
ibmcloud schematics resource-query  delete --id us-east.INVENTORY.inventoryid12342
```
{: pre}


### `ibmcloud schematics resource query get`
{: #schematics-get-rq}

Retrieve the information of an existing {{site.data.keyword.bplong_notm}} resource query by using a resource query ID.
{: shortdesc}

**Syntax**

```
ibmcloud schematics resource-query get --id ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the resource query that you want to retrieve.  |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Specify theh output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve a resource query without an interactive command line session. |
{: caption="Schematics resource query get flags" caption-side="top"}

**Example**

```
ibmcloud schematics resource-query get --id us-east.INVENTORY.inventoryid12342
```
{: pre}

### `ibmcloud schematics resource query list`
{: #schematics-list-rq}

Retrieve a list of all {{site.data.keyword.bpshort}} resource queries in your account.
{: shortdesc}

**Syntax**

```
ibmcloud schematics resource-query list [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

**Command options**

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--limit` or `-l` | Optional |  The maximum number of resource queries that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the resource query in the list of resource queries. For example, if you have three resource queries in your account, the command returns these resource queries as a list with three elements. To see a specific resource query in this list, you must enter the position number that the resource query has in the list. To list the first resource query in the list, enter `0`. To list the second resource query, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
{: caption="Schematics resource query list flags" caption-side="top"}

**Example**

```
ibmcloud schematics resource-query list --output listoutput.json
```
{: pre}


### `ibmcloud schematics resource query update`
{: #schematics-update-rq}

Update or replace a resource query creates a copy of an resource query and relaunches an existing resource query by updating the information of an existing {{site.data.keyword.bplong_notm}} resource query.
{: shortdesc}

**Syntax**

```
ibmcloud schematics resource-query update --id ID --name RESOURCE_QUERY_NAME [--type RESOURCE_QUERY_TYPE] [--query-file QUERY_FILE_PATH] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id`  or `-i` | Required | The resource query ID. |
| `--name` or `-n` | Required | The unique name for a resource query. |
| `--type` or `-t` | Optional | The type of the resource query. such as `vsi`|
|`--query-file` or `-f` | Optional | The path to the JSON file containing queries.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of an inventory.|
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the resource query without an interactive command line session. |
{: caption="Schematics resource query update flags" caption-side="top"}

**Example**

```
ibmcloud schematics resource-query  update  --id us-east.INVENTORY.inventory12312 --name inventoryname600 --description "Short description" --location us-east --resource-group Default --resource-query default.RESOURCEQUERY.string.12121
```
{: pre}


## Inventory commands
{: #inv-commands}

Review the command that you want to create, update, list, delete and to work with your {{site.data.keyword.bplong_notm}} inventory.
{: shortdesc}

### `ibmcloud schematics inventory create`
{: #schematics-create-inv}

Create a resource inventory in {{site.data.keyword.bplong_notm}} that you can reference in a {{site.data.keyword.bpshort} action. A resource inventory includes all the target hosts where you want to run an Ansible playbook. You can create an inventory by using a payload file or the interactive mode.
{: shortdesc}

**Syntax**

```
ibmcloud schematics inventory create --name INVENTORY_NAME [--description DESCRIPTION] [--location GEOGRAPHY] [--resource-group RESOURCE_GROUP] [--inventories-ini INVENTORY_INI_FILE] [--resource-query RESOURCE_QUERY_ID] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

You need to pass either `--inventories-ini` file path or `--resource-query` ID for the inventory to use the target host details.
{: note}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name` or `-n` | Required | The unique name of a resource inventory. |
| `--description` or `-d` | Optional | The short description of an inventory. |
| `--location` or `-l` | Optional | The location where you want to store your resource inventory, such as **us-south, us-east, eu-de,** or **eu-gb**. For more information about data storage in {{site.data.keyword.bpshort}}, see [Where is my data stored?](/docs/schematics?topic=schematics-secure-data#pi-location). |
|`resource-group` or `-r`| Optional | The name of the resource group where you want to create the action.|
|`--inventories-ini` or `-y` | Optional |The file path to the resource inventory file where you specified all target hosts. The resource inventory file must be provided in `INI` format. For more information about how to create a static resource inventory file, see [Creating static inventory files](/docs/schematics?topic=schematics-inventories-setup#static-inv). |
|`--resource-query` | Optional |Enter the ID of a resource query that you created. A resource query helps to dynamically build your resource inventory by using the {{site.data.keyword.cloud_notm}} resources that you created with a {{site.data.keyword.bpshort}} workspace. To create a resource query, see the `ibmcloud schematics resource-query create` [command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-rq). To enter multiple resource query IDs, use `--resource-query id1 --resource-query id2`.|
| `--file` or `-f` | Optional |The path to the JSON file where you specified the resource inventory that you want to create.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to create an inventory without an interactive command line session. |
{: caption="Schematics inventory create flags" caption-side="top"}

#### Using the payload file
{: #inv-create-payload}

You can provide a payload file to specify certain parameters for the `inventory create` command. Then, you pass the file name to the command by using the `--file` command option. 
{: shortdesc} 

**Syntax**

```
{
  "name": "<INVENTORY_NAME>",
  "description": "<DESCRIPTION",
  "location": "<GEOGRAPHY>",
  "resource_group": "<RESOURCE_GROUP>",
  "resource_queries": [
  "<RESOURCE_QUERY_ID>"
  ]
}

```
{: codeblock}

**Example**

```
{
  "name": "myinventory",
  "description": "This is the resource inventory for production",
  "location": "us-east",
  "resource_group": "default",
    "resource_queries": [
    "default.RESOURCEQUERY.string.df3d8a47"
  ]

}
```
{: codeblock}

```
ibmcloud schematics inventory create --file inventory.json
```
{: pre}


#### Using the interactive mode
{: #inv-create-interactive}

Instead of entering your inventory details by using the command options or a payload file, you can also use the interactive mode for the command. This mode prompts you to enter the required values to create an inventory in {{site.data.keyword.bpshort}}. 
{: shortdesc}

1. Enter the command to create the inventory without any command options. 
   ```
   ibmcloud schematics inventory create
   ```
   {: pre}
2. Enter a name for your inventory and press the return key.
3. Enter the resource group where you want to create the inventory and press the return key. 
4. Enter the location where you want to create the inventory, such as **us-south**, **us-east**, **eu-de**, or **eu-gb**. Then, press the return key.
5. Review the details of the inventory that was created. 

### `ibmcloud schematics inventory delete`
{: #schematics-delete-inventory}

Delete the resource inventory definition by using the inventory ID from the {{site.data.keyword.bplong_notm}} service. Note you can delete the location and region, resource group from where your inventory was created. Also, make sure your IP addresses are in the [allowlist](/docs/schematics?topic=schematics-allowed-ipaddresses). 
{: shortdesc}

**Syntax**

```
ibmcloud schematics inventory delete --id ACTION_ID [--force][--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an inventory that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="Schematics inventory delete flags" caption-side="top"}

**Example**

```
ibmcloud schematics inventory delete --id us-east.INVENTORY.inventoryid12342
```
{: pre}


### `ibmcloud schematics inventory get`
{: #schematics-get-inv}

Retrieve detailed information of an existing {{site.data.keyword.bplong_notm}} inventory by using the inventory ID.
{: shortdesc}

**Syntax**

```
ibmcloud schematics inventory get --id ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the resource inventory for which you want to list detailed information.  |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve details of an inventory without an interactive command line session. |
{: caption="Schematics inventory get flags" caption-side="top"}

**Example**

```
ibmcloud schematics inventory get --id us-east.INVENTORY.inventoryid12342 --output json
```
{: pre}

### `ibmcloud schematics inventory list`
{: #schematics-list-inv}

Retrieve a list of all {{site.data.keyword.bpshort}} inventories in your account.
{: shortdesc}

**Syntax**

```
ibmcloud schematics inventory list [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

**Command options**

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--limit` or `-l` | Optional |  The maximum number of inventories that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the inventory in the list of inventories. For example, if you have three inventories in your account, the command returns these inventories as a list with three elements. To see a specific inventory in this list, you must enter the position number that the inventory has in the list. To list the first inventory in the list, enter `0`. To list the second inventory, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
{: caption="Schematics job list flags" caption-side="top"}

**Example**

```
ibmcloud schematics inventory list --output json
```
{: pre}


### `ibmcloud schematics inventory update`
{: #schematics-update-inv}

Update or replace an existing resource inventory. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics inventory update  --id ID --name INVENTORY_NAME [--description DESCRIPTION] [--location GEOGRAPHY] [--resource-group RESOURCE_GROUP] [--inventories-ini INVENTORY_INI_FILE] [--resource-query RESOURCE_QUERY_ID] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id`  or `-i` | Required | Enter the ID of a resource inventory that you want to update. |
| `--name` or `-n` | Required | The unique name of an inventory. |
| `--description` or `-d` | Optional | The short description of an inventory. |
| `--location` or `-l` | Optional | The geographic locations supported by IBM Cloud Schematics service, such as **us-south, us-east, eu-de,** or **eu-gb**.|
|`resource-group` or `-r`| Optional | The resource group name for an action.|
|`--inventories-ini` or `-y` | Optional |  File path of `INI` format file that contains the host details.|
|`--resource-query` | Optional |  Pass resource query ID. To pass multiple IDs use `--resource-query id1 --resource-query id2`.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of an inventory.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command line session. |
{: caption="Schematics inventory update flags" caption-side="top"}

**Example**

```
ibmcloud schematics inventory update  --id us-east.INVENTORY.inventory12312 --name inventoryname600 --description "Short description" --location us-east --resource-group Default --resource-query default.RESOURCEQUERY.string.12121  --output OUTPUT
```
{: pre}

## Enable BYOK or KYOK commands
{: kms-commands}

You can use your encryption keys from key management services (KMS), {{site.data.keyword.keymanagementservicelong_notm}}(BYOK), and {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} (KYOK) to encrypt and secure data stored in {{site.data.keyword.bpshort}}. For more information, about how to protect sensitive data in {{site.data.keyword.bpshort}}, see [protecting your sensitive data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#data-storage).
{: shortdesc}


### Prerequisites
{: #key-prerequisites}

The key management system will list the instance that are created from your specific location and region. Following prerequisites are followed to perform the KMS activity.

- You should have your `KYOK`, or `BYOK`. To create a {{site.data.keyword.keymanagementservicelong_notm}} keys, refer to, [create KYOK root key by using UI](/docs/key-protect?topic=key-protect-create-root-keys). To create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} keys, refer to, [create BYOK root key by using UI](/docs/hs-crypto?topic=hs-crypto-create-root-keys).
- You need to [add root key](/docs/key-protect?topic=key-protect-import-root-keys#import-root-key-gui) to {{site.data.keyword.bpshort}} services.
- You need to configure [service to service authorization](/docs/schematics?topic=schematics-secure-data#data-storage) to integrate `BYOK`, and `KYOK` in {{site.data.keyword.bpshort}} service.


KMS setting is a one time settings. You need to open the [support ticket](/docs/get-support?topic=get-support-using-avatar) to update KMS settings.
{: note}

### `ibmcloud schematics kms instance ls`
{: #schematics-kms-list}

Lists all the KMS instances of your {{site.data.keyword.cloud_notm}} account to find your {{site.data.keyword.keymanagementserviceshort}}or {{site.data.keyword.hscrypto}} by using your location  where keys are created and encrypted scheme such as `KYOK`, or `BYOK`. 
{: shortdesc}

**Syntax**

```
ibmcloud schematics kms instances ls --location LOCATION_NAME --scheme ENCRYPTION_SCHEME [--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--scheme` or `-s` | Required | Specify the encryption scheme. Supported values are `KYOK`, or `BYOK`. |
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="Schematics KMS list flags" caption-side="top"}

**Example**

```
ibmcloud schematics kms instances ls -l US -s byok
```
{: pre}

### `ibmcloud schematics kms enable`
{: #schematics-kms-enable}

Enable KMS to encrypt your data in the specific location. For more information, about enabling customer-managed keys for {{site.data.keyword.bpshort}}, see [enabling keys](/docs/schematics?topic=schematics-secure-data#data-storage).

Update the KMS settings for your location, by using your private endpoint, `CRN`, primary `CRK`, and secondary `CRK`. **Note** you can update the KMS settings only once. For example, if you use an API endpoint for a geography, such as `North America`, only that are created in `us-south` or `us-east` are retrieved.
{: shortdesc}

**Syntax**

```
ibmcloud schematics kms enable --location LOCATION_NAME --scheme ENCRYPTION_SCHEME --group RESOURCE_GROUP --primary_name PRIMARY_KMS_NAME --primary_crn PRIMARY_KEY_CRN --primary_endpoint PRIMARY_KMSPRIVATEENDPOINT [--secondary_name SECONDARY_KMS_NAME][--secondary_crn SECONDARY_KEY_CRN] [--secondary_endpoint SECONDARY_KMSPRIVATEENDPOINT] [--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--scheme` or `-s` | Required | Specify the encryption scheme. Supported values are `KYOK`, or `BYOK`. |
| `--group` or `-g` | Required | Specify the resource group name. Default value is `Default`.|
| `--primary_name` or `--pn` | Required |  Specify the primary KMS name.|
| `--primary_crn` or `--pc` | Required |  Specify the primary key CRN name.|
| `--primary_endpoint` or `--pe` | Required |  Specify the primary KMS private endpoint.|
| `--secondary_name` or `--sn` | Optional | Specify the secondary KMS name.|
| `--secondary_crn` or `--sc`| Optional | Specify the secondary key CRN.|
| `--secondary_endpoint` or `--se`|Optional | Specify the secondary KMS private endpoint.|
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
| `--json` or `-j` | Deprecated | Prints the output in the `JSON` format. |
{: caption="Schematics KMS enable flags" caption-side="top"}

**Example**

```
ibmcloud schematics kms enable -l US -s byok -g Default -pn Key-Protect-south -pc crn:v1:bluemix:public:kms:us-south:lalalalal -pe https://private.us-south.kms.cloud.ibm.com
```
{: pre}


### `ibmcloud schematics kms info`
{: #schematics-kms-info}

Retrieve the KMS on the API endpoint that you have your `KYOK`, or `BYOK`. For example, if you use an API endpoint for a geography, such as `North America`, only that are created in `us-south` or `us-east` are retrieved. **Note** you need to enable `kms instances` in your account to run `info` command line.
{: shortdesc}

**Syntax**

```
ibmcloud schematics kms info --location LOCATION_NAME [--output OUTPUT][--json]
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the Schematics location name. Supported values are `US`, or `EU`. |
| `--output` or `-o` | Optional | Return the command line output in JSON format.Currently only `JSON` file format is supported.|
| `--json` or `-j` | Deprecated | Prints the output in the `JSON` format. |
{: caption="Schematics KMS information flags" caption-side="top"}

**Example**

```
ibmcloud schematics kms info -l US 
```
{: pre}


## Terraform commands
{: #tf-cmds}

You can run a bunch of Terraform commands and manipulate the {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}} API or CLI. The Schematics provides one generic API `commands` for each sub-command.
{: shortdesc}

You can see the `Commands` UI support only to display the state of the workspace. The complete commands support to be released shortly.
{: important}

The table provides the summary of supported commands by the `commands` API.

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

The `Commands` API executes one or group of Terraform commands by using the JSON file for your workspace command requirements. The access control such as `plan`, `apply`, `destroy`, or `refresh` are applicable for `Commands API`. Select your region where the workspace is created, and use the following syntax to run the commands API.

**Syntax**

```
ibmcloud schematics workspace commands --id WORKSPACE_ID --file FILE_NAME 
```
{: pre}

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace where you want to run the commands. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--file` or `--f` | Required | Path to the `JSON` file containing the list of Terraform commands.|
{: caption="Schematics Terraform commands flags" caption-side="top"}


  **Sample payload of Test.JSON file**
  
  ```
  {
        "commands": [
        {
            "command": "state show",
            "command_params": "data.template_file.test",
            "command_name": "Test1",
            "command_desc": "Showing state",
            "command_onerror": "continue"
        },
        {
            "command": "taint",
            "command_params": "null_resource.sleep",
            "command_name": "Test2",
            "command_desc": "Marking taint",
            "command_onerror": "continue"
        },
        {
            "command": "untaint",
            "command_params": "null_resource.sleep",
            "command_name": "Test3",
            "command_desc": "Marking untaint",
            "command_onerror": "continue"
        },
        {
            "command": "state list ",
            "command_params": "",
            "command_name": "Test4",
            "command_desc": "Checking state list",
            "command_onerror": "continue"
        },
        {
            "command": "state rm ",
            "command_params": "data.template_file.test",
            "command_name": "Test5",
            "command_desc": "Removing state",
            "command_onerror": "continue"
        }
    ],
    "operation_name": "Workspace Command",
    "description": "Executing command"
   }
  ```
  {: codeblock}


  The table provides the list of key parameters of the JSON file for the `Commands` API, for the command line and the API.

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
ibmcloud schematics workspace commands --id cli-sleepy-0bedc51f-c344-50 --file /<FILE_PATH>/Test.JSON
```
{: pre}

## Terraform statefile commands
{: #statefile-cmds}

Review the commands that you can use to work with the Terraform statefile (`terraform.tfstate`) for a workspace.
{: shortdesc}

You can import an existing Terraform statefile during the creation of your workspace. For more information, see the [`ibmcloud workspace new`](#schematics-workspace-new) command. 
{: note}

### `ibmcloud schematics state pull`
{: #state-pull}

Show the content of the Terraform statefile (`terraform.tfstate`) for a specific Terraform template of your workspace.  
{: shortdesc}	

**Syntax**

```
ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID
```
{: pre}

</br>

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace where you want to run the commands. |
| `--template` or `--tid` | Required | The unique identifier of the Terraform template for which you want to show the content of the Terraform statefile. To find the ID of the template, run `ibmcloud schematics workspace get --id &lt;workspace_ID&gt;` and find the template ID in the **Template Variables for:** field of your command line output. |
{: caption="Schematics state pull flags" caption-side="top"}

**Example**

```
ibmcloud schematics state pull --id myworkspace-a1aa1a1a-a11a-11 --template a1aa11a1-11a1-11
```
{: pre}


### `ibmcloud schematics workspace state show`
{: #schematics-workspace-show}

Provides the readable output from a state or plan of a workspace as Terraform sees it. You can use to ensure the current state and planned operations are executing as expected. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

**Syntax**

```
ibmcloud schematics workspace state show [--options OPTIONS] --address ADDRESS
```
{: pre}

</br>

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace where you want to update. |
| `--options` or `-o` | Optional | Enter the command line flags. |
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as taint.|
{: caption="Schematics state pull flags" caption-side="top"}

**Example**
```
ibmcloud schematics workspace show --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep 
```
{: pre}

### `ibmcloud schematics workspace state mv`
{: #schematics-wks_statemv}

Moves an instance or resources from the Terraform state. For example, if you move an instance from the state, the Schematics workspace instance continues running, but `Terrfaorm plan` cannot  see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

```
ibmcloud schematics workspace state mv --id WORKSPACE_ID [--source SOURCE]  [--destination DESTINATION] 
```
{: pre}

</br>

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique ID of the workspace for which you want to move an instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--source` or `-s` | Required | Enter the source address of an item to move.|
| `--destination` or `-d` | Required | Provide the destination address of an item.|
{: caption="Schematics state move flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace state mv --id myworkspace-a1aa1a1a-a11a-11 -s testsourceresource -d null_resource.sleep 
```
{: pre}


### `ibmcloud schematics workspace state rm`
{: #schematics-wks_staterm}

Removes an instance or resources from the Terraform state. For example, if you remove an instance from the state, the Schematics workspace instance continues running, but `Terrfaorm plan` cannot see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](#schematics-logs) command.
{: shortdesc}

```
ibmcloud schematics workspace state rm --id WORKSPACE_ID [--options FLAGS] [--address PARAMETER] 
```
{: pre}

</br>

**Command options**

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to remove the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to remove. |
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as taint.|
{: caption="Schematics state remove flags" caption-side="top"}

**Example**

```
ibmcloud schematics workspace state rm --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep --destination null_resource.slept 
```
{: pre}


