---

copyright:
  years: 2017, 2025
lastupdated: "2025-07-30"

keywords: schematics command-line reference, schematics commands, schematics command-line, schematics reference, command-line

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.bplong_notm}} CLI
{: #schematics-cli-reference}

Run these commands to work with {{site.data.keyword.bplong_notm}} workspaces, actions, provisioned resources and configure {{site.data.keyword.bpshort}}.
{: shortdesc}	

{{site.data.keyword.bpshort}} CLI commands are region specific. They operate only in the region/location that the {{site.data.keyword.cloud_notm}} CLI is configured to work in. Ensure the CLI `location` and the `url` endpoint are pointing to the region where you want to create or update your workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). {: note}

To run {{site.data.keyword.bpshort}} commands, use `ibmcloud schematics` or `ibmcloud sch`. {: tip}

## Prerequisites
{: #cli-prerequisites}

- Set up your [CLI](/docs/schematics?topic=schematics-setup-cli).
- Install [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).

Be sure to keep your CLI up-to-date so that you can use the current released commands and their options. For more information about the current command-line version releases, see [Command-line version history](/docs/schematics?topic=schematics-schematics-cli-reference#cli_version-releases).
{: important}

## Actions commands
{: #schematics-action-commands}

Review the commands to create, update, list, delete, and work with your {{site.data.keyword.bpshort}} actions.
{: shortdesc}



### `ibmcloud schematics action create`
{: #schematics-create-action}

Create an action to run an Ansible playbook on a single target host or a group of target hosts. You use Ansible playbooks to perform cloud operations or install software on cloud resources. To try out this capability or to get started, use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. You can create an action by using a payload file or the command's interactive mode.
{: shortdesc}

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

Syntax

```sh
ibmcloud schematics action create --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--credential CREDENTIAL_FILE] [--credential-json CREDENTIAL_JSON_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--bastion-credential-json BASTION_CREDENTIAL_JSON_FILE] [--inventory INVENTORY_ID] [—-inventory-connection-type INVENTORY_CONNECTION_TYPE] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--github-token GITHUB_ACCESS_TOKEN] [--output OUTPUT] [--file FILE_NAME ] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | A unique name for the action. |
| `--description` or `-d` | Optional | The short description for an action.|
| `--location` or `-l` | Required | The geography or location where you want to create the action, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`. The geography or location determines where your action runs and where your action data is stored. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). Make sure that you can store data in this location as you cannot change the location after the action is created.|
| `--resource-group` or `-r` | Required | The name of the resource group where you want to create the action. |
| `--template` or `-tr` | Optional | The URL to the Git repository where your Ansible playbook is stored.|
| `--playbook-name` or `--pn` | Optional| The name of the Ansible playbook. |
| `--credentials` or `-C` | Optional | The file path to the private SSH key that you want to use to access your target host, such as `~/.ssh/id_rsa`. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls.|
| `--credential-json` or `--cj` | Optional | Provide path of JSON file that contains credential JSON payload to access the target host. |
| `--bastion` or `-b` | Optional | The IP address of the bastion host.|
| `--bastion-credential-json` or `--bj` | Optional | Provide path of JSON file that contains bastion credential JSON payload to access the bastion host.|
| `--inventory` or `-y` | Optional | The ID of the resource inventory that you want to use in your action. To list existing inventories, run `ibmcloud schematics inventory list`. |
| `--inventory-connection-type` or `--it` | Optional | Type of inventory connection. Supported values are  `ssh`, or `winrm`. Currently, WinRM supports only Windows system with the public `IPs` and do not support Bastion host.|
| `--input` or `--in` | Optional | The input variables for your action. Input variables must be entered as key-value pairs, such as `--input mykey=myvalue`. To specify multiple input variables, use multiple `--input` flags in your command. You can also store your input variables in a file and reference this file by using the `--input-file` command option.|
|`--input-file` or `--if`|Optional | The path to a file where you specified all your input variables. Input variables must be specified as key-value pairs in JSON format. |
| `--env` or `-e` | Optional | The environment variables for an action. Environment variables must be entered as key-value pairs, such as `--env mykey=myvalue`. To provide multiple environment variables, use multiple `--env` flags in your command.|
| `--env-file` or `-E`| Optional | The path to a file where you specified all environment variables for an action. Environment variables must be specified as key-value pairs in JSON format. |
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--file` or `-f` | Optional | The path to the JSON payload file containing the definition of the action that you want to create. For more information, see [Using a payload file](/docs/schematics?topic=schematics-schematics-cli-reference#create-action-payload). |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive mode. |
{: caption="{{site.data.keyword.bpshort}} actions create flags" caption-side="top"}

Example

```sh
ibmcloud schematics action create --name start-vsi --location us-south --resource-group default --template https://github.com/Cloud-Schematics/ansible-is-instance-actions --playbook-name stop-vsi-playbook.yml --input instance_ip=172.4.5.0
```
{: pre}


#### Using a payload file
{: #create-action-payload}

Create a JSON file that includes the details for the action that you want to create, such as the ID, name, and description. Then, use the `--file` command option to create your action from your payload file.
{: shortdesc}

You need to replace the `<...>` placeholders with the actual values. For example, `"<ACTION_NAME>"` as `"testaction"`.
{: note}

Syntax

```json
{
    "name": "<ACTION_NAME>",
    "description": "<DESCRIPTION>",
    "location": "<LOCATION>",
    "resource_group": "<RESOURCE_GROUP>",
    "bastion_connection_type": "ssh",
    "inventory_connection_type": "winrm", 
    "source": {
        "source_type" : "git",
        "git" : {
            "git_repo_url": "<YOUR_REPOSITORY>"
        }
    },
    "command_parameter": "<PLAYBOOK_NAME>",
    "bastion": {},
    "bastion_credentials": {
	    "metadata": {}
    },
    "tags": [
        "<ACTION_TAGS>"
    ],
    "source_readme_url": "stringtype",
    "source_type": "GitHub"
}
```
{: pre}

```sh
ibmcloud schematics action create --file <FILE_NAME>
```
{: pre}

Example

```sh
ibmcloud schematics action create --file sample.json
```
{: pre}

#### Using the interactive mode
{: #create-action-interactive}

Instead of entering the command options or using a payload file, you can use the command's interactive mode to create an action. By default, the action is created with minimal user input. To add more information to your action, you can update the action later. 
{: shortdesc}

1. Initiate the interactive mode by running the command without command options. 
    ```sh
    ibmcloud schematics action create 
    ```
    {: pre}

2. Enter a name for your action and press the return key. 
3. Enter the resource group where you want to create the action and press the return key.
4. Enter the location where you want to create the action, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`. Then, press the return key. The location determines where your action runs and where your action data is stored. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). Make sure that you can store data in this location as you cannot change the location after the action is created.
5. Enter the URL to the GitHub repository where your Ansible playbook is stored. Then, press the return key.
6. If applicable, enter the personal access token that you want to use to access your GitHub repository. Then, press the return key. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning.
7. Enter the name of the Ansible playbook that you want to run and press the return key. 
8. Review the details of the action that was created for you. 


### `ibmcloud schematics action update`
{: #schematics-update-action}

Update the information of an existing action the using the `action_id`. Ensure the CLI `location` and the `url` endpoint are pointing to the region where you want to create or update your workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}

Syntax

```sh
ibmcloud schematics action update --id ACTION_ID --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--github-token GITHUB_ACCESS_TOKEN] [--credential CREDENTIAL_FILE] [--credential-json CREDENTIAL_JSON_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--bastion-credential-json BASTION_CREDENTIAL_JSON_FILE] [--inventory INVENTORY_ID] [--inventory-connection-type INVENTORY_CONNECTION_TYPE] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--file FILE_NAME] [--no-prompt] [--output OUTPUT]
```
{: pre}
 
Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to update. |
| `--name` or `-n` | Optional | A new unique name for your action. |
| `--description` or `-d` | Optional | The short description for an action.|
| `--location` or `-l` | Required | Geographic locations supported by {{site.data.keyword.bplong_notm}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`.|
| `--resource-group` or `-r` | Required | Resource-group name for an action.|
| `--template` or `-tr` | Optional | The URL to the Git repository where your Ansible playbook is stored.|
| `--playbook-name` or  `--pn` | Optional | Name of the playbook.|
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning.|
| `--credentials` or `-C` | Optional | The file path to the private SSH key that you want to use access your target host, such as `~/.ssh/id_rsa`. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls.|
| `--credential-json` or `--cj` | Optional | Provide path of JSON file that contains credential JSON payload to access the target host. |
| `--bastion` or `-b` | Optional | The IP address of the bastion host.|
| `--bastion-credential-json` or `--bj` | Optional | Provide path of JSON file that contains bastion credential JSON payload to access the bastion host.|
| `--inventory` or `-y` | Optional | The ID of the resource inventory that you want to use in your action. To list existing inventories, run `ibmcloud schematics inventory list`. |
| `--inventory-connection-type` or `--it`| Optional | Type of inventory connection. Supported values are `ssh`, or `winrm`. Currently, WinRM supports only Windows system with the public `IPs` and do not support Bastion host.|
| `--input` or `--in` | Optional | The input variables for your action. Input variables must be entered as key-value pairs, such as `--input mykey=myvalue`. To specify multiple input variables, use multiple `--input` flags in your command. You can also store your input variables in a file and reference this file in the `--input-file` command option.|
|`--input-file` or `--if`|Optional | The path to a file where you specified all your input variables. Input variables must be specified as key-value pairs in JSON format. |
| `--env` or `-e` | Optional | The environment variables for an action. Environment variables must be entered as key-value pairs, such as `--env mykey=myvalue`. To provide multiple environment variables, use multiple `--env` flags in your command.|
| `--env-file` or `-E`| Optional | The path to a file where you specified all environment variables for an action. Environment variables must be specified as key-value pairs in JSON format. |
| `--file` or `-f` | Optional | Path to the JSON payload file containing the definition of the action to update. For more information, see [Using the payload file](/docs/schematics?topic=schematics-schematics-cli-reference#create-action-payload). Note that parameters, such as the location or resource group cannot be updated after the action is created.|
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} actions update flags" caption-side="top"}


Example

```sh
ibmcloud schematics action update --id us-south.workspace.101010101 --description "This is my description" 
```
{: pre}


### `ibmcloud schematics action get`
{: #schematics-get-action}

Retrieve the details of an existing {{site.data.keyword.bpshort}} Action such as action ID, Name, Status, Creation Time, Encryption status, and Encryption CRN, including the values of all the input variables.
{: shortdesc}

Syntax

```sh
ibmcloud schematics action get --id ACTION_ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--no-prompt` | Optional |Set this flag to run the command without the interactive mode. |
{: caption="{{site.data.keyword.bpshort}} actions get flags" caption-side="top"}

Example

```sh
ibmcloud schematics action get --id us-south.workspace.101010101 -p summary 
```
{: pre}

### `ibmcloud schematics action list`
{: #schematics-list-action}

Retrieve a list of all actions defined in the current {{site.data.keyword.cloud_notm}} region for your account. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics action list [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT] 
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional | The maximum number of actions that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the action in the list of actions from where you want to start listing your actions. For example, if you have three actions in your account and region, the command returns these actions as a list with three elements. To retrieve all actions, you must enter position number 0. To retrieve actions number 2 and 3 and leave out action number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of actions. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional |The depth of information that is returned. Supported values are `ids`, and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} actions list flags" caption-side="top"}

Example

```sh
ibmcloud schematics action list --profile ids
```
{: pre}

### `ibmcloud schematics action delete`
{: #schematics-delete-action}

Delete a {{site.data.keyword.bpshort}} action. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics action delete --id ACTION_ID [--force][--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="{{site.data.keyword.bpshort}} actions delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics action delete --id us-south.workspace.101010101
```
{: pre}


### `ibmcloud schematics action upload`
{: #schematics-upload-action}

You can upload a tape archive file (`.tar`) from your local file system to an {{site.data.keyword.bplong_notm}} action. Enter the full file path on your local machine where your `.tar` file is stored. Create the `.tar` file of your template repo by using the `TAR` command given `tar -cvf mytestactionupload.tar $TEMPLATE_REPO_FOLDER` command.
{: shortdesc}

Syntax

```sh
ibmcloud schematics action upload --id ACTION_ID --file FILE_NAME [--no-prompt] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | ID of an action that you want to upload. |
| `--file` or `-f` | Required | Path of the `TAR` file to upload for an action.|
| `--no-prompt` | Optional | Set this flag to stop interactive command-line session. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} actions upload flags" caption-side="top"}

Example

```sh
ibmcloud schematics action upload --id us.ACTION.testphase1.2eddf83a --file <FILE_PATH>/mytestactionupload.tar
```
{: pre}

## Actions Job commands
{: #schematics-job-commands}

Review the commands to create, update, list, and delete {{site.data.keyword.bpshort}} jobs when working with {{site.data.keyword.bpshort}} actions.
{: shortdesc}

### `ibmcloud schematics job run`
{: #schematics-run-job}

Create a job in {{site.data.keyword.bplong_notm}} to execute the Ansible playbook specified by your {{site.data.keyword.bpshort}} action. You can create a job by using a payload file or the command's interactive mode. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics job run --command-object COMMAND_OBJECT_TYPE --command-object-id COMMAND_OBJECT_ID --command-name COMMAND_NAME [--playbook-name PLAYBOOK_NAME] [--command-options COMMAND_OPTIONS] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--output OUTPUT] [--file FILE_NAME ] [--no-prompt]
```
{: pre}


Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--command-object` or `-c` | Required | The name of the {{site.data.keyword.bpshort}} automation resource. Currently, only `action` is supported. |
| `--command-object-id` or `-cid` | Required | The ID of the {{site.data.keyword.bpshort}} actions where you want to run the job. |
| `--command-name,` or `-n` | Required | The command that you want to run for your action. Supported values are `ansible_playbook_check`, and `ansible_playbook_run`.|
| `--playbook-name` or `-pn` | Optional | The name of the Ansible playbook that you want to run. |
| `--command-options` or `-co` | Optional | The command-line options for the command.|
| `--input` or `--in` | Optional | The input variables for an action. This flag can be set multiple times, and must be in a format `--inputs test=testvalue`.|
|`--input-file` or `--if`|Optional | Input variables for an action. Provide the JSON file path that contains input variables.|
| `--env` or `-e` | Optional | The environment variables for an action. This flag can be set multiple times, and must be in a format `--env-variables test=testvalue`.|
| `--env-file` or `-E`| Optional | The environment variables for an action. Provide JSON file path that contains environment variables. |
| `--result-format` or `-f` | Optional |The result response output in JSON format.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of the new job. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} job run flags" caption-side="top"}

If the action contains the playbook name, you need to add the playbook name, so that the action playbook name takes the precedence. If you need to override the playbook name through the job, then, you have to create an action with the new playbook name.
{: note}

#### Using the payload file
{: #job-run-payload}

You can provide a payload file to specify certain parameters for the `job run` command. Then, you pass the file name to the command by using the `--file` command option.
{: shortdesc}

You need to replace the `<...>` placeholders with the actual values. For example, `"<COMMAND_OBJECT>"` as "action".
{: note}

Syntax

```json
{
    "command_object": "<COMMAND_OBJECT>",
    "command_object_id": "<COMMAND_OBJECT_ID>",
    "command_name": "<COMMAND_NAME>",
    "command_parameter": "<PLAYBOOK_NAME>"
}
```
{: codeblock}

Example

```json
{
    "command_object": "action",
    "command_object_id": "us-east.ACTION.Example-11110000011",
    "command_name": "ansible_playbook_check",
    "command_parameter": "site.yml"
}
```
{: codeblock}

```sh
ibmcloud schematics job run --file sample.json
```
{: pre}


#### Using the interactive mode
{: #job-create-interactive}

Instead of entering your job details by using command options or a payload file, you can use the interactive mode for the command. This mode prompts you to enter the required values to create a job in {{site.data.keyword.bpshort}}. 
{: shortdesc}

1. Enter the command to create the job without any command options. 
    ```sh
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

Syntax

```sh
ibmcloud schematics job update --id JOB_ID [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an existing job that you want to copy and run again. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} job update flags" caption-side="top"}

Example

```sh
ibmcloud schematics job update --id  us-east.JOB.yourjob_ID_1231 
```
{: pre}

### `ibmcloud schematics job get`
{: #schematics-get-job}

Retrieve the details of an actions job using a job ID.
{: shortdesc}

Syntax

```sh
ibmcloud schematics job get --id JOB_ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the job ID that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve job details without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} job get flags" caption-side="top"}

Example

```sh
ibmcloud schematics job get --id us-east.JOB.yourjob_ID_1231 --profile detailed
```
{: pre}

### `ibmcloud schematics job list`
{: #schematics-list-job}

Retrieve a list of all {{site.data.keyword.bpshort}} jobs that ran for a {{site.data.keyword.bpshort}} Action. The command displays a list of jobs with the status as `in_progress`, `success`, or `failed`.
{: shortdesc}

Syntax

```sh
ibmcloud schematics job list --resource-type RESOURCE_TYPE --id RESOURCE_ID [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT] [--all] [--no-prompt]
```
{: pre}

Command options

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--resource-type` or `-rt` | Required | The name of the {{site.data.keyword.bpshort}} resource. Only `action` is supported.|
| `--id` or `-i` | Required | The ID of the {{site.data.keyword.bpshort}} actions for which you want to list jobs. |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the job in the list of jobs from where you want to start listing your jobs. For example, if you have three jobs in your account, the command returns these jobs as a list with three elements. To retrieve all jobs, you must enter position number 0. To retrieve job number 2 and 3 and leave out job number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of jobs. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional | The depth of information that is returned. Supported values are `ids` or `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--all` or `-A` | Optional | Lists all the jobs including the {{site.data.keyword.bpshort}} internal jobs.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} job list flags" caption-side="top"}

Example

```sh
ibmcloud schematics job list --resource-type action --id us-south.ACTION.interactive.aaa1a111 --profile ids --output json
```
{: pre}


### `ibmcloud schematics job logs`
{: #schematics-logs-job}

Retrieve the logs for a {{site.data.keyword.bpshort}} action job. For more information about viewing job logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-interrupt-job#sch-job-logs).
{: shortdesc}

Syntax

```sh
ibmcloud schematics job logs --id JOB_ID [log-prefix] [log-header] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The ID of the job for which you want to retrieve detailed logs. |
| `--log-prefix` or `--lp` | Optional | Adds the prefix of command executed in the job logs. |
| `--log-header` or `--lh` | Optional |  Used to convert command headers in the job logs in the {{site.data.keyword.bpshort}} format. |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} job logs flags" caption-side="top"}

Example

```sh
ibmcloud schematics job logs --id us-east.JOB.yourjob_ID_1231 
```
{: pre}

### `ibmcloud schematics job delete`
{: #schematics-delete-job}

Delete a job for a {{site.data.keyword.bpshort}} action. 
{: shortdesc}

You cannot delete or stop a running job. To remove a job, you must wait for the job to complete. 
{: note}

Syntax

```sh
ibmcloud schematics job delete --id JOB_ID [--force] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id` or `-i` | Required | The ID of the job that you want to delete. |
| `--force` or `-f` | Optional | To force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} job delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics job delete --id us-east.JOB.yourjob_ID_1231 
```
{: pre}

## Agents commands
{: #agents-cmd}


### `ibmcloud schematics agent create`
{: #schematics-agent-create}

Create an agent registration in the currently selected {{site.data.keyword.bpshort}} region. Agents help you run your Terraform or Ansible jobs on your infrastructure. For more information about the steps to use the create command, see [deploying agents](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent create --name AGENT_NAME --location LOCATION --agent-location AGENT_LOCATION --cluster-id CLUSTER_ID --cluster-resource-group CLUSTER_RESOURCE_GROUP --cos-instance-name COS_INSTANCE_NAME --cos-bucket COS_BUCKET --cos-location COS_LOCATION --resource-group RESOURCE_GROUP [--version VERSION] [--infra-type INFRA_TYPE] [--description DESCRIPTION] [--tags TAGS] [--metadata AGENT_METADATA] [--validate] [--deploy] [--file FILE] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | The unique name of an agent. Must be descriptive of the agent role, location and usage.  |
| `--location` or `-l` | Required | The {{site.data.keyword.bpshort}} location where the agent are defined, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for execution. |
| `--agent-location` or `--al` | Required | A descriptive user defined label to identify where the agent is deployed in the user environment. This could be a Cloud region or a user data center.  For example, `London MZR`. |
| `--cluster-id` or `-c` | Required | The ID of the Kubernetes cluster for deploying an Agent.|
| `--cluster-resource-group` or `--cg` | Required | The name of the clusters' resource group. |
| `--cos-instance-name` or `--on` | Required | The name of the COS instance. |
| `--cos-bucket` or `-b` | Required |  The ID or the name of the COS bucket. |
| `--cos-location` or `--ol` | Required |  The COS bucket location. Supported format are `eu-gb`, `us-south`, so on. |
| `--resource-group` or `-g` | Required | Resource group name or ID the agent are associated with. |
| `--version` or `-v` | Required | A user defined label specifying the version of the agent. Example `v1.0.0` |
| `--infra-type` or `-i` | Required | Specify the type of the target agent infrastructure. Supported values are `ibm-kubernetes`, `ibm-openshift`, or `ibm-satellite`.|
| `--description` or `-d` | Optional | A description that identifies the agent usage, and the network zones and resources the agent is able to access. |
| `--tags` or `-t`| Optional | Agent tags. You can repeat the flag multiple times. Tags allow for faster and easier search for agent related resources. |
|  `--metadata` or `--md` | Optional | Metadata of the agent. You can use the flag multiple times. For example, `git:private-git.github.com` or `git:gitlab.com`. If not set, defaults to `git:github.com`.|
| `--validate` | Optional | Run validate, after creating the agent.|
| `--deploy` | Optional | Run deploy without validating, after creating the agent.|
| `--file` or `f` | Optional | Path to a `JSON` file containing the definition of an agent. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent create flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent create --name agenttestcli10jan --location us-east --agent-location us-east --version 1.0.0-prega --infra-type ibm_kubernetes --cluster-id clbjrdml00cgremot1k0 --cluster-resource-group Default --cos-instance-name agent-test-cos-standard --cos-bucket agent-test-bucket --cos-location us-east --resource-group Default --description "This agent is created to test for the prod release and COS"
```
{: pre}


### `ibmcloud schematics agent delete`
{: #schematics-agent-delete}

Uninstall an agent. For more information about the steps to use the delete command, see [deleting an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent delete --id AGENT_ID [--force]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an agent. |
| `--force` or `-f` | Optional | The force action without confirmation. Set the `--force` parameter to **true** to delete all the agent flows to keep destroy parallel to workspace destroy flow. By default, this parameter is set to **false**.|
{: caption="{{site.data.keyword.bpshort}} Agent delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent delete --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent deploy`
{: #schematics-agent-apply}

Deploy or upgrade an agent to force deploy. For more information about the steps to use deploy command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent deploy --id AGENT_ID [--force-redploy] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an agent. |
| `--force-redeploy` or `-fd` | Optional | Force redeploys an Agent. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent deploy flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent deploy --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent destroy`
{: #schematics-agent-destroy}

Destroy an agent destroys the Cloud resources associated with the {{site.data.keyword.bpshort}} agent deployment.

Syntax

```sh
ibmcloud schematics agent destroy --id AGENT_ID [--force]

```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an agent. |
| `--force` or `-f` | Optional | The force action without confirmation. |
{: caption="{{site.data.keyword.bpshort}} Agent destroy flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent destroy --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent get`
{: #schematics-agent-get}

Retrieve the details of an existing agent such as agent ID, Name, Status, Version, Creation Time, Encryption status and Encryption CRN, including the values of all input variables. For more information about the steps to use get command, see [displaying an agent](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli)

Syntax

```sh
ibmcloud schematics agent get --id AGENT_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an agent. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent get flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent get --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent health`
{: #schematics-agent-health}

Performs the post deployment validation of an agent. For more information about the steps to use the agent health command, see [Monitoring agent health](/docs/schematics?topic=schematics-agentb1-health&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent health --id AGENT_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id`| Required | The ID of an agent. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent health flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent health --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent list`
{: #schematics-agent-list}

Lists the agents defined in the current {{site.data.keyword.bpshort}} region. For more information about the steps to use list command, see [displaying an agent](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent list [--location LOCATION] [--limit LIMIT] [--offset OFFSET] [--output OUTPUT_FORMAT]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--location` or `-l` | Optional |  Geographic locations supported by {{site.data.keyword.bplong_notm}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`. |
| `--limit` or `-lm` | Optional | Maximum number of agents to list. Ignored if a negative number is set. Maximum limit is 200, (default is -1). |
| `--offset` or `-m`| Optional | Offset in list. Ignored if a negative number is set (default: -1). |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent list flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent list --location us-south
```
{: pre}

### `ibmcloud schematics agent update`
{: #schematics-agent-update}

Update an agent configuration. Updating an agent does not re-validate or re-deploy your agent. For more information about the steps to use the agent update command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent update --id AGENT_ID [--description DESCRIPTION] [--tags TAGS] [--version VERSION] [--metadata AGENT_METADATA] [--file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id`| Required | The ID of an agent. |
| `--tags` or `-t` | Optional | Agent tags. This flag can be used multiple times, and search the agent related resources faster.|
| `--description` or `-d` | Optional | Short description of an agent.|
| `--version value` or `-v` | Optional | Specify the version of an agent. Defaults to available latest version.|
|  `--metadata` | Optional | Metadata of the agent. You can use the flag multiple times. For example, `git:private-git.github.com` or `git:gitlab.com`. If not set, defaults to `git:github.com`.|
| `--file` or `-f` | Optional | Path to the `JSON` file that contains the definition of the agent.|
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent update flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent update --id <AGENT_ID>
```
{: pre}


### `ibmcloud schematics agent validate`
{: #schematics-agent-plan}

Checks the prerequisites scan that analyses an agent and cluster configuration before deploying. For more information about the steps to use validate command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent validate --id AGENT_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id`| Required | The ID of the agent. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} Agent validate flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent validate --id AGENT_ID
```
{: pre}

## Agents policy commands
{: #policy-cmd}

{{site.data.keyword.bpshort}} (assignment) policies tell {{site.data.keyword.bpshort}} which agent it should use to execute workspace and action jobs in a specific network zone. Each agent have at least one policy associated with it to identify the jobs to run in the agents' location. See [assignment policies](/docs/schematics?topic=schematics-policy-manage&interface=cli).
{: shortdesc}

### `ibmcloud schematics policy create`
{: #schematics-policy-create}

Create a policy by using {{site.data.keyword.bpshort}} to select one or more {{site.data.keyword.bpshort}} objects, such as a workspace or  action, to be executed on the target agent.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy create --name POLICY_NAME --kind POLICY_KIND --location LOCATION --resource-group RESOURCE_GROUP --target-file TARGET_FILE [--description DESCRIPTION] [--tags TAGS] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | The unique name of the policy. |
| `--kind` or `-K` | Required | Policy kind for managing and deriving policy decision. Supported is `agent_assignment_policy`. |
| `--location` or `-l` | Optional | Geographic location of {{site.data.keyword.bpshort}} service where the agent is defined. For example, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for processing. |
| `--resource-group` or `-r` | Required | Resource group name or ID for the policy. |
| `--target-file` or `tf` | Optional | Path to the JSON file containing the definition of the policy. |
| `--description` or `-d` | Optional |  The description of the {{site.data.keyword.bpshort}} policy. |
| `--tags` or `-t`| Optional | Tags can be used multiple times to search for and locate agent policies faster. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} policy create flags" caption-side="bottom"}

#### Using the payload file
{: #policy-create-payload}

You can provide a payload file to specify certain parameters for the `policy create` command. Then, you pass the file name to the command by using the `--target-file` command option.
{: shortdesc}

You need to replace the `<...>` placeholders with the actual values. For example, `"<SELECTOR_KIND>"` as `"ids"`.
{: note}

Syntax

```json
{
	"target": {
		"selector_kind": "<SELECTOR_KIND>",
		"selector_ids": [
			"<SELECTOR_ID>"
		]
	},
	"parameter": {
		"agent_assignment_policy_parameter": {
			"selector_kind": "<SELECTOR_KIND>",
			"selector_scope": [{
				"kind": "<WORKSPACE>",
				"tags": [
					"dev:<ENVIRONMENT>",
					"demo"
				],
				"resource_groups": [
					"<RESOURCE_GROUP>"
				],
				"locations": [
					"<LOCATION>"
				]
			}]
		}
	}
}
```
{: codeblock}

Example

```json
{
	"target": {
		"selector_kind": "ids",
		"selector_ids": [
			"demo-agent-one"
		]
	},
	"parameter": {
		"agent_assignment_policy_parameter": {
			"selector_kind": "scoped",
			"selector_scope": [{
				"kind": "workspace",
				"tags": [
					"dev:test",
					"demo"
				],
				"resource_groups": [
					"Default"
				],
				"locations": [
					"us-south"
				]
			}]
		}
	}
}
```
{: codeblock}

Example

```sh
ibmcloud schematics policy create --name policy-101 --kind agent_assignment_policy --location us-south --resource-group Default --target-file ./<PATH>/target.json
```
{: pre}

### `ibmcloud schematics policy delete`
{: #schematics-policy-delete}

Delete a {{site.data.keyword.bpshort}} policy.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy delete --id POLICY_ID [--force]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the policy. |
| `--force` or `-f` | Optional | The force action without confirmation. |
{: caption="{{site.data.keyword.bpshort}} policy delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy delete --id policy-101.soP.282e
```
{: pre}

### `ibmcloud schematics policy get`
{: #schematics-policy-get}

Retrieve the details of an existing {{site.data.keyword.bpshort}} policy using policy ID.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy get --id POLICY_ID [--profile PROFILE] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | ID of the policy. |
| `--profile` or `-p` | Optional | Level of details to return. Valid values are `summary`, `detailed`, or `ids`. Defaults to `summary`. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} policy get flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy get --id policy-101.soP.282e
```
{: pre}

### `ibmcloud schematics policy list`
{: #schematics-policy-list}

Retrieve a list of all policies in the {{site.data.keyword.cloud_notm}} region for your account.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy list [--profile PROFILE] [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--profile` or `-r` | Optional | The level of details to return. Valid values are `summary`, `detailed` and `ids`. Defaults to `summary`.|
| `--limit` or `-l` | Optional |  Maximum number of policies to list. Ignored if a negative number is set. The number must be a positive integer between 1 and 200. The default value is `-1`.|
| `--offset`or `-m`| Optional | Offset in list. Ignored if a negative number is set. The default value is `-1`.|
| `--output` or `-o` |  Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} Policy list flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy list  --profile ids092030

```
{: pre}

### `ibmcloud schematics policy update`
{: #schematics-policy-update}

Update an existing policy using policy ID. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy update --id POLICY_ID [--kind POLICY_KIND] [--description DESCRIPTION] [--resource-group RESOURCE_GROUP] [--tags TAGS] [--file FILE] [--output OUTPUT]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
|   `--id` or `-i` | Required | ID of the policy. |
|   `--kind` or `-k` | Optional | Policy kind for managing and deriving policy decision. Supported is `agent_assignment_policy`. |
|   `--description` or `-d` | Optional |  The description of {{site.data.keyword.bpshort}} customization policy. |
|   `--resource-group` or `-r` | Optional |  Resource group name or ID for the policy. |
|   `--tags` or `-t` | Optional |     Policy tags. This flag can be used multiple times to search for and locate agent policies faster. |
|   `--file` or `-f`  | Optional |    Path to the `JSON` file containing the definition of the policy. |
|   `--output` or `-o`  | Optional |  Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} policy update flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy update --id <AGENT_ID> --description PolicyDescriptionUpdated
```
{: pre}



## Configure BYOK or KYOK commands
{: #kms-commands}

You can use your encryption keys from the {{site.data.keyword.cloud_notm}} key management services (KMS), {{site.data.keyword.keymanagementservicelong_notm}}(BYOK), and {{site.data.keyword.hscrypto}} (KYOK) to encrypt and secure your data stored in {{site.data.keyword.bpshort}}. For more information about how to protect sensitive data in {{site.data.keyword.bpshort}}, see [protecting your sensitive data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#data-storage).
{: shortdesc}

### Prerequisites
{: #key-prerequisites}

The key management system lists the instance that are created from your specific location and region. Following prerequisites are followed to perform the KMS activity.

- You should have your `KYOK`, or `BYOK`. To create the {{site.data.keyword.keymanagementservicelong_notm}} keys, see [create BYOK](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services). To create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} keys, see [create KYOK](https://cloud.ibm.com/catalog/services/key-protect).
- You need to [add root key](/docs/key-protect?topic=key-protect-import-root-keys&interface=ui#import-root-key-gui) to {{site.data.keyword.bpshort}} services.
- You need to configure [service to service authorization](/docs/account?topic=account-serviceauth&interface=ui#create-auth) to integrate `BYOK`, and `KYOK` in {{site.data.keyword.bpshort}} service.


KMS setting is a one time settings. You need to open a [support ticket](/docs/account?topic=account-using-avatar) to update KMS settings.
{: note}

### `ibmcloud schematics kms instance ls`
{: #schematics-kms-list}

Lists all the KMS instances of your {{site.data.keyword.cloud_notm}} account to find your {{site.data.keyword.keymanagementserviceshort}}or {{site.data.keyword.hscrypto}} by using your location  where keys are created and encrypted scheme such as `KYOK`, or `BYOK`. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics kms instances ls --location LOCATION_NAME --scheme ENCRYPTION_SCHEME [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--scheme` or `-s` | Required | Specify the encryption scheme. Supported values are `KYOK`, or `BYOK`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} KMS list flags" caption-side="top"}

Example

```sh
ibmcloud schematics kms instances ls -l US -s byok
```
{: pre}

### `ibmcloud schematics kms enable`
{: #schematics-kms-enable}

Enable KMS to encrypt your data in the specific location. For more information about enabling customer-managed keys for {{site.data.keyword.bpshort}}, see [enabling keys](/docs/schematics?topic=schematics-secure-data#data-storage).

Update the KMS settings for your location, by using your private endpoint, `CRN`, primary `CRK`, and secondary `CRK`. Note that you can update the KMS settings only once. For example, if you use an API endpoint for a geography, such as `North America`, only that are created in `us-south` or `us-east` are retrieved.
{: shortdesc}

Syntax

```sh
ibmcloud schematics kms enable --location LOCATION_NAME --scheme ENCRYPTION_SCHEME --group RESOURCE_GROUP --primary_name PRIMARY_KMS_NAME --primary_crn PRIMARY_KEY_CRN --primary_endpoint PRIMARY_KMSPRIVATEENDPOINT [--secondary_name SECONDARY_KMS_NAME][--secondary_crn SECONDARY_KEY_CRN] [--secondary_endpoint SECONDARY_KMSPRIVATEENDPOINT] [--output OUTPUT]
```
{: pre}


Command options

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
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} KMS enable flags" caption-side="top"}

Example

```sh
ibmcloud schematics kms enable -l US -s byok -g Default -pn Key-Protect-south -pc crn:v1:bluemix:public:kms:us-south:lalalalal -pe https://private.us-south.kms.cloud.ibm.com
```
{: pre}


### `ibmcloud schematics kms info`
{: #schematics-kms-info}

Retrieve the KMS on the API endpoint that you have your `KYOK`, or `BYOK`. For example, if you use an API endpoint for a geography, such as `North America`, only that are created in `us-south` or `us-east` are retrieved. Note that you need to enable `kms instances` in your account to run `info` command-line.
{: shortdesc}

Syntax

```sh
ibmcloud schematics kms info --location LOCATION_NAME [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} KMS information flags" caption-side="top"}

Example

```sh
ibmcloud schematics kms info -l US 
```
{: pre}

## General commands
{: #schematics-general-commands}

Use these general commands to find help and version information for the {{site.data.keyword.bplong_notm}} command-line plug-in. 
{: shortdesc}

### `ibmcloud schematics help`
{: #schematics-help-cmd}

View the supported {{site.data.keyword.bplong_notm}} command-line commands. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics help [command]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--help` or `-h` | Required | Lists the supported commands. |
| `command` | Optional | Specify the name of the command to fetch the command details. |
{: caption="{{site.data.keyword.bpshort}} help flags" caption-side="top"}

Example

```sh
ibmcloud schematics help 
```
{: pre}

### `ibmcloud schematics version`
{: #schematics-version}

List the versions of all supported open source projects in {{site.data.keyword.bpshort}}, such as the {{site.data.keyword.terraform-provider_full_notm}}, Ansible, Helm, and Kubernetes that are used to run {{site.data.keyword.bpshort}} actions on Cloud resources.
{: shortdesc}

Syntax

```sh
ibmcloud schematics version [--output OUTPUT]
```
{: pre}


Command options 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--output` or `-o` | Optional | Returns the CLI output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} version flags" caption-side="top"}

Example

```sh
ibmcloud schematics version --output json > "<filename.json>"
```
{: pre}


## Inventory commands
{: #inv-commands}

Review the commands to create, update, list, delete and work with your {{site.data.keyword.bplong_notm}} inventories used with {{site.data.keyword.bpshort}} actions.
{: shortdesc}

### `ibmcloud schematics inventory create`
{: #schematics-create-inv}

Create a [resource inventory](/docs/schematics?topic=schematics-inventories-setup) in {{site.data.keyword.bplong_notm}} that you can use with a {{site.data.keyword.bpshort}} action. A resource inventory includes all the target hosts where you want to run an Ansible playbook. You can create an inventory by using a payload file or the interactive mode.
{: shortdesc}

Syntax

```sh
ibmcloud schematics inventory create --name INVENTORY_NAME [--description DESCRIPTION] [--location GEOGRAPHY] [--resource-group RESOURCE_GROUP] [--inventories-ini INVENTORY_INI_FILE] [--resource-query RESOURCE_QUERY_ID] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

You need to pass either `--inventories-ini` file path or `--resource-query` ID for the inventory to use the target host details.
{: note}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name` or `-n` | Required | The unique name of a resource inventory. |
| `--description` or `-d` | Optional | The short description of an inventory. |
| `--location` or `-l` | Optional | The location where you want to store your resource inventory, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`. For more information about data storage in {{site.data.keyword.bpshort}}, see [Where is my data stored?](/docs/schematics?topic=schematics-secure-data#pi-location). |
|`resource-group` or `-r`| Optional | The name of the resource group where you want to create the action.|
|`--inventories-ini` or `-y` | Optional |The file path to the resource inventory file where you specified all target hosts. The resource inventory file must be provided in `INI` format. For more information about how to create a static resource inventory file, see [Creating static inventory files](/docs/schematics?topic=schematics-inventories-setup#static-inv). |
|`--resource-query` | Optional |Enter the ID of a resource query that you created. A resource query helps to dynamically build your resource inventory by using the Cloud resources that you created with a {{site.data.keyword.bpshort}} workspace. To create a resource query, see the `ibmcloud schematics resource-query create` [command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-rq). To enter multiple resource query IDs, use `--resource-query id1 --resource-query id2`.|
| `--file` or `-f` | Optional |The path to the JSON file where you specified the resource inventory that you want to create.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to create an inventory without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} inventory create flags" caption-side="top"}

#### Using the payload file
{: #inv-create-payload}

You can provide a payload file to specify certain parameters for the `inventory create` command. Then, you pass the file name to the command by using the `--file` command option.
{: shortdesc}

You need to replace the `<...>` placeholders with the actual values. For example, `"<INVENTORY_NAME>"` as `"myinventory"`.
{: note}


Syntax

```json
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

Example

```json
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

```sh
ibmcloud schematics inventory create --file inventory.json
```
{: pre}


#### Using the interactive mode
{: #inv-create-interactive}

Instead of entering your inventory details by using the command options or a payload file, you can also use the interactive mode for the command. This mode prompts you to enter the required values to create an inventory in {{site.data.keyword.bpshort}}. 
{: shortdesc}

1. Enter the command to create the inventory without any command options. 
    ```sh
    ibmcloud schematics inventory create
    ```
    {: pre}

2. Enter a name for your inventory and press the return key.
3. Enter the resource group where you want to create the inventory and press the return key. 
4. Enter the location where you want to create the inventory, such as **`us-south`**, **us-east**, **eu-de**, or **eu-gb**. Then, press the return key.
5. Review the details of the inventory that was created. 

### `ibmcloud schematics inventory delete`
{: #schematics-delete-inventory}

Delete the resource inventory definition using the inventory ID. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics inventory delete --id ACTION_ID [--force][--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an inventory that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="{{site.data.keyword.bpshort}} inventory delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics inventory delete --id us-east.INVENTORY.inventoryid12342
```
{: pre}


### `ibmcloud schematics inventory get`
{: #schematics-get-inv}

Retrieve detailed information of an existing {{site.data.keyword.bplong_notm}} inventory using the inventory ID.
{: shortdesc}

Syntax

```sh
ibmcloud schematics inventory get --id ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the resource inventory for which you want to list detailed information. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve details of an inventory without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} inventory get flags" caption-side="top"}

Example

```sh
ibmcloud schematics inventory get --id us-east.INVENTORY.inventoryid12342 --output json
```
{: pre}

### `ibmcloud schematics inventory list`
{: #schematics-list-inv}

Retrieve a list of all {{site.data.keyword.bpshort}} inventories in the current region for your account.
{: shortdesc}

Syntax

```sh
ibmcloud schematics inventory list [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

Command options

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--limit` or `-l` | Optional |  The maximum number of inventories that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the inventory in the list of inventories. For example, if you have three inventories in your account, the command returns these inventories as a list with three elements. To see a specific inventory in this list, you must enter the position number that the inventory has in the list. To list the first inventory in the list, enter `0`. To list the second inventory, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
{: caption="{{site.data.keyword.bpshort}} job list flags" caption-side="top"}

Example

```sh
ibmcloud schematics inventory list --output json
```
{: pre}


### `ibmcloud schematics inventory update`
{: #schematics-update-inv}

Update an existing resource inventory. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics inventory update  --id ID --name INVENTORY_NAME [--description DESCRIPTION] [--location GEOGRAPHY] [--resource-group RESOURCE_GROUP] [--inventories-ini INVENTORY_INI_FILE] [--resource-query RESOURCE_QUERY_ID] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id`  or `-i` | Required | Enter the ID of a resource inventory that you want to update. |
| `--name` or `-n` | Required | The unique name of an inventory. |
| `--description` or `-d` | Optional | The short description of an inventory. |
| `--location` or `-l` | Optional | The geographic locations supported by {{site.data.keyword.bplong_notm}} service, such as `us-south`, `us-east`, `eu-de`, or `eu-gb`.|
|`resource-group` or `-r`| Optional | The resource group name for an action.|
|`--inventories-ini` or `-y` | Optional |  File path of `INI` format file that contains the host details.|
|`--resource-query` | Optional |  Pass resource query ID. To pass multiple IDs use `--resource-query id1 --resource-query id2`.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of an inventory.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} inventory update flags" caption-side="top"}

Example

```sh
ibmcloud schematics inventory update  --id us-east.INVENTORY.inventory12312 --name inventoryname600 --description "Short description" --location us-east --resource-group Default --resource-query default.RESOURCEQUERY.string.12121  --output OUTPUT
```
{: pre}

## Inventory resource query commands
{: #rq-commands}

Dynamically build actions resource inventories using resource queries. Resource queries enable you to gather target host information from {{site.data.keyword.bpshort}} workspaces. For more information about resource queries and conditions, see [Creating resource inventories for {{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-inventories-setup).
{: shortdesc}

### `ibmcloud schematics resource query create`
{: #schematics-create-rq}

Create a resource query in {{site.data.keyword.bplong_notm}} that you can use to build your resource inventory. You can create a resource query by using a payload file or the command's interactive mode. You can create resource conditions by using [resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries).
{: shortdesc}

Syntax

```sh
ibmcloud schematics resource-query create --name RESOURCE_QUERY_NAME [--type RESOURCE_QUERY_TYPE] [--query-file QUERY_FILE_PATH] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name` or `-n` | Required | The unique name for a resource query. |
| `--type` or `-t` | Optional | The type of resource that you want to retrieve. Supported values are `vsi`. |
|`--query-file` or `-f` | Optional | The path to the JSON file where you specified the details of your resource query. To find a list of supported queries, see [Supported resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries). |
| `--file` or `-f` | Optional | The path to the JSON file that specifies the details of the resource query that you want to create. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the resource query without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} resource query create flags" caption-side="top"}

#### Using the payload file
{: #rq-create-payload}

You can provide a payload file to specify certain parameters for the `resource_query create` command. Then, you pass the file name to the command by using the `--file` command option. For a list of supported resource queries, see [Supported resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries).
{: shortdesc} 

You need to replace the `<...>` placeholders with the actual values. For example, `"<WORKSPACE_ID"` as `us-east.workspace.ID1231`.
{: note}

Syntax

```json
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

Example

```json
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

```sh
ibmcloud schematics resource-query create --name myquery --type vsi --query-file queries.json
```
{: pre}


#### Using the interactive mode
{: #rq-create-interactive}

Instead of entering your resource query details by using the command options or a payload file, you can use the interactive mode for the command. This mode prompts you to enter the required values to create a resource query in {{site.data.keyword.bpshort}}. You can create resource conditions by using [resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries).
{: shortdesc}

1. Enter the command to create the resource query without any command options. 
    ```sh
    ibmcloud schematics resource-query create 
    ```
    {: pre}

2. Enter a name for your resource query and press the return key.
3. Enter the path to your payload file. For a sample payload file, see [Using the payload file](/docs/schematics?topic=schematics-schematics-cli-reference#rq-create-payload). Then, press the return key.
4. Review the details of the resource query that was created for you. 


### `ibmcloud schematics resource query delete`
{: #schematics-delete-resource-query}

Delete the resource resource query definition by using the resource query ID from the {{site.data.keyword.bplong_notm}} service. Note you can delete the location and region, resource group from where your inventory was created. Also, make sure your IP addresses are in the [allowlist](/docs/schematics?topic=schematics-allowed-ipaddresses). 
{: shortdesc}

Syntax

```sh
ibmcloud schematics resource-query delete --id ID [--force] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of a resource query that you want to delete. |
| `--force` or `-f` | Optional | Force the deletion without user confirmation. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="{{site.data.keyword.bpshort}} resource query delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics resource-query  delete --id us-east.INVENTORY.inventoryid12342
```
{: pre}


### `ibmcloud schematics resource query get`
{: #schematics-get-rq}

Retrieve the information of an existing {{site.data.keyword.bplong_notm}} resource query by using a resource query ID.
{: shortdesc}

Syntax

```sh
ibmcloud schematics resource-query get --id ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the resource query that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve a resource query without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} resource query get flags" caption-side="top"}

Example

```sh
ibmcloud schematics resource-query get --id us-east.INVENTORY.inventoryid12342
```
{: pre}

### `ibmcloud schematics resource query list`
{: #schematics-list-rq}

Retrieve a list of all {{site.data.keyword.bpshort}} resource queries in the current region for your account.
{: shortdesc}

Syntax

```sh
ibmcloud schematics resource-query list [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

Command options

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--limit` or `-l` | Optional |  The maximum number of resource queries that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the resource query in the list of resource queries. For example, if you have three resource queries in your account, the command returns these resource queries as a list with three elements. To see a specific resource query in this list, you must enter the position number that the resource query has in the list. To list the first resource query in the list, enter `0`. To list the second resource query, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--output` or `-o` | Optional | Specify the output format. Only `JSON` format is supported.|
{: caption="{{site.data.keyword.bpshort}} resource query list flags" caption-side="top"}

Example

```sh
ibmcloud schematics resource-query list --output listoutput.json
```
{: pre}


### `ibmcloud schematics resource query update`
{: #schematics-update-rq}

Update or replace a resource query creates a copy of an resource query and relaunches an existing resource query by updating the information of an existing {{site.data.keyword.bplong_notm}} resource query. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics resource-query update --id ID --name RESOURCE_QUERY_NAME [--type RESOURCE_QUERY_TYPE] [--query-file QUERY_FILE_PATH] [--file FILE_NAME ] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id`  or `-i` | Required | The resource query ID. |
| `--name` or `-n` | Required | The unique name for a resource query. |
| `--type` or `-t` | Optional | The type of the resource query. such as `vsi`|
|`--query-file` or `-f` | Optional | The path to the JSON file containing queries.|
| `--file` or `-f` | Optional | Path to the JSON file containing the definition of an inventory.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the resource query without an interactive command-line session. |
{: caption="{{site.data.keyword.bpshort}} resource query update flags" caption-side="top"}

Example

```sh
ibmcloud schematics resource-query  update  --id us-east.INVENTORY.inventory12312 --name inventoryname600 --description "Short description" --location us-east --resource-group Default --resource-query default.RESOURCEQUERY.string.12121
```
{: pre}

## Workspace commands
{: #schematics-workspace-commands}

Review the commands that you can use to create and work with your {{site.data.keyword.bplong_notm}} workspace. 
{: shortdesc}

### `ibmcloud schematics workspace action`
{: #schematics-workspace-action}

Retrieve all activities (jobs) for a workspace, including the user ID of the person who initiated the action, the status, and a timestamp. 
{: shortdesc}

When you create a Terraform execution plan, or apply your Terraform template with {{site.data.keyword.bpshort}}, a {{site.data.keyword.bpshort}} actions is automatically created and assigned an action ID. You can use the action ID to retrieve the logs of this action by using the [`ibmcloud schematics logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs) command.

Syntax

```sh
ibmcloud schematics workspace action --id WORKSPACE_ID [--act-id ACTION_ID] [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to retrieve workspace activities. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--act-id` or `-a` | Optional | Enter the ID of a action that you want to retrieve. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} workspace execution flags" caption-side="top"}

Example
```sh
ibmcloud schematics workspace action --id myworkspace-a1aa1a1a-a11a-11
```
{: pre}

### `ibmcloud schematics workspace delete`
{: #schematics-workspace-delete}

Delete a workspace from the current region for your account. The deletion of your workspace does not remove any Cloud resources that you provisioned with this workspace. You can access and work with your resources from the {{site.data.keyword.cloud_notm}} dashboard directly, but you cannot use {{site.data.keyword.bplong_notm}} to manage your resources after you delete the workspace. 
{: shortdesc}

Decide if you want to delete the workspace, any associated resources, or both. This action cannot be undone. If you remove the workspace and keep the resources, you need to manage the resources with the resource list or CLI.
{: note}

| Action | Delete workspace | Delete all associated resources |
| -- | -- | -- |
| Delete workspace | True | False |
| Delete only resources | False | True |
| Delete workspace and the resources provisioned by workspace | True | True |
| Resources destroyed using command-line or resource list, and want to delete workspace | True | False |
{: caption="delete workspace and associated resource" caption-side="bottom"}

Syntax

```sh
ibmcloud schematics workspace delete --id WORKSPACE_ID [--force]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace that you want to remove. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--force` or `-f` | Optional | Force the deletion of your workspace without command-line prompts. |
{: caption="{{site.data.keyword.bpshort}} workspace delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace delete --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics workspace get`
{: #schematics-workspace-get}

Retrieve the details of an existing workspace such as workspace ID, Name, Status, Version, Creation Time, Template ID, Commit ID, Encryption status and Encryption CRN, including the values of all input variables.
{: shortdesc}	

Syntax

```sh
ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace, for which you want to retrieve the details. To find the `Resource ID` of a workspace, run `ibmcloud schematics workspace list` command to view the of list service instances. From your resource group get an `Resource ID` for the `--id` flag. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspace get flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace get --id myworkspace-a1aa1a1a-a11a-11
```
{: pre}

### `ibmcloud schematics workspace import`
{: #schematics-workspace-import}

You can import an existing resource with an valid resource address into your workspace state file. You need to ensure that the resource is only imported once and to a single workspace. Otherwise, you may see unwanted behavior if the resource is defined in multiple workspaces. Review the [Terraform documentation](https://developer.hashicorp.com/terraform/cli/commands/import#usage){: external} for details on how to use the `import` command. 


{: shortdesc}

Syntax

```sh
ibmcloud schematics workspace import --id WORKSPACE_ID --options OPTIONS --address ADDRESS --resourceID RESOURCE_ID
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to import an instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--options` or `-o` | Required | The command-line flags. For example `-var-file xxxxx/tf`. |
| `--address` or `-adr` | Required | Provide the address of the resource name you want to import.|
| `--resourceID` or `-rid` | Required | Provide the resource ID that you need to import in the file. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspace import flags" caption-side="top"}

Use the option `-options -var-file=schematics.tfvars` to tell {{site.data.keyword.bpshort}} to import the resource with the saved workspace variables.  


Example

```sh
ibmcloud schematics workspace import --id WID --address ibm_iam_access_group.accgrp --resourceID AccessGroupId-xxxxxx-xxxx-xxx-xxx-xxxx -o -var-file=schematics.tfvars
```
{: pre}


### `ibmcloud schematics workspace list`	
{: #schematics-workspace-list}

List the workspaces for the current region of your {{site.data.keyword.cloud_notm}} account and shows the details for your workspace. List workspace checks for the deprecation in a loop by invoking `versions` API every time for all the workspace through file cache.

Syntax

```sh
ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--output] [--region]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer starting from 1, maximum is 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the workspace in the list of workspaces. For example, if you have three workspaces in your account, the command returns these workspaces as a list with three elements. To see a specific workspace in this list, you must enter the position number that the workspace has in the list. To list the first workspace in the list, enter `0`. To list the second workspace, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--region` or `-r` | Optional | Specify the region, such as `eu`, `us`, `eu-gb`, `eu-de`, `us-south`, or `us-east`.|
{: caption="{{site.data.keyword.bpshort}} workspace list flags" caption-side="top"}

Example 

```sh
ibmcloud schematics workspace list --limit 10 --offset 20 
```
{: pre}

### `ibmcloud schematics workspace new`
{: #schematics-workspace-new}

Create a {{site.data.keyword.bpshort}} workspace that points to your Terraform template in GitHub or GitLab. If you want to provide your Terraform template by uploading a tape archive file (`.tar`), you can create the workspace without a connection to a GitHub repository and then use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command to provide the template.

{{site.data.keyword.bpshort}} does not support passing `.tar` file to create a workspace.
{: important}

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The location can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

To create a workspace, you can specify your workspace settings in a JSON file. Make sure that the JSON file follows the structure as outlined in this command. Also ensure the `location` and the `url` endpoint are pointing to the same region when you create or update workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

Syntax

```sh
ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH  [--agent-id AGENT_ID]  [--github-token GITHUB_TOKEN] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--file` or `-f` | Required | The relative path to a JSON file on your local machine that is used to configure your workspace. For more information about the sample JSON file with the details, see [JSON file create template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-create-template).|
| `--state` | Optional | The relative path to an existing Terraform state file on your local machine. To create the Terraform state file: </br> **1.** Show the content of an existing Terraform state file by using the [`ibmcloud schematics state pull`](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command.</br> **2.** Copy the content of the state file from your command-line output in to a file on your local machine that is named `terraform.tfstate`. </br> **3.** Use the relative path to the file in the `--state` command parameter. **Note** The {{site.data.keyword.bpshort}} workspace supports the `terraform.tfstate` file of less than 2 MB.|
| `--github-token` or `-g` | Optional |  Enter the functional personal access tokens for HTTPS Git operations. For example, `--github-token ${FUNCTIONAL_GIT_KEY}`.|
| `--agent-id` or `--aid` | Optional | The ID of an Agent where your workspace is created. The agent help you to run your workspace jobs on your infrastructure. For more information, see [{{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-agent-about-intro).|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspace create flags" caption-side="top"}

The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as **Alias 'terraform' are deprecated. Use 'schematics' or 'sch' in your commands.**
{: note}

#### Create file template in JSON format
{: #json-file-create-template}

 {{site.data.keyword.bpshort}} supports to download the Terraform modules template from the private repository. For more information, see [Supporting to download modules from private remote host](/docs/schematics?topic=schematics-download-modules-pvt-git). 
 
 You can create the JSON file as shared in the `example.json` file for workspace creation and pass the file path along with the file name in `--file` flag. The description of all the parameters of `example.json` as described in the table. 

You need to replace the `<...>` placeholders with the actual values. For example, `"<workspace_name>"` as `"testworkspace"`.
{: note}

Example

```json
{
    "name": "<workspace_name>",
    "type": [
        "<terraform_version>"
    ],
    "location": "<location>",
    "description": "<workspace_description>",
    "tags": [],
    "template_repo": {
        "url": "<github_source_repo_url>"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "<terraform_version>",
        "env_values":[
        {
          "env_key1": "dummy_text"
        },
        {
          "env_key2": "dummy_text"
        }
        ],
        "variablestore": [
        {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "string",
          "secure": true,
          "description":"<description>"
        },
        {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "bool",
          "secure": false,
          "description":"<description>"
        },
    {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "list(string);",
          "secure": false,
            "description":"<description>"
        },
    {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "map(number)",
          "secure": false,
          "description":"<description>"
        },
    {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "tuple([string, list(string), number, bool])",
          "secure": false,
          "description":"<description>"
        },
    {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "any",
          "secure": false,
          "description":"<description>"
        }
        ]
    }
    ],
}
```
{: codeblock}

Example JSON for uploading in a `.tar` file

```json
{
    "name": "<workspace_name>",
    "type": [
        "<terraform_version>"
    ],
    "location": "<location>",
    "description": "<workspace_description>",
    "tags": [],
    "template_repo": {
        "url": "<github_source_repo_url>"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "<terraform_version>",
        "env_values":[
        {
          "env_key1": "dummy_text"
        },
        {
          "env_key2": "dummy_text"
        }
        ],
        "variablestore": [
        {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "string",
          "secure": true,
	      "description":"<description>"
        },
        {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "bool",
          "secure": false,
	      "description":"<description>"
        },
        {
          "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "list(string)",
          "secure": false,
	      "description":"<description>"
        },
	    {
	      "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "map(number)",
          "secure": false,
	      "description":"<description>"
        },
	    {
	      "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "tuple([string, list(string), number, bool])",
          "secure": false,
	      "description":"<description>"
        },
	    {
	      "name": "<variable_name_x>",
          "value": "<variable_value_x>",
          "type": "any",
          "secure": false,
	      "description":"<description>"
        }
        ]
    }
    ]
}
```
{: codeblock}

| Parameter | Required / Optional | Description |
| -- | -- | -- |
| `workspace_name` | Optional | Enter a name for your workspace. The maximum length of character limit is set to less than 1 MB. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspaces-plan#structure-workspace).|
| `terraform_version` | Optional | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v1.5` to use Terraform version 1.5,`terraform_v1.4` to use Terraform version 1.4, and similarly, `terraform_v1.4`. For example, when you specify `terraform_v1.5` means users can have template that are of Terraform `v1.5.0`, `v1.5.1`, or `v1.5.7`, so on. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template. {{site.data.keyword.bpshort}} supports `Terraform_v1.x` and also plans to make releases available after `30 to 45 days` of HashiCorp Configuration Language (HCL) release. |
| `location` | Optional | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} actions run and where your workspace data is stored. If you do not enter a location, {{site.data.keyword.bpshort}} determines the location based on the {{site.data.keyword.cloud_notm}} region that you targeted. To view the region that you targeted, run `ibmcloud target --output json` and look at the `region` field. To target a different region, run `ibmcloud target -r <region>`. If you enter a location, make sure that the location matches the {{site.data.keyword.cloud_notm}} region that you targeted. |
| `description` | Optional | Enter a description for your workspace. |
| `template_repo.url` | Optional | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. |
| `template_repo.branch` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. Now, in `template_repo`, you can also update URL with more parameters as shown in the block. |
| `template_repo.datafolder` | Optional | Enter the name of the folder in the Git repository, that contains the template. |
| `template_repo.release` | Optional | Enter the GitHub or GitLab release that points to your Terraform configuration files. |
| `github_source_repo_url` | Optional | Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. If you choose to create your workspace without a GitHub repository, your workspace is created with a **draft** state. To connect your workspace to a GitHub repository later, you must use the `ibmcloud schematics workspace update` command. If you plan to provide your Terraform template by uploading a tape archive file (`.tar`), leave the URL empty, and use the [ibmcloud schematics workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command after you created the workspace. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning. |
| `env_values` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform action. This field must be provided as a list of key-value pairs. Each entry is a map with one entry where `key = variable name` and `value = value`. You can define environment variables for {{site.data.keyword.cloud_notm}} catalog offerings that are provisioned by using a bash script files. |
| `variable_name` | Optional | Enter the name for the input variable that you declared in your Terraform configuration files. |
| `variable_type` | Optional | `Terraform v0.12` supports `string`, `list`, `map`, `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`. |
| `variable_value` | Optional | Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable). [For example](/docs/schematics?topic=schematics-schematics-cli-reference#syntax_of_variablevalue)|
| `secure` | Optional | Set the `secure` parameter to **true**. By default, this parameter is set to **false**. |
| `val1` | Optional | In the payload you can provide an environment variable that can execute in your workspace during plan, apply or destroy stage. Also values are encrypted and stored in COS. |
{: caption="JSON file component description" caption-side="bottom"}


{{site.data.keyword.bplong_notm}} supports setting up environment variable such as `TF_PARALLELISM`, `TF_LOG`. For more information about the list of environment variable and its usage, see [List of environment variables](/docs/schematics?topic=schematics-set-parallelism#list-special-env-vars).
{: note}

Example

```sh
ibmcloud schematics workspace new --file example.json
```
{: pre}

### `ibmcloud schematics refresh`
{: #schematics-refresh}

Perform an {{site.data.keyword.cloud_notm}} refresh action against your workspace. A refresh action validates the Cloud resources in your account against the state that is stored in the Terraform state file of your workspace. If differences are found, the Terraform state file is updated accordingly. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics refresh --id WORKSPACE_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that you want to refresh and run an action against. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} refresh flags" caption-side="top"}


Example
```sh
ibmcloud schematics refresh --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics state list`
{: #state-list}

List the `Name`, `Type`, `URL`, and `Taint Status` of the Cloud resources that are documented in your Terraform state file (`terraform.tfstate`). 
{: shortdesc}	

`Taint Status` returns **tainted** for (true) or **blank** for (false).
{: note}

Syntax

```sh
ibmcloud schematics state list --id WORKSPACE_ID  [--output json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list the Cloud resources that are documented in the Terraform state file. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} state list flags" caption-side="top"}

Example

```sh
ibmcloud schematics state list --id myworkspace-a1aa1a1a-a11a-11  
```
{: pre}


### `ibmcloud schematics workspace taint`
{: #schematics-workspace-taint}

Manually marks an instance or resources as tainted, by forcing the resources to be re-created on the next apply. Taint modifies the state file, but not the infrastructure in your workspace. When you perform next plan the changes displays as re-created, and in the next apply the change is implemented.
{: shortdesc}

You must execute [`ibmcloud schematics state list`](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) command to view the tainted status of your resources. `Taint Status` returns **tainted** for (true) or **blank** for (false).
{: note}

Syntax

```sh
ibmcloud schematics workspace taint --id WORKSPACE_ID [--options OPTIONS]  --address PARAMETER
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to re-create the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to show. |
| `--address` or `-adr` | Required | Enter the address of the resource to mark as taint.|
{: caption="{{site.data.keyword.bpshort}} workspace taint flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace taint --id myworkspace-lalalalalalala-11 --address null_resource.sleep  
```
{: pre}


### `ibmcloud schematics workspace untaint`
{: #schematics-workspace-untaint}

Manually marks an instance or resources as `untaint`, by forcing the resources to be restored on the next apply. When you perform next plan the changes shows as restored and in the next apply the change is implemented.
{: shortdesc}

You can execute [`ibmcloud schematics state list`](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) command to view the tainted status of your resources. `Taint Status` returns **tainted** for (true) or **blank** for (false).
{: note}

Syntax

```sh
ibmcloud schematics workspace untaint --id WORKSPACE_ID [--options OPTIONS]  [--address PARAMETER]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to re-create the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to show. |
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as `untaint`.|
{: caption="{{site.data.keyword.bpshort}} workspace `untaint` flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace untaint --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}

### `ibmcloud schematics workspace update`
{: #schematics-workspace-update}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The region can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again. Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}	

If you provided your Terraform template by uploading a tape archive file (`.tar`) and you want to update your template, you must use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command.
{: note}

Syntax

```sh
ibmcloud schematics workspace update --id WORKSPACE_ID [--file FILE_NAME] [--github-token GITHUB_TOKEN] [--pull-latest] [--output OUTPUT]
```
{: pre}

`Pull-latest` flag is not supported for workspaces created by using templates from {{site.data.keyword.cloud_notm}} catalogs.
{: note}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to update the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Optional | The relative path to a JSON file on your local machine that includes the updated parameters for your workspace. For more information about the sample JSON file with the details, see [JSON file update template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-update-template).|
| `--github-token` or `-g` | Optional |  Enter the GitHub token value to access private Git repository.|
| `--pull-latest` or `--pl` | Optional | Pull the latest changes from your GitHub repository into workspace. If this flag is set `--file` flag is ignored. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspace update flags" caption-side="top"}

#### Update file template in JSON format
{: #json-file-update-template}

You can create the JSON as shared in the `example.json` file for workspace update and pass the file path along with the file name in `--file` flag. The description of all the parameters of example.json is described in the table. 

You need to replace the `<...>` placeholders with the actual values. For example, `"<workspace_name>"` as `"testworkspace"`.
{: note}

**example.json:**

```json
{
    "name": "<workspace_name>",
    "type": "<terraform_version>",
    "description": "<workspace_description>",
    "tags": [],
    "resource_group": "<resource_group>",
    "workspace_status": {
        "frozen": "<true_or_false>"
    },
    "template_repo": { 
        "url": "<source_repo_url>"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "<terraform_version>",
        "env_values":[
        {
           "env_key1": "dummy_text"
        },
        {
           "env_key2": "dummy_text"
        }
        ],
        "variablestore": [
        {
          "name": "<variable_name1>",
          "value": "<variable_value1>",
          "type": "<variable_type1>",
          "secure": true,
	  "use_default": true        },
        {
          "name": "<variable_name2>",
          "value": "<variable_value2>",
          "type": "<variable_type2>",
          "secure": false,
	  "use_default": true
	  }
        ]
    }
    ],
}
```
{: codeblock}

| Parameter | Required / Optional | Description |
| --- | --- | --- |
| `name` | Optional | Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspaces-plan#structure-workspace). If you update the name of the workspace, the ID of the workspace does not change. |
| `type` | Optional | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v1.5` to use Terraform version 1.5, `terraform_v1.4` to use Terraform version 1.4. For example, when you specify `terraform_v1.5` means users can have template that are of Terraform `v1.5.0`, `v1.5.1`, or `v1.5.7`, so on. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template. |
| `description` | Optional | Enter tags that you want to associate with your workspace. Tags can help you find your workspace faster. |
| `resource_group` | Optional | Enter the resource group where you want to provision your workspace. |
| `workspace_status` | Optional | Freeze or unfreeze a workspace. If a workspace is frozen, changes to the workspace are disabled. |
| `template_repo.url` | Optional | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. |
| `template_repo.branch` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. Now, in template repository, you can also update URL with more parameters as shown in the block. |
| `template_repo.datafolder` | Optional | Enter the name of the folder in the Git repository, that contains the template.|
| `template_repo.release` | Optional | Enter the GitHub or GitLab release that points to your Terraform configuration files. |
| `github_source_repo_url` | Optional | Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. |
| `template_data.folder` | Optional | Enter the name for the input variable that you declared in your Terraform configuration files. |
| `template_data.type` | Optional | Enter the name for the input variable type that you declared in your Terraform configuration files. |
| `template_data[0].env_values[i].va11` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform job. This field must be provided as a list of key-value pairs, for example, `TF_LOG=debug`. Each entry is a map with one entry where **key is the environment variable name and value is value**. |
| `template_data[0].env_values[i].val2` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform job. This field must be provided as a list of key-value pairs, for example, `TF_LOG=debug`. Each entry is a map with one entry where **key is the environment variable name and value is value**. |
| `template_data[0].env_values_metadata` | Optional | Environment variables metadata. |
| `template_data[0].variablestore[i].name` | Optional | Enter the name for the input variable that you declared in your Terraform configuration files. |
| `template_data[0].variablestore[ii].type` | Required | `Terraform v0.12` supports `string`, `list`, `map`, `bool`, `number` and complex data types such as `list(type)`, `map(type)`, `object({attribute name=type,..})`, `set(type)`, `tuple([type])`.|
| `template_data[0].variablestore[iii].value` | Optional | Enter the value as a string for the primitive types such as `bool`, `number`, `string`, and `HCL` format for the complex variables, as you provide in a `.tfvars` file. You can override the default values of `.tfvars` by setting `use_default` parameter as `true`. You need to enter escaped string of `HCL` format for the value, as shown in the example. For more information about how to declare variables in a Terraform configuration file and provide value to schematics, see [Using input variables to customize resources](/docs/schematics?topic=schematics-create-tf-config#declare-variable) and [variable store example](/docs/schematics?topic=schematics-schematics-cli-reference#syntax_of_variablestore)|
| `template_data[0].variablestore[iv].secure` | Optional | Set the `secure` parameter to **true**. By default, this parameter is set to **false**.|
| `template_data[0].variablestore[v].use_default` | Optional | Set the `use_default` parameter to **true** to override the default `.tfvars` parameter. By default, this parameter is set to **false**. |
| `github_source_repo_url` | Optional | Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. |
{: caption="{{site.data.keyword.bplong_notm}} update payload" caption-side="bottom"}

#### Example for variable store
{: #syntax_of_variablestore}

```yaml
"variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))",
		                "use_default":true
                },
```
{: pre}


Example

```sh
ibmcloud schematics workspace update --id myworkspace-a1aa1a1a-a11a-11 --file myfile.json 
```
{: pre}

### `ibmcloud schematics workspace update variables`
{: #schematics-workspace-update-variables}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

Update variables allows you to update one or more input variables for an existing workspace. You cannot update the workspace metadata variables such as name, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.

Syntax

```sh
ibmcloud schematics workspace update-variables --id WORKSPACE_ID --template TEMPLATE_ID --file FILE_NAME [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to update the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Required | The relative path to a JSON file on your local machine that includes the updated parameters for your workspace variables to update. For more information about the sample JSON file with the details, see [JSON file update template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-update-template).|
| `--template` or `-tid` | Required |  Enter the template ID. Use [ibmcloud schematics workspace get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) to fetch the template ID.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspace update flags" caption-side="top"}

#### Example for variable store and environment values
{: #syntax_of_update_variablestore}

**exampleupdatevar.json:**

```json
{
    "variablestore":
    [
                {
                    "name": "vpc_name",
                    "secure": true,
                    "value": "vpc_name_snsitive_updated",
                    "type": "string",
                    "description": ""
                },
                {
                    "name": "IC_SCHEMATICS_WORKSPACE_ID",
                    "secure": false,
                    "value": "test_updated",
                    "type": "string",
                    "description": ""
                }
    ],
    "env_values": 
    [
                {
                    "name": "TF_LOG",
                    "value": "debug_working",
                    "secure": false,
                    "hidden": false
                },
                {
                    "name": "TF_ENV",
                    "value": "test_working",
                    "secure": false,
                    "hidden": false
                }
    ]
}
```
{: pre}


Example

```sh
ibmcloud schematics workspace update-variables --id myworkspace-a1aa1a1a-a11a-11 --template myworkspacetemplateid-1000 --file exampleupdatevar.json 
```
{: pre}



### `ibmcloud schematics workspace upload`
{: #schematics-workspace-upload}

Provide your Terraform template by uploading a tape archive file (`.tar`) to your {{site.data.keyword.bpshort}} workspace. The `.tar` supports the Cloud Shell commands.
{: shortdesc}

Before you begin, make sure that you [created your workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) without a link to a GitHub or GitLab repository.
{: important}

Syntax

```sh
ibmcloud schematics workspace upload  --id WORKSPACE_ID --file FILE_NAME --template TEMPLATE_ID [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace where you want to upload your tape archive file (`.tar`). To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Required | Enter the full file path on your local machine where your `.tar` file is stored.|
| `--template` or `-tid` | Required |  The unique identifier of the Terraform template for which you want to show the content of the Terraform state file. To find the ID of the template, run `ibmcloud schematics workspace get --id <workspace_ID>` and find the template ID in the **Template Variables for:** field of your command-line output.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} workspaces upload flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace upload --id myworkspace-a1aa1a1a-a11a-11 --file /Users/myuser/Documents/mytar/vpc.tar --template 25111111-0000-4c
```
{: pre}

Create the `TAR` file of your template repo by using the `TAR` command given `tar -cvf vpc.tar $TEMPLATE_REPO_FOLDER`
{: note}

#### Example of the variable value
{: #syntax_of_variablevalue}

```json
"variablestore": [
    {
        "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
        "description": "",
        "name": "docker_ports",
        "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))"
    },
]
```
{: pre}


## Workspace job commands
{: #schematics-resource-commands}

Run {{site.data.keyword.bpshort}} operations, to create, update, and delete Cloud resources. Using familiar Terraform semantics, plan, apply and destroy Terraform workspaces to manage the lifecycle of cloud resources. 

### `ibmcloud schematics apply`
{: #schematics-apply}

When you apply a workspace Terraform template, your resources are provisioned, modified, or removed from {{site.data.keyword.cloud_notm}}. Temporary files created during the apply operation can be [persisted](/docs/schematics?topic=schematics-general-faq#persist-file) for future operations.  
{: shortdesc}

Your workspace must be in an **Inactive**,  **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} apply operation. For more information about workspace states, see [workspace state diagram](/docs/schematics?topic=schematics-wks-state#workspace-state-diagram).
{: note}

While your Terraform jobs are running, the workspace is locked and changes cannot be made to your workspace until execution is complete.
{: important}

Syntax

```sh
ibmcloud schematics apply --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--var-file PATH_TO_VARIABLES_FILE] [--force] [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source control repository that you want to apply in {{site.data.keyword.cloud_notm}}. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the creation of a specific resource of your Terraform configuration file by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources that are defined in your configuration file is not created or updated. To target the creation of multiple resources, use the following syntax: `--target <resource1> --target <resource2>`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for creation.|
| `--var-file` or `--vf` | Optional |  The file path to the `terraform.tfvars` file that you created on your local machine. Use this file to store sensitive information, such as the {{site.data.keyword.cloud_notm}} API key or credentials to connect to {{site.data.keyword.cloud_notm}} classic infrastructure in the format `<key>=<value>`. Variables must be defined in single line format for example, as `availability_zone_names = ["us-east-1a","us-west-1c"]`. All key value pairs that are defined in this file are automatically loaded into Terraform when you initialize the Terraform CLI. To specify multiple `tfvars` files, specify `--var-file TFVARS_FILE_PATH1 --var-file TFVARS_FILE_PATH2`.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} apply flags" caption-side="top"}

Example

```sh
ibmcloud schematics apply --id myworkspace-a1aa1a1a-a11a-11 --target ibm_is_instance.vm1 --var-file ./terraform.tfvars
```
{: pre}


### `ibmcloud schematics destroy`
{: #schematics-destroy}

Remove Cloud resources that you provisioned using your {{site.data.keyword.bpshort}} workspace, even if these resources are active. By default, the command lists all the resources to preview and then receives confirmation to destroy. If you use `--force or -f` flag in the destroy command, you cannot see the preview of the resources that you want to destory. 
{: shortdesc}

Use this command with caution. After you run the command, you cannot reverse the removal of your Cloud resources. If you have written data to provisioned storage or databases, ensure that you create a backup to persist your data
{: important} 	

Your workspace must be in an **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} destroy action. 
{: note}

Syntax

```sh
ibmcloud schematics destroy --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--force] [--output OUTPUT]
```
{: pre}


Command options 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that specifies the Cloud resources that you want to remove. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the deletion of a specific resource by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources in your workspace remain unchanged. To target the deletion of multiple resources, use the following syntax: `--target <resource1> --target <resource2>`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for deletion. Also, if the targeted resource can only be deleted if dependent resources are deleted, such as a VPC can only be deleted if the attached subnet is deleted, then all dependent resources are targeted for deletion as well.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. You cannot see the preview of the resources that you want to destory. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} destroy flags" caption-side="top"}

Example

```sh
ibmcloud schematics destroy --id myworkspace-a1aa1a1a-a11a-11 --target ibm_is_vpc.myvpc
```
{: pre}

### `ibmcloud schematics logs`
{: #schematics-logs}

Retrieve the Terraform log files for {{site.data.keyword.bpshort}} workspace or a specific workspace action ID. Use the log files to troubleshoot Terraform template issues or issues that occur during the resource provisioning, modification, or deletion process. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics logs --id WORKSPACE_ID [--act-id ACTION_ID]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to retrieve Terraform log files. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--act-id` or `-1` | Optional | The ID of an action for which you want to retrieve Terraform logs. To find a list of action IDs, run `ibmcloud schematics workspace action --id WORKSPACE_ID` command. |
{: caption="{{site.data.keyword.bpshort}} logs flags" caption-side="top"}

Example

```sh
ibmcloud schematics logs --id myworkspace-a1aa1a1a-a11a-11 --act-id 9876543121abc1234cdst
```
{: pre}

### `ibmcloud schematics output`
{: #schematics-output2}

Retrieve the Terraform output values for the workspace. You can define output values in your Terraform template to include data that you want to make accessible to other workspaces. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics output --id WORKSPACE_ID[--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list Terraform output values. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} output flags" caption-side="top"}

Example

```sh
ibmcloud schematics output --id myworkspace3_2-31cf7130-d0c4-4d
```
{: pre}

### `ibmcloud schematics plan`
{: #schematics-plan}

Scan the Terraform template in your source repository and compare this template against the Cloud resources that are already deployed. The command-line output shows the Cloud resources that must be added, modified, [persisted](/docs/schematics?topic=schematics-general-faq#persist-file), or removed to achieve the state that is described in your configuration file.
{: shortdesc}

Your workspace must be in an **Inactive**, **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} plan action. 
{: note}

During the creation of the Terraform execution plan, you cannot make any changes to your workspace. 
{: note}

Syntax

```sh
ibmcloud schematics plan --id WORKSPACE_ID [--var-file PATH_TO_VARIABLES_FILE] [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that you want to scan. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--var-file` or `--vf` | Optional |  The file path to the `terraform.tfvars` file that you created on your local machine. Use this file to store sensitive information, such as the {{site.data.keyword.cloud_notm}} API key or credentials to connect to {{site.data.keyword.cloud_notm}} classic infrastructure in the format `<key>=<value>`. Variables must be defined in single line format for example, as `availability_zone_names = ["us-east-1a","us-west-1c"]`. All key value pairs that are defined in this file are automatically loaded into Terraform when you initialize the Terraform CLI. To specify multiple `tfvars` files, specify `--var-file TFVARS_FILE_PATH1 --var-file TFVARS_FILE_PATH2`.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} output flags" caption-side="top"}

Example

```sh
ibmcloud schematics plan --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}


## Workspace stop commands
{: #stop-cmds}

After invoking a workspace job, like a `plan`, an `apply`, or a `destroy`, you may want to stop the running job, or to stop the provisioning of resources. When stopping, or canceling a long running job, it is advisable to first check the job logs to determine whether the job is actually stuck and needs stopping, or if it is performing long running operations that are taking time to complete. 

{{site.data.keyword.bpshort}} provides a number of options to allows users to `(gracefully) stop`, `force-stop`, or `terminate` the running job in order of immediacy and impact of the stop operation. 
{: shortdesc}

Review the commands to `(gracefully) stop`, `force-stop` or `terminate` jobs. 

### `ibmcloud schematics workspace job stop`
{: #schematics-stop-job}

Stops a running workspace job by sending an interrupt signal to Terraform to terminate execution.  
{: shortdesc}

Syntax

```sh
ibmcloud schematics workspace job stop --id WORKSPACE_ID --job-id JOB_ID [--stop] [--force-stop] [--terminate]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The workspace ID to update. |
| `--job-id` or `--jid` | Required | The job ID of the job. |
| `--stop,` | Optional | Removes the job from the pending queue.|
| `--force-stop` or `--fs` | Optional | Sends a kill signal to the Terraform execution in the engine, also attempts to immediately stop the execution. |
| `--terminate` or `-t` | Optional | Abruptly kills the engine, marks the job as stopped, and unlocks your workspace. Data is not saved using this flag. |
{: caption="{{site.data.keyword.bpshort}} job stop flags" caption-side="bottom"}

Example

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --stop --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --force-stop --job-id <JOB_ID>
```
{: pre}

```sh
ibmcloud schematics workspace job stop --id <WORKSPACE_ID> --terminate --job-id <JOB_ID>
```
{: pre}



## Workspace state file commands
{: #state-file-cmds}

Review the commands that you can use to work with the Terraform state file (`terraform.tfstate`) for a workspace.
{: shortdesc}

You can import an existing Terraform state file during the creation of your workspace. For more information, see the [`ibmcloud workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command. 
{: note}

### `ibmcloud schematics state pull`
{: #state-pull}

Show the content of the Terraform state file (`terraform.tfstate`) for a specific Terraform template of your workspace. 
{: shortdesc}	

Syntax

```sh
ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace where you want to run the commands. |
| `--template` or `--tid` | Required | The unique identifier of the Terraform template for which you want to show the content of the Terraform state file. To find the ID of the template, run `ibmcloud schematics workspace get --id <workspace_ID>` and find the template ID in the **Template Variables for:** field of your command-line output. |
{: caption="{{site.data.keyword.bpshort}} state pull flags" caption-side="top"}

Example

```sh
ibmcloud schematics state pull --id myworkspace-a1aa1a1a-a11a-11 --template a1aa11a1-11a1-11
```
{: pre}


### `ibmcloud schematics workspace state show`
{: #schematics-workspace-show}

Provides the readable output from a state or plan of a workspace as Terraform sees it. You can use to ensure the current state and planned operations status. You need to use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs) command.
{: shortdesc}

Syntax

```sh
ibmcloud schematics workspace state show --id WORKSPACE_ID  --address ADDRESS [--options OPTIONS]
```
{: pre}



Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace to update. |
| `--address` or `-adr` | Required | Enter the address that points to a single resource in the state to show.|
| `--options` or `-o` | Optional | Enter the command-line flags. |
{: caption="{{site.data.keyword.bpshort}} state pull flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace show --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep 
```
{: pre}

### `ibmcloud schematics workspace state mv`
{: #schematics-wks_statemv}

If you move the state for a resource within the state file. The workspace continues to function, but the next plan or apply operation will not find the resource or instance in the state file. If no changes are made to the template, you can see recreation of the resource on the next operation by Terraform.
{: shortdesc}

```sh
ibmcloud schematics workspace state mv --id WORKSPACE_ID --source SOURCE  --destination DESTINATION 
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique ID of the workspace for which you want to move an instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--source` or `-s` | Required | Enter the source address of an item to move.|
| `--destination` or `-d` | Required | Provide the destination address of an item.|
{: caption="{{site.data.keyword.bpshort}} state move flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace state mv --id myworkspace-a1aa1a1a-a11a-11 -s testsourceresource -d null_resource.sleep 
```
{: pre}


### `ibmcloud schematics workspace state rm`
{: #schematics-wks_staterm}

If you remove the state for a resource or instance within the state file. The workspace continues to function, but the next plan or apply operation will not find the resource or instance in the state file. If no changes are made to the template, you can see recreation of the resource on the next operation by Terraform.
{: shortdesc}


```sh
ibmcloud schematics workspace state rm --id WORKSPACE_ID [--options OPTIONS] --address PARAMETER 
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to remove the instance or resource. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--options` or `-o` | Optional | Enter the option flag that you want to remove. |
| `--address` or `-adr` | Required | Enter the address of the resource to mark as taint.|
{: caption="{{site.data.keyword.bpshort}} state remove flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace state rm --id myworkspace-a1aa1a1a-a11a-11 --address null_resource.sleep --destination null_resource.slept 
```
{: pre}

## Workspace Terraform commands
{: #tf-cmds}

You can run Terraform commands to manipulate Cloud resources and modify {{site.data.keyword.bpshort}} state.
{: shortdesc}

Workspace Terraform commands are not supported in the UI. 
{: important}

The table provides the summary of supported Terraform workspace commands.

|Command | Description| 
|------|  ------|
|`show`| Inspects Terraform state or plan.|
|`output`| Reads an output from a Terraform state file.|
|`import`| Imports an existing infrastructure into Terraform.|
|`taint`|	 Mark a resource for recreation. |
|`untaint`|Do not mark a resource as tainted.|
|`state`|	An advanced state management command to write sub commands to remove or move `rm && mv`.|
{: caption="Terraform commands summary" caption-side="bottom"}

### Terraform commands
{: #cmds}

Terraform commands are executed using a JSON file to specify inputs. 

Syntax

```sh
ibmcloud schematics workspace commands --id WORKSPACE_ID --file FILE_NAME 
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--id` or `-i` | Required | The unique ID of the workspace where you want to run the commands. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--file` or `--f` | Required | Path to the `JSON` file containing the list of Terraform commands.|
{: caption="{{site.data.keyword.bpshort}} Terraform commands flags" caption-side="top"}

Sample payload of `Test.JSON` file

```json
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
"operation_name": "workspace Command",
"description": "Executing command"
}
```
{: codeblock}


The table provides the list of key parameters of the JSON file for the `Commands` API, for the command-line and the API.

| Key | Required / Optional | Description |
| ------ | -------- | ---------- |
|`command`| Required |Provide the command. Supported commands are `show`,`taint`, `untaint`, `state`, `import`, `output`.|
|`command_params`| Required | The address parameters for the command name for `CLI`, such as resource name, absolute path of the file name. For API, you have to send option flag and address parameter in `command_params`.|
|`command_name`| Required | The name for the command block.|
|`command_desc`| Optional | The text to describe the command block.|
|`command_onError`| Optional |  Instruction to continue or break in case of error in the command. |
|`command_dependsOn`|Optional| Dependency on the previous commands.|
|`command_status`| Not required | Displays the command executed status, either `success` or `failure`|
{: caption="List of key parameters" caption-side="bottom"}

Example

```sh
ibmcloud schematics workspace commands --id cli-sleepy-0bedc51f-c344-50 --file /<FILE_PATH>/Test.JSON
```
{: pre}

## CLI version history 
{: #cli_version-releases}

Find a summary of changes for each version of {{site.data.keyword.bpshort}} CLI plug-in. Be sure to keep your CLI up-to-date so that you can use all the available commands and their options.
{: shortdesc}


| Version | Release date | Changes |
| ----- | ------- | -------------- |
| 1.12.27 | 30 July 2025 | {{site.data.keyword.bpshort}} CLI plugin fixes the support to target Montreal endpoints through `ca-mon` region.|
| 1.12.26 | 07 April 2025 | {{site.data.keyword.bpshort}} CLI plugin enhanced [ibmcloud schematics destroy](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-destroy) preview, updated one pipeline base image, fixed `nil pointer exception` in [ibmcloud schematics action create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-create-action), [ibmcloud schematics action update](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-update-action), and [ibmcloud schematics action get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-get-action) operations.|
| 1.12.25 | 10 January 2025 | {{site.data.keyword.bpshort}} CLI plugin supports [ibmcloud schematics workspace update variables](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update-variables) CLI command to update only the required input variables for an existing workspace. It also enhances the [ibmcloud schematics destroy](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-destroy) command with the preview feature to list all the job resources with confirmation. The {{site.data.keyword.bplong_notm}} [workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get), [an action](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-get-action), and [an agent](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-get) get commands fetches the encryption CRN and encryption status such as `IBM Default` or `BYOK` or `KYOK` details.|
| 1.12.24 | 8 July 2024 | {{site.data.keyword.bpshort}} CLI plugin fixes the support to target Toronto endpoints through `ca-tor` region.|
| 1.12.23 | 11 June 2024 | {{site.data.keyword.bpshort}} CLI plugin enhances the display of `terraform.tfvars` file format during `--var-file` argument usage in [ibmcloud workspace apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) and [ibmcloud workspace plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command. The support for Internationalization (I18n) translation is updated.|
| 1.12.22 | 30 May 2024 | {{site.data.keyword.bpshort}} CLI plugin supports [`ibmcloud schematics agent destroy`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-destroy) to destroy the deployment resources. And set the `--force` parameter to **true** to delete all the agent flows to keep destroy parallel to workspace destroy flow.|
| 1.12.21 | 19 April 2024 | {{site.data.keyword.bpshort}} CLI plugin deprecates `--json` flag in all the CLI commands. Also fixed the `CLI v1.12.20` deprecation bug in the `ibmcloud schematics workspace refresh` or plan CLI commands.|
| 1.12.20 | 25 March 2024 | {{site.data.keyword.bpshort}} CLI plugin supports {{site.data.keyword.redhat_openshift_notm}} {{site.data.keyword.containershort_notm}}.|
| 1.12.18 | 08 March 2024 | Display the Terraform deprecation warning message during workspace commands using less than `terraform_v1.5`, Support for agent infrastructure update is removed, and fixed `index out of range` error by using `ibmcloud schematics state list` command.|
| 1.12.17 | 14 February 2024 | {{site.data.keyword.bpshort}} plug-in installation supports Cloud Shell, and [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-workspace-upload) command now supports the Cloud Shell commands.|
| 1.12.16 | 7 February 2024 | [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=api#schematics-workspace-list) supports caching for API versions. `terraform_v1.2`, `terraform_v1.3`, `terraform_v1.4` deprecation message are populated for creating the [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=api#schematics-workspace-new) templates.|
| 1.12.15 | 24 January 2024 | Support for `refresh_token` in [agent update API](/docs/schematics?topic=schematics-update-agent-overview&interface=api#update-agent-api) request, enhanced the version support for [agent update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update) command.|
| 1.12.14 | 10 January 2024 | Added new commands and translations to support the agent and policy. The system workspaces from the workspace list command output are hidden. Enhanced the agent job display on command output. Usage of `/v1/versions` API for the agent versions.|
| 1.12.12 | 17 September 2023 | {{site.data.keyword.bpshort}} Agent create and update added with a [`new flag --metadata`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-create) and a bug fix to configure an [HTTP timeout for request](/docs/schematics?topic=schematics-general-faq&interface=cli#http-api-call). |
| 1.12.10 | 22 May 2023 | {{site.data.keyword.bpshort}} [Agent update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update) and [`agent list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-list) command bug fixes to set the runtime errors. |
| 1.12.9 | 6 April 2023 | {{site.data.keyword.bpshort}} Agent beta-1 and policy CLI commands are enhanced to include the `-target-file`, and the `output` of [agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan), [agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply), and [agent health](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-health).  |
| 1.12.8 | 22 Mar 2023 | [{{site.data.keyword.bpshort}} Agent beta-1](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#agents-cmd) and [policy](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#policy-cmd) CLI commands are available in `us-south`, `us-east`, `eu-de`, `eu-gb` region.  |
| 1.12.7 | 07 Feb 2023 | Bug fix to disable `API_AGENT_ATTACHMENT` in `us-south`, `us-east`, `eu-de`, `eu-gb` region.  |
| 1.12.6 | 30 Jan 2023 | Enhanced complex input support through `yaml` file. Fixes related to the status output, index out of range for workspace action output, refresh token issue for long running and spinner panic fixes. |
| 1.12.5 | 18 Dec 2022 | The subcommand usage, and support for specifying complex inputs through a local YAML file by using `-input-file` option.|  
| 1.12.3 | 18 Nov 2022 |  Fixed subcommand usage support `source type`. |
| 1.12.3 | 3 Nov 2022 |  Enhanced CLI commands, with the latest SDK update, and the workspace action command update. |
| 1.12.2 | 11 Aug 2022 | Included `--output` flag and bug fixes for the commands in and released {{site.data.keyword.bpshort}} v1.12.2 plug-in.|
| 1.12.1 | 26 July 2022 | Incorporated the bugs and fixes commands in {{site.data.keyword.bpshort}}.|
| 1.12.0 | 11 July 2022 | Support for `agents` commands in {{site.data.keyword.bpshort}} from command-line.|
| 1.11.1 | 8 July 2022 | Support to fix the translation issue in {{site.data.keyword.bpshort}} from command-line.|
| 1.10.0 | 5 May 2022 | Support for `stop`, `force-stop`, and `terminate` in {{site.data.keyword.bpshort}} from command-line.|
| 1.9.0 | 25 April 2022 | Support for `Drift` detection in {{site.data.keyword.bpshort}} from command-line.|
| 1.8.1 | 17 April 2022 | Fixes alias deprecation display message for the {{site.data.keyword.bpshort}} JSON output.|
| 1.8.0 | 13 March 2022 | Supports passing `.tfvars` and `.json` files to plan and apply command. Usage of `ibmcloud terraform` command displays a warning message. The version also supports private {{site.data.keyword.bpshort}} endpoints through command-line and enhances the tabular view output to list the provisioned resource in {{site.data.keyword.bpshort}} workspace. |
| 1.7.3 | 4 March 2022 | Supports passing `vars` files to command-line plan command, displays `commit ID` in `ibmcloud schematics workspace get` command, and edited the `ibmcloud schematics workspace state show` command description. |
| 1.7.2 | 17 February 2022 | Supports Linux&trade; arm64 and Mac OS arm64 platform binaries. Fixes related to `stdout/stderr` stream, invalid `TF vars` file and translation are released. |
| 1.7.1 | 11 February 2022 | Support for trace logging and added integration tests for few commands. Fixes to update `env values metadata`, panic for invalid flags, and `ibmcloud schematics workspace output command` is unavailable. |
| 1.7.0 | 12 January 2022 | Displays Terraform v11.0 deprecation message after command execution. Fix command-line alias. Remove the appearance of the duplicate strings. Support global time in the log file. |
| 1.6.2 | 2 December 2021 | Support for non-English translations. Fix apply command `--var-file` and actions `--target not setting` argument. Fix a pipeline vulnerability.|
| 1.6.1 | 21 October 2021 | Supports `winrm` for {{site.data.keyword.bpshort}} actions. Added the `--inventory-connection-type`, `--bastion-credential-json` and `--credential-json` option value to the create and config updates. Updated non-English translations for the command-line. Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.|
| 1.6.0 | 29 September 2021 | Support for `linux-ppc64le`, and `linux-s390x` binaries. Lists `Terraform v1.0` in the details panel. Display `Terraform v0.11` deprecation message in {{site.data.keyword.bpshort}} workspace page. Fixed the resource query list command returns values as empty string.|
| 1.5.12 | 02 September 2021 | Suppress status message for `--output json` flag.|
| 1.5.11 | 27 August 2021 | Added a flag `--pull-latest` to existing workspace **update** command. Fixed `BNPP` issue. Fixed the locale translations.|
| 1.5.10 | 11 August 2021 | Supports `Terraform v0.15`. Fixed locale translations.|
| 1.5.9 | 13 July 2021 | Fixed locale translations.|
| 1.5.8 | 08 July 2021 | Fixed shared data sets API path. Disabled shared data sets commands.|
| 1.5.7 | 04 June 2021 | Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status. Fixed `ibmcloud schematics job run` command with `--input` flag description. Fixed `ibmcloud schematics job run` command with `--output json` flag description. Fixed `ibmcloud schematics action update` command with `--credentials` flag and the locale translations.|
| 1.5.6 | 03 June 2021 | Updated `ibmcloud schematics workspace new` command to support `Terraform v0.14` and the locale translations.|
{: caption="Command line version history" caption-side="bottom"}
