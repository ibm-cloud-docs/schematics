---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-13"

keywords: schematics command-line reference, schematics commands, schematics command-line, schematics reference, command-line

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.bplong_notm}} CLI
{: #schematics-cli-reference}

Run these commands when you want to automate your {{site.data.keyword.bplong_notm}} Workspaces, Actions, Jobs, resources.
{: shortdesc}	

## Prerequisites
{: #cli-prerequisites}

- Set up your [CLI](/docs/schematics?topic=schematics-setup-cli).
- Install [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).

Be sure to keep your CLI up-to-date so that you can use the current released commands and their options. For more information about the current command-line version releases, see [Command-line version history](/docs/schematics?topic=schematics-cli_version-releases).
{: important}

## Actions commands
{: #schematics-action-commands}

Review the commands that you want to create, update, list, delete, and work with your {{site.data.keyword.bpshort}} Actions.
{: shortdesc}

### Inventory host groups
{: #inventory-host-grps}

{{site.data.keyword.bplong_notm}} supports inventory host groups to group the applications hostname such as web server, database server, Operating System, region, or network. 

A host group is a collection of hosts that you can run your Ansible playbook against. A condition defines either a workspace or a query within a workspace. For instance, you can run your inventory against all the hosts in your `development` workspace, or against all hosts with a `webserver` tag in your `development` workspace. The host groups can be defined by using **Static inventory** or **Dynamic inventory** method. 

**Static inventory** allows to create the collection of hosts that you can run your Ansible playbook against. A condition defines either a workspace or a query within a workspace. For instance, you can run your inventory against all the hosts in your `dev` workspace, or against all hosts with a `webserver` tag in your `dev` workspace. You can also add multiple conditional target resources for your workspaces to run. 

**Dynamic inventory** allows to create the collection of hosts in a inventory file that defines the hosts and group of hosts upon which your playbook operates. The hostnames and IP addresses must be provided in an `hosts.ini` file. Follow the syntax and example for the `INI` file format that can be used in the `create` and `update` actions commands as `--TARGET-FILE <ABSOLUTE_PATH with FILE_NAME>` argument.

Syntax

```text
[hostgroupname1]
<IPaddress1> 
<IPaddress2> 
[hostgroupname2]
<IPaddress1>
```
{: codeblock}

Example 

```text
[webserverhost]
178.54.68.78
187.54.68.78
[dbhost]
174.45.86.87
```
{: codeblock}

| Target | Description| 
|------|  ------|
|`hostgroupname1`| The application hostname. For example, Web Server host application name as `[webserverhost]`, database hostname as `[dbhost]`, in a single word. Note the system validates and throws an error if a space is provided in the host group name.|
|`IPaddress`|The IP addresses of the hostname.|
{: caption="Inventory host group parameters" caption-side="top"}

You can set the proxy between an SSH client and the {{site.data.keyword.cloud_notm}} inventory resources where you want to run an Ansible playbook in the **{{site.data.keyword.cloud_notm}} resource inventory SSH key** field. This set up adds a layer of security to your {{site.data.keyword.cloud_notm}} resources, and minimize the surface of potential vulnerabilities. Note now the Actions support only `one SSH key` for all virtual server instances. The SSH key must contain `\n` at the end of the key details in case of command-line or API calls.
{: note}

### `ibmcloud schematics action create`
{: #schematics-create-action}

Create an action to run an Ansible playbook on a single target host or a group of target hosts. You use Ansible playbooks to perform cloud operations or install software on cloud resources. To try out this capability or to get started, use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. You can create an action by using a payload file or the command's interactive mode.
{: shortdesc}

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

Syntax

```sh
ibmcloud schematics action create --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--credential CREDENTIAL_FILE] [--credential-json CREDENTIAL_JSON_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--bastion-credential-json BASTION_CREDENTIAL_JSON_FILE] [--inventory INVENTORY_ID] [—-inventory-connection-type INVENTORY_CONNECTION_TYPE] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--github-token GITHUB_ACCESS_TOKEN] [--output OUTPUT] [--file FILE_NAME ] [--json] [--no-prompt]
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
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--file` or `-f` | Required | The path to the JSON payload file containing the definition of the action that you want to create. For more information, see [Using a payload file](/docs/schematics?topic=schematics-schematics-cli-reference#create-action-payload). |
| `--no-prompt` | Optional | Set this flag to run the command without an interactive mode. |
{: caption="{{site.data.keyword.bpshort}} Actions create flags" caption-side="top"}

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
6. If applicable, enter the personal access token that you want to use to access your GitHub repository. Then, press the return key. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning.
7. Enter the name of the Ansible playbook that you want to run and press the return key. 
8. Review the details of the action that was created for you. 


### `ibmcloud schematics action update`
{: #schematics-update-action}

Update the information of an existing action by using an action ID. Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}

Syntax

```sh
ibmcloud schematics action update --id ACTION_ID --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--github-token GITHUB_ACCESS_TOKEN] [--credential CREDENTIAL_FILE] [--credential-json CREDENTIAL_JSON_FILE] [--bastion BASTION_HOST_IP_ADDRESS] [--bastion-credential-json BASTION_CREDENTIAL_JSON_FILE] [--inventory INVENTORY_ID] [--inventory-connection-type INVENTORY_CONNECTION_TYPE] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--file FILE_NAME] [--no-prompt] [--output OUTPUT] [--json]
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
| `--github-token` or `-g` | Optional | The personal access token in GitHub that you want to use to connect to a private GitHub repository. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning.|
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
| `--json` or `--j` | Deprecated | Prints the output as JSON. Use `--output` JSON instead. | 
{: caption="{{site.data.keyword.bpshort}} Actions update flags" caption-side="top"}


Example

```sh
ibmcloud schematics action update --id us-south.workspace.101010101 --description "This is my description" 
```
{: pre}


### `ibmcloud schematics action get`
{: #schematics-get-action}

Retrieve the detailed information of an existing {{site.data.keyword.bpshort}} Actions by using an action ID. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics action get --id ACTION_ID [--profile PROFILE] [--output OUTPUT] [--json] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of an action that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
| `--no-prompt` | Optional |Set this flag to run the command without the interactive mode. |
{: caption="{{site.data.keyword.bpshort}} Actions get flags" caption-side="top"}

Example

```sh
ibmcloud schematics action get --id us-south.workspace.101010101 -p summary 
```
{: pre}

### `ibmcloud schematics action list`
{: #schematics-list-action}

Retrieve a list of all actions in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics action list [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT]  [--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional | The maximum number of actions that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the action in the list of actions from where you want to start listing your actions. For example, if you have three actions in your account, the command returns these actions as a list with three elements. To retrieve all actions, you must enter position number 0. To retrieve actions number 2 and 3 and leave out action number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of actions. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional |The depth of information that is returned. Supported values are `ids`, and `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Actions list flags" caption-side="top"}

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
{: caption="{{site.data.keyword.bpshort}} Actions delete flags" caption-side="top"}

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
{: caption="{{site.data.keyword.bpshort}} Actions upload flags" caption-side="top"}

Example

```sh
ibmcloud schematics action upload --id us.ACTION.testphase1.2eddf83a --file <FILE_PATH>/mytestactionupload.tar
```
{: pre}


## Agent beta-1 commands
{: #agents-cmd}

{{site.data.keyword.bplong_notm}} Agent beta-1 delivers a simplified agent installation process and policy for agent assignment.. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that is available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

### `ibmcloud schematics agent apply`
{: #schematics-agent-apply}

Deploy or upgrade an agent to force deploy. For more information about the steps to use apply command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent apply --id AGENT_ID [--force-redeploy] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of an agent. |
| `--force-redeploy` or `-f` | Optional | Force redeploys an Agent. |
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
{: caption="{{site.data.keyword.bpshort}} agent apply flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent apply --id <provide your agent id>
```
{: pre}

### `ibmcloud schematics agent create`
{: #schematics-agent-create}

Create an agent by using {{site.data.keyword.bpshort}}. Agents help you run your Terraform or Ansible jobs on your infrastructure. For more information about the steps to use create command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent create --name AGENT_NAME --location LOCATION --agent-location AGENT_LOCATION --version VERSION --infra-type INFRA_TYPE --cluster-id CLUSTER_ID --cluster-resource-group CLUSTER_RESOURCE_GROUP --cos-instance-name COS_INSTANCE_NAME --cos-bucket COS_BUCKET --cos-location COS_LOCATION --resource-group RESOURCE_GROUP [--description DESCRIPTION] [--plan-only] [--plan-apply] [--tags TAGS] [--file FILE] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | The unique name of an agent. Must be descriptive of the agent role, location and usage.  |
| `--location` or `-l` | Required | The {{site.data.keyword.bpshort}} location where the agent will be defined, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for execution. |
| `--agent-location` or `--al` | Required | A descriptive user defined label to identify where the agent is deployed in the user environment. This could be a Cloud region or a user data center.  For example, `London MZR`. |
| `--version` or `-v` | Required | A user defined label specifying the version of the agent. |
| `--infra-type` or `-i` | Required | Specify the type of the target agent infrastructure. Supported values are `ibm-kubernetes`, `ibm-openshift`, or `ibm-satellite`.|
| `--clusterid` or `-k` | Required | The ID of the Kubernetes cluster for deploying an Agent.|
| `--cluster-resource-group` or `--kr` | Required | The name of the clusters' resource group. |
| `--cos-instance-name` or `--on` | Required | The name of the COS instance. |
| `--cos-bucket` or `-b` | Required |  The ID or the name of the COS bucket. |
| `--resource-group` or `-r` | Required | Resource group name or ID the agent will be associated with. |
| `--description` or `-d` | Optional | A description that identifies the agent usage, and the network zones and resources the agent is able to access. |
| `--plan-only` | Optional | Run plan command, after creating the agent.|
| `--plan-apply` | Optional | Run plan and apply command, after creating the agent.|
| `--tags` or `-t`| Optional | Agent tags. This flag can be repeated multiple times. Tags allow for faster and easier search for agent related resources. |
| `--file` or `f` | Optional | Path to a JSON file containing the definition of an agent. |
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
{: caption="{{site.data.keyword.bpshort}} agent create flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent create --name agenttestcli --location us-south --agent-location us-south --version v1.0.0 --infra-type ibm_kubernetes --cluster-id c6ja7vtf0afldib61l20 --cluster-resource-group Default --cos-instance-name COSForAgentLogging --cos-bucket agentlogs --cos-location us-south --resource-group Default
```
{: pre}

### `ibmcloud schematics agent delete`
{: #schematics-agent-delete}

Uninstall an agent. For more information about the steps to use delete command, see [deleting an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent delete --id AGENT_ID [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of an agent. |
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
| `--no-prompt` | Optional | Set this flag to stop interactive CLI session such as, prompting user for input a field value on terminal. |
{: caption="{{site.data.keyword.bpshort}} agent delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent delete --id <AGENT_ID>
```
{: pre}


### `ibmcloud schematics agent get`
{: #schematics-agent-get}

Retrieves the details of an agent. Agents help you to fetch your workspace jobs on your infrastructure. For more information about the steps to use get command, see [displaying an agent](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent get --id AGENT_ID [--verbose VERBOSE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | ID of the agent. |
| `--profile` or `-p` | Optional | Level of details to return. Valid values are `summary`, `detailed`, or `ids`. Defaults to `summary`. |
| `--verbose` or `-v` | Optional | Sets the verbosity level to get the relevant details about an agent. Valid values are `all`, `assignment-policy`, `prs-result`, `deploy-result`, `health-result`. The default value is `all`. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
| `--no-prompt` | Optional | Set this flag to stop interactive CLI session such as, prompting user for input a field value on terminal. |
{: caption="{{site.data.keyword.bpshort}} agent get flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent get --id <Provide your agent ID>
```
{: pre}

### `ibmcloud schematics agent health`
{: #schematics-agent-health}

Performs the post deployment validation of an agent. For more information about the steps to use health command, see [Monitoring agent health](/docs/schematics?topic=schematics-agentb1-health&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent health --id AGENT_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the agent. |
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
{: caption="{{site.data.keyword.bpshort}} agent health flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent health --id <AGENT_ID>
```
{: pre}

### `ibmcloud schematics agent list`
{: #schematics-agent-list}

Lists the agents. Defaults to show the registered agents. For more information about the steps to use list command, see [displaying an agent](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent list [--location LOCATION] [--limit LIMIT] [--offset OFFSET] [--output OUTPUT_FORMAT
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--location` or `-l` | Optional |  Geographic locations supported by {{site.data.keyword.bplong_notm}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`. |
| `--filter` | Optional | Set this flag to filter agents. Valid values are `all`, `saved`, or `new`. If unset defaults to `saved`. |
| `--limit` or `-lm` | Optional | Maximum number of agents to list. Ignored if a negative number is set. Maximum limit is 200, (default is -1). |
| `--offset` or `-m`| Optional | Offset in list. Ignored if a negative number is set (default: -1). |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
{: caption="{{site.data.keyword.bpshort}} agent list flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent list
```
{: pre}

### `ibmcloud schematics agent plan`
{: #schematics-agent-plan}

Checks for the pre-requisites of the pre-created agent before install. Generates an agent plan by using {{site.data.keyword.bpshort}}. For more information about the steps to use plan command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).
{: shortdesc}

Syntax

```sh
ibmcloud schematics agent plan --id AGENT_ID [--output OUTPUT] 
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the agent. |
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
{: caption="{{site.data.keyword.bpshort}} agent plan flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent plan
```
{: pre}

### `ibmcloud schematics agent update`
{: #schematics-agent-update}

Update an agent. For more information about the steps to use apply command, see [deploying agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).

Syntax

```sh
ibmcloud schematics agent update --id AGENT_ID [--description DESCRIPTION] [--user-state USER_STATE] [--tags TAGS] [--file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of an agent. |
| `--tags` or `-t` | Optional | Agent tags. This flag can be used multiple times, and search the agent related resources faster.|
| `--description` or `-d` | Optional | Short description of an agent.|
| `--user-state` or `-s` | Optional | User defined status of an agent. Valid values are `enable`, `disable`. |
| `--file` or `-f` | Optional | Path to the `JSON` file that contains the definition of the agent.|
| `--output` or `-o` | Optional | Specify output format, only 'JSON' is supported. |
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} agent update flags" caption-side="top"}

Example

```sh
ibmcloud schematics agent update --id <AGENT_ID>
```
{: pre}



## Blueprint commands
{: #blueprints-cmd}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

### `ibmcloud schematics blueprint create`
{: #schematics-blueprint-create}

Create a blueprint config from the command line using the `ibmcloud schematics blueprint create` command. Creating a blueprint config is the first step to deploy a blueprint environment. The blueprint config is created from user provided parameters via the command line that specifies the source of the blueprint template in a Git repository, and input values sourced from Git hosted input files and locally sourced input values.

The create command supports passing all required parameters through CLI and passing input values from local files. 

A experimental file option is provided to create a blueprint config from a configuration provided as a JSON file from the local file system. Refer to the [`ibmcloud schematics blueprint create - file option`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) section. These two usage modes are mutually exclusive. 
{: shortdesc}

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.5` version.
{: important}

Refer to the section on [Creating Blueprints](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) for examples of command syntax and output.

Before you begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.5`.
- Select the {{site.data.keyword.cloud_notm}} region that you wish to use to manage your {{site.data.keyword.bpshort}}. Set the region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command. For more information, see [FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-target-region).
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

**Syntax to create blueprint config using CLI parameters:**

```sh
ibmcloud schematics blueprint create --name BLUEPRINT_NAME [--resource-group RESOURCE_GROUP] [--description BLUEPRINT_DESCRIPTION] [--location BLUEPRINT_LOCATION] [--source-type BLUEPRINT_SOURCE_TYPE] --bp-git-url BLUEPRINT_GIT_URL [-—bp-git-file BLUEPRINT_GIT_FILE] [--bp-git-branch BLUEPRINT_GIT_BRANCH] [--bp-git-release BLUEPRINT_GIT_RELEASE] [--input-git-url INPUT_GIT_URL] [-—input-git-file INPUT_GIT_FILE] [--input-file BLUEPRINT_INPUT_FILE] [--input-git-branch INPUT_GIT_BRANCH] [--input-git-release INPUT_GIT_RELEASE] [--inputs BLUEPRINT_INPUT_LIST] [--bp-git-token BLUEPRINT_GIT_TOKEN] [--input-git-token BLUEPRINT_INPUT_GIT_TOKEN] | --file CONFIG_FILE_PATH [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name` or `-n`| Required | Name of the blueprint. Delimited by double quotes if the name contains spaces. For example `-name "my blueprint"`|
| `--resource-group` or `-r` | Required | The management resource group for the blueprint.|
| `--description` or `--desc` | Optional | The description of the blueprint. Delimited by double quotes if the description contains spaces. For example `-description "my blueprint example"`|
| `--location` or `-l` |  Optional |  The location of blueprint. Select the {{site.data.keyword.cloud_notm}} region that you wish to use to manage your {{site.data.keyword.bpshort}}. Set the region through [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command. |
| `--source-type` or `-s`| Optional |  The blueprint source type. Valid values are `git_hub`, `ibm_cloud_catalog`.|
| `--bp-git-url` or `--bu` | Required | The blueprint Git URL. This is the URL of the repository containing the blueprint template. For example `-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example` |
| `--bp-git-file` or `--bf`| Required | The blueprint template file name, including the file extension and any sub directory. For example `-bp-git-file <subfolder>/basic-blueprint.yaml`   |
| `--bp-git-branch` or `--bb`| Optional | The blueprint Git branch name. Mutually exclusive with the `-bp-git-release` option. If both options are not specified, the branch defaults to `main`. For example `-bp-git-branch devhardening`|
| `--bp-git-release` or `--br`| Optional | A Git release tag identifying the version of the template file. Mutually exclusive with the `-bp-git-branch` option. For example `-bp-git-release 1.4.2`|
|  `--bp-git-token` or `--bg` | Optional | The GitHub token value to access the private Git repository. |
| `--input-git-url` or `--igu`| Optional | The input Git URL. This is the URL of the repository containing the blueprint input file . For example `-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example` |
| `--input-git-file` or `--igf`| Optional | The input file name, including extension and any sub directory. For example `-input-git-file <subfolder>/basic-input.yaml`. Refer to [Blueprints input file YAML Schema](/docs/schematics?topic=schematics-bp-input-schema-yaml) for the file format. |
| `--input-git-branch` or `--igb`| Optional |The input file Git branch name. Mutually exclusive with the `-input-git-release` option. If both options are not specified, the branch defaults to `main`. For example `-input-git-branch nextdrop` |
| `--input-git-release` or `--igr`| Optional | A Git release tag identifying the version of the input file. Mutually exclusive with the `-input-git-branch` option. For example `-input-git-release 1.0.5` |
| `--input-git-token` or `--ig` | Optional | A GitHub token value to access the private input Git repository.|
| `--inputs` or `--in` | Optional | The input variables for the blueprint. This flag can be used multiple times. For example, '-inputs key=value -inputs key1=value1'. This flag only supports simple strings without spaces. This option can be combined with the `--input-file` option to provide a complete set of simple and complex values.  |
| `--input-file` or `--if` | Optional | The input variables for the blueprint from a local YAML file. The path and name to a local YAML file containing input values. This input method supports passing complex Terraform values, that are split across lines, containing spaces and special characters. Refer to [Blueprints input file YAML Schema](/docs/schematics?topic=schematics-bp-input-schema-yaml) for the file format. Variable format is the same as used for `--input-git-file`. This option can be used for local testing of the Git input file or overrides on the Git input file. |
| `--file` or `-f` | Optional | The path to the JSON file containing the configuration of the blueprint.|
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint create flags" caption-side="bottom"}

Example

```sh
ibmcloud schematics blueprint create -name "blueprint basic" -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file example/basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file example/basic-input.yaml -inputs provision_rg=true -inputs resource_group_name=mynewrgdemo -input-file input.yaml 
```
{: pre}

### `ibmcloud schematics blueprint apply`
{: #schematics-blueprint-apply}

The apply command creates the cloud resources defined by the blueprint template. Apply performs Terraform `Apply` operations for each module, using the Terraform configuration specified in the blueprint template to create the cloud resources.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint apply --id BLUEPRINT_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the blueprint.|
| `--output` or  `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="blueprints apply flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint apply --id Blueprint_Basic.eaB.5cd9
```
{: pre}

Output

```text
Modules to be applied
SNO   Module Type   Name                   Status   
1     Workspace     basic-resource-group   INITIALISED   
2     Workspace     basic-cos-storage      INITIALISED   
      
Blueprint job us-east.JOB.blueprint_Basic.c1f2164f started at 2022-11-21 12:15:25

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.blueprint_Basic.c1f2164f completed at 2022-11-21 12:18:54

Module Type   Name                   Status               Job ID   
Blueprint     blueprint_Basic        job_finished   us-east.JOB.blueprint_Basic.c1f2164f   
Workspace     basic-resource-group   APPLIED                 
Workspace     basic-cos-storage      APPLIED                 
              
Blueprint ID blueprint_Basic.eaB.435a job_finished at 2022-11-21 12:18:55
OK
```
{: screen}

### `ibmcloud schematics blueprint update`
{: #schematics-blueprint-update}

You update the blueprint configuration in {{site.data.keyword.bpshort}} with changes to the inputs and blueprint template with the `ibmcloud schematics blueprint update` command. Changes are only applied to the deployed resources by running the the `ibmcloud schematics blueprint apply` after the update has been performed.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint update --id BLUEPRINT_ID [--file CONFIG_FILE_PATH] [[--inputs INPUT_VARIABLES_LIST]] [--input-file BLUEPRINT_INPUT_FILE] [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the blueprint.|
| `--inputs` or `--in` | Optional | Update values for input variables. This flag can be used multiple times. For example, '-inputs foo=bar -inputs foo1=bar1'. This flag only supports simple strings without spaces. This option can be combined with the `--input-file` option. |
| `--file` or `-f` | Optional |  The config path to the JSON file containing the template of the blueprint to update.|
| `--input-file` or `--if` | Optional | Update values for input variables for the blueprint from a local YAML file. The path and name to a local YAML file containing input values. This input method supports passing complex Terraform values, that are split across lines, containing spaces and special characters. Refer to [Blueprints input file YAML Schema](/docs/schematics?topic=schematics-bp-input-schema-yaml) for the file format. |
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprints config update flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint update -id Blueprint_Basic.eaB.5cd9 -inputs key1=value1 
```
{: pre}

Output

```text
Update blueprint  blueprint_Basic

Modules to be updated
SNO   Module Type   Name                   Updates   
1     Workspace     basic-resource-group   NA   
2     Workspace     basic-cos-storage      Updating   
      
Blueprint job us-east.JOB.blueprint_Basic.0862fea7 started at 2022-11-21 12:13:24

Module job execution status
Waiting:0    In Progress:0    Success:0    Failed:0   

Blueprint job us-east.JOB.blueprint_Basic.0862fea7 completed at 2022-11-21 12:14:53

Module Type   Name                   Status           Job ID   
Blueprint     blueprint_Basic        UPDATE_SUCCESS   us-east.JOB.blueprint_Basic.0862fea7   
Workspace     basic-resource-group   INITIALISED         
Workspace     basic-cos-storage      INITIALISED         
              
Blueprint ID blueprint_Basic.eaB.435a update_success at 2022-11-21 12:14:55
OK
```
{: screen}

### `ibmcloud schematics blueprint get`
{: #schematics-blueprint-get}

Using the `ibmcloud schematics blueprint get` command you can display the configuration of a blueprint.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint get --id BLUEPRINT_ID [--level LEVEL] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the blueprints. |
| `--level` or `-l` | Optional | Level of details to return. Valid values are `summary`, `detailed`, `modules`, and `outputs`. The default value is **summary**. |
| `--output` or  `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint get flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint get --id Blueprint_Basic.eaB.5cd9
```
{: pre}

Output

```text
BLUEPRINT          
                
Name            Blueprint Basic Json   
ID              blueprint_Basic.eaB.435a   
Description     Deploys a simple two module blueprint Updated   
Status          create_success   
Location        us-east   
Creator         test@in.ibm.com   
Last modified   2022-11-21T08:24:23.197Z   
                
MODULES

SNO   Workspace Name         Workspace ID                                      Status        Location   Updated   
1     basic-resource-group   us-east.workspace.basic-resource-group.3a89a5f1   INITIALISED   us-east       
2     basic-cos-storage      us-east.workspace.basic-cos-storage.c775f80a      INITIALISED   us-east       
      
OK
```
{: screen}

### `ibmcloud schematics blueprint list`
{: #schematics-blueprint-list}

You can list all the blueprints.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint list [--profile PROFILE] [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--profile` or `-p`| Optional |  The level of details to return. Valid values are `summary` and `ids`. Default value is `summary`.|
| `--limit` or `-l` | Optional | The maximum number of blueprints to list. Ignored if a negative number is set. Maximum number to list is `200`, the default is `-1`.|
| `--offset` or `-m` | Optional | Offset in list. Ignored if a negative number is set. The default value is `-1`.|
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint list flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint list
```
{: pre}

Output

```text
Name              ID                         Source Type   Status               Location   Creator           Last modified   
Blueprint Basic   blueprint_Basic.eaB.435a   GitHub        job_finished   us-east    test@in.ibm.com   2022-11-21T10:45:57.329Z   
                                                           
OK
```
{: screen}

### `ibmcloud schematics blueprint destroy`
{: #schematics-blueprint-destroy}

Destroys all the resources associated with the modules in a blueprint. This action cannot be reversed. On workspaces {{site.data.keyword.bpshort}} performs a Terraform Destroy operation.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint destroy --id BLUEPRINT_ID [--no-prompt] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the blueprint.|
| `--no-prompt` | Optional |Set this flag to stop interactive command-line session. |
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprints destroy flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint destroy -id blueprint_Basic.eaB.435a
```
{: pre}

Output

```text
Modules to be destroyed
SNO   Module Type   Name                   Status   
1     Workspace     basic-resource-group   APPLIED   
2     Workspace     basic-cos-storage      APPLIED   
      
Do you really want to destroy all the resources of blueprint blueprint_Basic.eaB.435a? [y/N]> y
Blueprint job us-east.JOB.Blueprint-Basic-Json.59bf17a8 started at 2022-11-21 12:47:26

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint-Basic-Json.59bf17a8 completed at 2022-11-21 12:50:55

Module Type   Name                   Status               Job ID   
Blueprint     Blueprint Basic Json   job_finished   us-east.JOB.Blueprint-Basic-Json.59bf17a8   
Workspace     basic-resource-group   INITIALISED             
Workspace     basic-cos-storage      INITIALISED             
              
Blueprint ID blueprint_Basic.eaB.435a job_finished at 2022-11-21 12:50:56
OK
```
{: screen}


### `ibmcloud schematics blueprint delete`
{: #schematics-blueprint-delete}

Delete a blueprint. A blueprint can only be deleted once all resources have been destroyed using the `destroy` command and workspaces are in `Inactive` state. For more information about the difference between destroy and delete command, see [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint delete --id BLUEPRINT_ID [—force-delete] [--no-prompt] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the blueprint.|
| `--force-delete` or `--fd` | Optional | Force deletes the blueprint including the active module. This action cannot be reversed.|
| `--no-prompt` | Optional |Set this flag to stop interactive command-line session. |
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint delete -id blueprint_Basic.eaB.5cd9
```
{: pre}

Output

```text
Modules to be deleted
SNO   Module Type   Name                   Status   
1     Workspace     basic-resource-group   INITIALISED   
2     Workspace     basic-cos-storage      INITIALISED   
      
Do you really want to delete the blueprint ? [y/N]> y
Job : us-east.JOB.Blueprint-Basic-Json.c64c45a1 Created

Job Type: BLUEPRINT DELETE

OK
```
{: screen}

### `ibmcloud schematics blueprint job get`
{: #schematics-blueprint-job-get}

Get information of a blueprint job.
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint job get --id JOB_ID [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the job that you want to get. Run `ibmcloud schematics blueprint job list` to get Job ID of a particular blueprint job.|
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint job get flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint job get --id us-east.JOB.Blueprint-Basic-Json.c64c45a1
```
{: pre}

Output

```text
BLUEPRINT JOB DETAILS      
Job ID                  us-east.JOB.Blueprint-Basic-Json.c64c45a1   
Blueprint ID            blueprint_Basic.eaB.435a   
Job Type                blueprint_delete   
Location                us-east   
Start Time              2022-11-21 07:25:32   
End Time                0001-01-01 00:00:00   
Status                  In Progress   
                           
SNO   Child Job          Module ID   Job Status        Job ID   
1     blueprint_delete               job_in_progress   us-east.JOB.Blueprint-Basic-Json.c64c45a1   
                         
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 1
                 
Module ID        
Status        job_in_progress   
Log Summary   (last few lines)..........  
               2022/11/21 07:25:35 -----  New blueprint Action  -----   
               2022/11/21 07:25:35 Request: blueprintId=blueprint_Basic.eaB.435a, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=test@in.ibm.com, requestID=a2a9922b-5a79-4700-88e3-efb2d3194fa9   
               2022/11/21 07:25:36 Related Job:  jobID=us-east.JOB.Blueprint-Basic-Json.c64c45a1   
                 
                 
Use ibmcloud schematics blueprint job logs --id us-east.JOB.Blueprint-Basic-Json.c64c45a1 to review the full job output 
OK
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> N
```
{: screen}

### `ibmcloud schematics blueprint job list`
{: #schematics-blueprint-job-list}

List all the Jobs for a blueprint. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint job list --id BLUEPRINT_ID [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the blueprint. |
| `--limit` or `-l` | Optional | The maximum number of jobs to list. Ignored if a negative number is set. Maximum number to list is `200`, the default is `-1`|
| `--offset` or `-m` | Optional | Offset in list. Ignored if a negative number is set. The default value is `-1`.|
| `--output` or  `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint job list flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint job list --id Blueprint-Basic-Json.eaB.937a
```
{: pre}

Output

```text
ID     Blueprint-Basic-Json.eaB.937a   
JOBS      
SNO   Job Type                Status   Start Time            Job ID   
1     blueprint_create_init   Normal   2022-11-21 08:22:47   us-east.JOB.Blueprint-Basic-Json.af289c3f   
      
Enter Job sequence number to get the blueprint job summary.(or enter no/n to ignore)> 1
BLUEPRINT JOB DETAILS      
Job ID                  us-east.JOB.Blueprint-Basic-Json.af289c3f   
Job Type                blueprint_create_init   
Status                  Normal   
Start Time              2022-11-21 08:22:47   
End Time                2022-11-21 08:24:15   
                           
SNO   Child Job               Module ID                                         Job Status     Job ID   
1     blueprint_create_init                                                     job_finished   us-east.JOB.Blueprint-Basic-Json.af289c3f   
2     Module_apply            us-east.workspace.basic-resource-group.3a89a5f1   job_finished      
3     Module_apply            us-east.workspace.basic-cos-storage.c775f80a      job_finished      
                              
Enter Job sequence number to get blueprint child job output summary(or enter no/n to ignore)> 1
                 
Module ID        
Status        job_finished   
Log Summary   (last few lines)..........  
              =Blueprint-Basic-Json.eaB.937a, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=test@in.ibm.com, requestID=f0d63272-974a-44a7-acc1-e18a99b75a9b   
               2022/11/21 08:22:51 Related Job:  jobID=us-east.JOB.Blueprint-Basic-Json.af289c3f   
               2022/11/21 08:23:05  --- Ready to execute the blueprint flow init command ---    
               2022/11/21 08:23:09 Status of create/update module request for moduleID  is 201   
               2022/11/21 08:23:15 Status of module for moduleID  is DRAFT   
               2022/11/21 08:23:27 Status of module for moduleID  is CONNECTING   
               2022/11/21 08:23:38 Status of module for moduleID  is INACTIVE   
               2022/11/21 08:23:45 Status of create/update module request for moduleID  is 201   
               2022/11/21 08:23:50 Status of module for moduleID  is DRAFT   
               2022/11/21 08:24:02 Status of module for moduleID  is CONNECTING   
               2022/11/21 08:24:14 Status of module for moduleID  is INACTIVE   
               2022/11/21 08:24:15  ENVIRONMENT_INIT - ENVIRONMENT_SYSTEM_STATEENUM_CREATE_SUCCESS    
               2022/11/21 08:24:15 Done with the blueprint init flow job   
                 
                 
Use ibmcloud schematics blueprint job logs --id us-east.JOB.Blueprint-Basic-Json.af289c3f to review the full job output 
OK
```
{: screen}


### `ibmcloud schematics blueprint job logs`
{: #schematics-blueprint-job-logs}

Get logs of a single blueprint job. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint job logs --id JOB_ID
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | The ID of the job that you want to get. Run `ibmcloud schematics blueprint job get` to get a particular blueprint job logs. |
{: caption="{{site.data.keyword.bpshort}} blueprint job list flags" caption-side="top"}

Example

```sh
ibmcloud schematics blueprint job logs --id us-east.JOB.Blueprint-Basic-Json.af289c3f
```
{: pre}

```text
 2022/11/21 08:22:51 -----  New blueprint Action  -----
 2022/11/21 08:22:51 Request: blueprintId=Blueprint-Basic-Json.eaB.937a, account=1f7277194bb748cdb1d35fd8fb85a7cb, owner=test@in.ibm.com, requestID=f0d63272-974a-44a7-acc1-e18a99b75a9b
 2022/11/21 08:22:51 Related Job:  jobID=us-east.JOB.Blueprint-Basic-Json.af289c3f
 2022/11/21 08:23:05  --- Ready to execute the blueprint flow init command --- 
 2022/11/21 08:23:09 Status of create/update module request for moduleID  is 201
 2022/11/21 08:23:15 Status of module for moduleID  is DRAFT
 2022/11/21 08:23:27 Status of module for moduleID  is CONNECTING
 2022/11/21 08:23:38 Status of module for moduleID  is INACTIVE
 2022/11/21 08:23:45 Status of create/update module request for moduleID  is 201
 2022/11/21 08:23:50 Status of module for moduleID  is DRAFT
 2022/11/21 08:24:02 Status of module for moduleID  is CONNECTING
 2022/11/21 08:24:14 Status of module for moduleID  is INACTIVE
 2022/11/21 08:24:15  ENVIRONMENT_INIT - ENVIRONMENT_SYSTEM_STATEENUM_CREATE_SUCCESS 
 2022/11/21 08:24:15 Done with the blueprint init flow job

OK

```
{: screen}

## Policy commands
{: #policy-cmd}

{{site.data.keyword.bpshort}} Policy is a beta feature that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

### `ibmcloud schematics policy create`
{: #schematics-policy-create}

Create a policy using {{site.data.keyword.bpshort}} to select one or more {{site.data.keyword.bpshort}} objects (such as, Workspaces, Action, Blueprint) to deliver targeted {{site.data.keyword.bpshort}} feature. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy create --name POLICY_NAME --kind POLICY_KIND --location LOCATION [--description DESCRIPTION] --resource-group RESOURCE_GROUP [--tags TAGS] [--target-file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | The unique name of the policy. |
| `--kind` or `-K` | Policy kind for managing and deriving policy decision. Supported is `agent_assignment_policy`. |
| `--location` or `-l` | Optional | Geographic location of {{site.data.keyword.bpshort}} service where the agent is defined. For example, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for processing. |
| `--description` or `-d` | Optional |  The description of the {{site.data.keyword.bpshort}} policy. |
| `--resource-group` or `-r` | Required | Resource group name or ID for the policy. |
| `--tags` or `-t`| Optional | Tags can be used multiple times to search for and locate agent policies faster. |
| `--target-file` or `tf` | Optional | Path to the JSON file containing the definition of the policy. |
| `--output` or `-o` | Optional | Specify output format, only `JSON` is supported. |
| `--no-prompt` | Optional |  Set this flag to run the command without user prompts. |
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
					"test"
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

```sh
ibmcloud schematics policy create --name policy-101 --kind agent_assignment_policy --location us-south --resource-group Default --target-file policy.json
```
{: pre}

### `ibmcloud schematics policy get`
{: #schematics-policy-get}

Retrieve the detailed information of an existing {{site.data.keyword.bpshort}} policy by using policy ID.
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

Retrieve a list of all policies in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy list [--profile PROFILE] [--output OUTPUT] [--limit LIMIT] [--offset OFFSET]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--profile` or `-r` | Optional | Geographic region supported by {{site.data.keyword.bpshort}} service such as, `us-south`, `us-east`, `eu-de`, `eu-gb`.|
| `--limit` or `-l` | Optional |  Maximum number of policies to list. Ignored if a negative number is set. The number must be a positive integer between 1 and 200. The default value is `-1`.|
| `--offset`or `-m`| Optional | Offset in list. Ignored if a negative number is set. The default value is `-1`.|
| `--output` or `-o` |  Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} Policy list flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy list 
```
{: pre}

### `ibmcloud schematics policy update`
{: #schematics-policy-update}

Update the information of an existing policy by using policy ID. Changes are applied to the description by running the `ibmcloud schematics policy update` command.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy update --id POLICY_ID --kind POLICY_KIND --location LOCATION [--description DESCRIPTION] --resource-group RESOURCE_GROUP [--tags TAGS] [--target-file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
|   `--id` or `-i` | Required | ID of the policy. |
|   `--kind` or `-k` | Optional | Policy kind for managing and deriving policy decision. Supported is `agent_assignment_policy`. |
|   `--location` or `-l` | Optional | Geographic locations supported by {{site.data.keyword.bpshort}} service. For example, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for processing. |
|   `--description` or `-d` | Optional |  The description of {{site.data.keyword.bpshort}} customization policy. |
|   `--resource-group` or `-r` | Optional |  Resource group name or ID for the policy. |
|   `--tags` or `-t` | Optional |     Policy tags. This flag can be used multiple times to search for and locate agent policies faster. |
|   `--target-file` or `-tf`  | Optional |    Path to the JSON file containing the definition of the policy. |
|   `--output` or `-o`  | Optional |  Specify output format, only `JSON` is supported. |
|   `--no-prompt`  | Optional |       Set this flag to stop interactive CLI session, such as prompting user for input a field value on terminal. |
{: caption="{{site.data.keyword.bpshort}} policy update flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy update --id policy-msk.soP.5c65 --description PolicyDescriptionUpdated
POLICY DETAILS
Name         ID                    Description                Kind                      Location   Resource Group   CRN   Target   Tags   
policy-msk   policy-msk.soP.5c65   PolicyDescriptionUpdated   agent_assignment_policy   us-south   Default                            
                                   
OK
```
{: pre}

### `ibmcloud schematics policy delete`
{: #schematics-policy-delete}

Delete a {{site.data.keyword.bpshort}} policy.
{: shortdesc}

Syntax

```sh
ibmcloud schematics policy delete [--id POLICY_ID]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | ID of the policy. |
| `--no-prompt` | Optional | Set this flag to run the command without user prompts. |
{: caption="{{site.data.keyword.bpshort}} policy delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics policy delete --id policy-101.soP.282e
```
{: pre}

## Enable BYOK or KYOK commands
{: #kms-commands}

You can use your encryption keys from key management services (KMS), {{site.data.keyword.keymanagementservicelong_notm}}(BYOK), and {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} (KYOK) to encrypt and secure data stored in {{site.data.keyword.bpshort}}. For more information about how to protect sensitive data in {{site.data.keyword.bpshort}}, see [protecting your sensitive data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#data-storage).
{: shortdesc}

### Prerequisites
{: #key-prerequisites}

The key management system lists the instance that are created from your specific location and region. Following prerequisites are followed to perform the KMS activity.

- You should have your `KYOK`, or `BYOK`. To create the {{site.data.keyword.keymanagementservicelong_notm}} keys, see [create KYOK root key by using UI](/docs/key-protect?topic=key-protect-create-root-keys). To create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} keys, see [create BYOK root key by using UI](/docs/hs-crypto?topic=hs-crypto-create-root-keys).
- You need to [add root key](/docs/key-protect?topic=key-protect-import-root-keys#import-root-key-gui) to {{site.data.keyword.bpshort}} services.
- You need to configure [service to service authorization](/docs/account?topic=account-serviceauth#create-auth) to integrate `BYOK`, and `KYOK` in {{site.data.keyword.bpshort}} service.


KMS setting is a one time settings. You need to open the [support ticket](/docs/get-support?topic=get-support-using-avatar) to update KMS settings.
{: note}

### `ibmcloud schematics kms instance ls`
{: #schematics-kms-list}

Lists all the KMS instances of your {{site.data.keyword.cloud_notm}} account to find your {{site.data.keyword.keymanagementserviceshort}}or {{site.data.keyword.hscrypto}} by using your location  where keys are created and encrypted scheme such as `KYOK`, or `BYOK`. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics kms instances ls --location LOCATION_NAME --scheme ENCRYPTION_SCHEME [--output OUTPUT][--json]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--scheme` or `-s` | Required | Specify the encryption scheme. Supported values are `KYOK`, or `BYOK`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
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
ibmcloud schematics kms enable --location LOCATION_NAME --scheme ENCRYPTION_SCHEME --group RESOURCE_GROUP --primary_name PRIMARY_KMS_NAME --primary_crn PRIMARY_KEY_CRN --primary_endpoint PRIMARY_KMSPRIVATEENDPOINT [--secondary_name SECONDARY_KMS_NAME][--secondary_crn SECONDARY_KEY_CRN] [--secondary_endpoint SECONDARY_KMSPRIVATEENDPOINT] [--output OUTPUT][--json]
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
| `--json` or `-j` | Deprecated | Prints the output in the `JSON` format. |
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
ibmcloud schematics kms info --location LOCATION_NAME [--output OUTPUT][--json]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ | 
| `--location` or `-l` | Required | Set the {{site.data.keyword.bpshort}} location name. Supported values are `US`, or `EU`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--json` or `-j` | Deprecated | Prints the output in the `JSON` format. |
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

List the versions of all supported open source projects in {{site.data.keyword.bpshort}}, such as the {{site.data.keyword.terraform-provider_full_notm}}, Ansible, Helm, and Kubernetes that are used to run {{site.data.keyword.bpshort}} Actions on {{site.data.keyword.cloud_notm}} resources.
{: shortdesc}

Syntax

```sh
ibmcloud schematics version [--output OUTPUT] [--json JSON_FILE]
```
{: pre}


Command options 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--json` or `-j` | Deprecated | Returns the CLI output in JSON format. |
| `--output` or `-o` | Optional | Returns the CLI output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} version flags" caption-side="top"}

Example

```sh
ibmcloud schematics version --output json > "<filename.json>"
```
{: pre}


## Inventories commands
{: #inv-commands}

Review the command that you want to create, update, list, delete and to work with your {{site.data.keyword.bplong_notm}} inventory.
{: shortdesc}

### `ibmcloud schematics inventory create`
{: #schematics-create-inv}

Create a resource inventory in {{site.data.keyword.bplong_notm}} that you can reference in a {{site.data.keyword.bpshort}} action. A resource inventory includes all the target hosts where you want to run an Ansible playbook. You can create an inventory by using a payload file or the interactive mode.
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
|`--resource-query` | Optional |Enter the ID of a resource query that you created. A resource query helps to dynamically build your resource inventory by using the {{site.data.keyword.cloud_notm}} resources that you created with a {{site.data.keyword.bpshort}} Workspace. To create a resource query, see the `ibmcloud schematics resource-query create` [command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-rq). To enter multiple resource query IDs, use `--resource-query id1 --resource-query id2`.|
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

Delete the resource inventory definition by using the inventory ID from the {{site.data.keyword.bplong_notm}} service. Note you can delete the location and region, resource group from where your inventory was created. Also, make sure your IP addresses are in the [allowlist](/docs/schematics?topic=schematics-allowed-ipaddresses). 
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

Retrieve detailed information of an existing {{site.data.keyword.bplong_notm}} inventory by using the inventory ID.
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

Retrieve a list of all {{site.data.keyword.bpshort}} inventories in your account.
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

Update or replace an existing resource inventory. 
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


## Job commands
{: #schematics-job-commands}

Review the commands to create, update, list, and delete {{site.data.keyword.bplong_notm}} jobs.
{: shortdesc}

### `ibmcloud schematics job run`
{: #schematics-run-job}

Create a job in {{site.data.keyword.bplong_notm}} to run the Ansible playbook in your {{site.data.keyword.bpshort}} action. You can create a job by using a payload file or the command's interactive mode. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics job run --command-object COMMAND_OBJECT_TYPE --command-object-id COMMAND_OBJECT_ID --command-name COMMAND_NAME [--playbook-name PLAYBOOK_NAME] [--command-options COMMAND_OPTIONS] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--output OUTPUT] [--file FILE_NAME ] [--no-prompt] [--json]
```
{: pre}


Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--command-object` or `-c` | Required | The name of the {{site.data.keyword.bpshort}} automation resource. Currently, only `action` is supported. |
| `--command-object-id` or `-cid` | Required | The ID of the {{site.data.keyword.bpshort}} Actions where you want to run the job. |
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
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
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
ibmcloud schematics job update --id JOB_ID [--output OUTPUT] [--no-prompt] [--json]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` | Required | The ID of an existing job that you want to copy and run again. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command-line session. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} job update flags" caption-side="top"}

Example

```sh
ibmcloud schematics job update --id  us-east.JOB.yourjob_ID_1231 
```
{: pre}

### `ibmcloud schematics job get`
{: #schematics-get-job}

Retrieve the information of an existing {{site.data.keyword.bplong_notm}} job by using a job ID.
{: shortdesc}

Syntax

```sh
ibmcloud schematics job get --id JOB_ID [--profile PROFILE] [--output OUTPUT] [--json] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The ID of the job ID that you want to retrieve. |
| `--profile` or `-p` | Optional | The depth of information that you want to retrieve. Supported values are `detailed` and `summary`. The default value is `summary`.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to retrieve job details without an interactive command-line session. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} job get flags" caption-side="top"}

Example

```sh
ibmcloud schematics job get --id us-east.JOB.yourjob_ID_1231 --profile detailed
```
{: pre}

### `ibmcloud schematics job list`
{: #schematics-list-job}

Retrieve a list of all {{site.data.keyword.bpshort}} jobs that ran against a target hosts through {{site.data.keyword.bpshort}} action. The job displays a list of jobs with the status as `in_progress`, `success`, or `failed`.
{: shortdesc}

Syntax

```sh
ibmcloud schematics job list --resource-type RESOURCE_TYPE --id RESOURCE_ID [--limit LIMIT] [--offset OFFSET] [--profile PROFILE] [--output OUTPUT] [--all] [--no-prompt] [--json]
```
{: pre}

Command options

| Flag |  Required / Optional |Description |
| ----- | -------| -------- | 
| `--resource-type` or `-rt` | Required | The name of the {{site.data.keyword.bpshort}} resource. Only `action` is supported.|
| `--id` or `-i` | Required | The ID of the {{site.data.keyword.bpshort}} Actions for which you want to list jobs. |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer between 1 and 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the job in the list of jobs from where you want to start listing your jobs. For example, if you have three jobs in your account, the command returns these jobs as a list with three elements. To retrieve all jobs, you must enter position number 0. To retrieve job number 2 and 3 and leave out job number 1 in this list, you must enter position number 1. Position number 1 represents the second position in the list of jobs. Negative numbers are not supported and are ignored. |
| `--profile` or `-p` | Optional | The depth of information that is returned. Supported values are `ids` or `summary`. The default value is `summary`. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--all` or `-A` | Optional | Lists all the jobs including the {{site.data.keyword.bpshort}} internal jobs.|
| `--no-prompt` | Optional | Set this flag to create the job without an interactive command-line session. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} job list flags" caption-side="top"}

Example

```sh
ibmcloud schematics job list --resource-type action --id us-south.ACTION.interactive.aaa1a111 --profile ids --output json
```
{: pre}


### `ibmcloud schematics job logs`
{: #schematics-logs-job}

Retrieve the detailed logs of a job that ran for your {{site.data.keyword.bpshort}} action. For more information about viewing job queue logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup#job-logs).
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

## Resource management commands
{: #schematics-resource-commands}

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}}.

### `ibmcloud schematics apply`
{: #schematics-apply}

Scan and run the infrastructure code of your Terraform template that your workspace points to. When you apply a Terraform template, your resources are provisioned, modified, [stored](/docs/schematics?topic=schematics-general-faq#persist-file), or removed in {{site.data.keyword.cloud_notm}}.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}

Your workspace must be in an **Inactive**,  **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} apply action. For more information about workspace state, see [workspace state diagram](/docs/schematics?topic=schematics-workspace-setup#workspace-state-diagram).
{: note}

While your infrastructure code runs in {{site.data.keyword.bplong_notm}}, you cannot make any changes to your workspace.
{: important}

Syntax

```sh
ibmcloud schematics apply --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--var-file PATH_TO_VARIABLES_FILE] [--force] [--output OUTPUT][--json]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source control repository that you want to apply in {{site.data.keyword.cloud_notm}}. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the creation of a specific resource of your Terraform configuration file by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources that are defined in your configuration file is not created or updated. To target the creation of multiple resources, use the following syntax: `--target <resource1> --target <resource2>`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for creation.|
| `--var-file` or `--vf` | Optional |  The file path to the `terraform.tfvars` file that you created on your local machine. Use this file to store sensitive information, such as the {{site.data.keyword.cloud_notm}} API key or credentials to connect to {{site.data.keyword.cloud_notm}} classic infrastructure in the format `<key>=<value>`. All key value pairs that are defined in this file are automatically loaded into Terraform when you initialize the Terraform CLI. To specify multiple `tfvars` files, specify `--var-file TFVARS_FILE_PATH1 --var-file TFVARS_FILE_PATH2`.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} apply flags" caption-side="top"}

Example

```sh
ibmcloud schematics apply --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_instance.vm1 --var-file ./terraform.tfvars
```
{: pre}


### `ibmcloud schematics destroy`
{: #schematics-destroy}

Remove the {{site.data.keyword.cloud_notm}} resources that you provisioned with your {{site.data.keyword.bpshort}} Workspace, even if these resources are active.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

Use this command with caution. After you run the command, you cannot reverse the removal of your {{site.data.keyword.cloud_notm}} resources. If you use permanent storage, ensure that you create a backup for your data.
{: important} 	

Your workspace must be in an **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} destroy action. 
{: note}

Syntax

```sh
ibmcloud schematics destroy --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--force] [--output OUTPUT][--json]
```
{: pre}


Command options 

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that specifies the {{site.data.keyword.cloud_notm}} resources that you want to remove. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--target` or `-t` | Optional | Target the deletion of a specific resource by entering the Terraform resource address, such as `ibm_is_instance.vm1`. All other resources in your workspace remain unchanged. To target the deletion of multiple resources, use the following syntax: `--target <resource1> --target <resource2>`. If the targeted resource specifies the `count` attribute and no index is specified in the resource address, such as `ibm_is_instance.vm1[1]`, all instances that share the same resource name are targeted for deletion. Also, if the targeted resource can only be deleted if dependent resources are deleted, such as a VPC can only be deleted if the attached subnet is deleted, then all dependent resources are targeted for deletion as well.|
| `--force` or `-f` | Optional | Force the execution of this command without user prompts. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} destroy flags" caption-side="top"}

Example

```sh
ibmcloud schematics destroy --id myworkspace-a1aa1a1a-a11a-11 --json --target ibm_is_vpc.myvpc
```
{: pre}

### `ibmcloud schematics logs`
{: #schematics-logs}

Retrieve the Terraform log files for a {{site.data.keyword.bpshort}} Workspaces or a specific action ID. Use the log files to troubleshoot Terraform template issues or issues that occur during the resource provisioning, modification, or deletion process. 
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

Retrieve a list of Terraform output values. You define output values in your Terraform template to include information that you want to make accessible for other Terraform templates.
{: shortdesc}

Syntax

```sh
ibmcloud schematics output --id WORKSPACE_ID[--output OUTPUT][--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list Terraform output values. To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} output flags" caption-side="top"}

Example

```sh
ibmcloud schematics output --id myworkspace3_2-31cf7130-d0c4-4d
```
{: pre}

### `ibmcloud schematics plan`
{: #schematics-plan}

Scan the Terraform template in your source repository and compare this template against the {{site.data.keyword.cloud_notm}} resources that are already deployed. The command-line output shows the {{site.data.keyword.cloud_notm}} resources that must be added, modified, [persisted](/docs/schematics?topic=schematics-general-faq#persist-file), or removed to achieve the state that is described in your configuration file.
{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The host can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}

Your workspace must be in an **Inactive**, **Active**, **Failed**, or **Stopped** state to perform a {{site.data.keyword.bpshort}} plan action. 
{: note}

During the creation of the Terraform execution plan, you cannot make any changes to your workspace. 
{: note}

Syntax

```sh
ibmcloud schematics plan --id WORKSPACE_ID [--output OUTPUT] [--json]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that points to the Terraform template in your source repository that you want to scan. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--var-file` or `-vf`| Optional |  Path to variables definition file that are ending with `.tfvars` or `.json`. This flag can be used multiple times. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. Use `--output` JSON instead.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
{: caption="{{site.data.keyword.bpshort}} output flags" caption-side="top"}

Example

```sh
ibmcloud schematics plan --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}


## Resource query commands
{: #rq-commands}

Dynamically build resource inventories by using resource queries. Resource queries help you to retrieve your target hosts from existing {{site.data.keyword.bplong_notm}} Workspaces. For more information about resource queries and conditions, see [Creating resource inventories for {{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-inventories-setup).
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

Retrieve a list of all {{site.data.keyword.bpshort}} resource queries in your account.
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


## Stop commands
{: #stop-cmds}

After invoking a Workspaces job, like a `plan`, an `apply`, or a `destroy`, you may want to stop the running job, or to stop the provisioning of resources. When stopping, or canceling a long running job, it is advisable to first check the job logs to determine whether the job is actually stuck and needs stopping, or if it is performing long running operations that are taking time to complete. 

{{site.data.keyword.bpshort}} provides a number of options to allows users to `(gracefully) stop`, `force-stop`, or `terminate` the running job in order of immediacy and impact of the stop operation. 
{: shortdesc}

Review the commands to `(gracefully) stop`, `force-stop` or `terminate` jobs. 

### `ibmcloud schematics workspace job stop`
{: #schematics-stop-job}

Stops a running action or a job in {{site.data.keyword.bplong_notm}} Workspaces by sending and interrupt signal to Terraform to 
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

## Terraform commands
{: #tf-cmds}

You can run a bunch of Terraform commands and manipulate the {{site.data.keyword.cloud_notm}} resources by using {{site.data.keyword.bplong_notm}} API or CLI. The {{site.data.keyword.bpshort}} provides one generic API `commands` for each sub-command.
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
{: caption="Terraform commands summary" caption-side="bottom"}

### Commands
{: #cmds}

The `Commands` API executes one or group of Terraform commands by using the JSON file for your workspace command requirements. The access control such as `plan`, `apply`, `destroy`, or `refresh` are applicable for `Commands API`. Select your region where the workspace is created, and use the following syntax to run the commands API.

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

**Sample payload of Test.JSON file:**

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
|`command_name`| Optional | The name for the command block.|
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

## Terraform state file commands
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

Moves an instance or resources from the Terraform state. For example, if you move an instance from the state, the {{site.data.keyword.bpshort}} Workspaces instance continues running, but `Terraform plan` cannot  see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs) command.
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

Removes an instance or resources from the Terraform state. For example, if you remove an instance from the state, the {{site.data.keyword.bpshort}} Workspaces instance continues running, but `Terraform plan` cannot see that instance. You can use the workspace ID to retrieve the logs by using the [`ibmcloud schematics logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs) command.
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


## Workspaces commands
{: #schematics-workspace-commands}

Review the commands that you can use to set up and work with your {{site.data.keyword.bplong_notm}} Workspace. 
{: shortdesc}

### `ibmcloud schematics workspace action`
{: #schematics-workspace-action}

Retrieve all activities for a workspace, including the user ID of the person who initiated the action, the status, and a timestamp. 
{: shortdesc}

When you create a Terraform execution plan, or apply your Terraform template with {{site.data.keyword.bpshort}}, a {{site.data.keyword.bpshort}} Actions is automatically created and assigned an action ID. You can use the action ID to retrieve the logs of this action by using the [`ibmcloud schematics logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs) command.

Syntax

```sh
ibmcloud schematics workspace action --id WORKSPACE_ID [--act-id ACTION_ID] [--output OUTPUT][--json]
```
{: pre}


Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace for which you want to retrieve workspace activities. To find the ID of your workspace, run `ibmcloud schematics workspace list` command. |
| `--act-id` or `-a` | Optional | Enter the ID of a action that you want to retrieve. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} Workspaces action flags" caption-side="top"}

Example
```sh
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
{: caption="{{site.data.keyword.bpshort}} Workspaces delete flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace delete --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics workspace get`
{: #schematics-workspace-get}

Retrieve the details of an existing workspace, including the values of all input variables.	
{: shortdesc}	

Syntax

```sh
ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT][--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | The unique identifier of the workspace, for which you want to retrieve the details. To find the `Resource ID` of a workspace, run `ibmcloud schematics workspace list` command to view the of list service instances. From your resource group get an `Resource ID` for the `--id` flag. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Workspaces get flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace get --id myworkspace-a1aa1a1a-a11a-11 --json
```
{: pre}

### `ibmcloud schematics workspace import`
{: #schematics-workspace-import}

You can import an existing resource with an valid resource address into your Workspace state file. You need to ensure that the resource is only imported once and to a single Workspace. Otherwise, you may see unwanted behavior if the resource is defined in multiple workspaces. Review the [Terraform documentation](https://developer.hashicorp.com/terraform/cli/commands/import#usage){: external} for details on how to use the `import` command. 


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
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Workspaces import flags" caption-side="top"}

Use the option `-options -var-file=schematics.tfvars` to tell {{site.data.keyword.bpshort}} to import the resource with the saved workspace variables.  


Example

```sh
ibmcloud schematics workspace import --id WID --address ibm_iam_access_group.accgrp --resourceID AccessGroupId-xxxxxx-xxxx-xxx-xxx-xxxx -o -var-file=schematics.tfvars
```
{: pre}


### `ibmcloud schematics workspace list`	
{: #schematics-workspace-list}

List the workspaces in your {{site.data.keyword.cloud_notm}} account and optionally, show the details for your workspace.	

Syntax

```sh
ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--output] [--region] [--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--limit` or `-l` | Optional |  The maximum number of workspaces that you want to list. The number must be a positive integer starting from 1, maximum is 200. The default value is `-1`. |
| `--offset` or `-m` | Optional | The position of the workspace in the list of workspaces. For example, if you have three workspaces in your account, the command returns these workspaces as a list with three elements. To see a specific workspace in this list, you must enter the position number that the workspace has in the list. To list the first workspace in the list, enter `0`. To list the second workspace, enter `1` and so forth. Negative numbers are not supported and are ignored. The default value is `-1`.|
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--region` or `-r` | Optional | Specify the region, such as `eu`, `us`, `eu-gb`, `eu-de`, `us-south`, or `us-east`.|
{: caption="{{site.data.keyword.bpshort}} Workspaces list flags" caption-side="top"}

Example 

```sh
ibmcloud schematics workspace list --limit 10 --offset 20 --json
```
{: pre}

### `ibmcloud schematics workspace new`
{: #schematics-workspace-new}

Create a {{site.data.keyword.bpshort}} Workspaces that points to your Terraform template in GitHub or GitLab. If you want to provide your Terraform template by uploading a tape archive file (`.tar`), you can create the workspace without a connection to a GitHub repository and then use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command to provide the template.

{{site.data.keyword.bpshort}} does not support passing `.tar` file to create a workspace.
{: important}

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The location can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again.
{: shortdesc}	

To create a workspace, you can specify your workspace settings in a JSON file. Make sure that the JSON file follows the structure as outlined in this command. Also ensure the `location` and the `url` endpoint are pointing to the same region when you create or update workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

Syntax

```sh
ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH  [--agent-id AGENT_ID]  [--github-token GITHUB_TOKEN] [--output OUTPUT] [--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--file` or `-f` | Optional | The relative path to a JSON file on your local machine that is used to configure your workspace. For more information about the sample JSON file with the details, see [JSON file create template](/docs/schematics?topic=schematics-schematics-cli-reference#json-file-create-template).|
| `--state` | Optional | The relative path to an existing Terraform state file on your local machine. To create the Terraform state file: **1.** Show the content of an existing Terraform state file by using the [`ibmcloud schematics state pull`](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command. **2.** Copy the content of the state file from your command-line output in to a file on your local machine that is named `terraform.tfstate`. **3.** Use the relative path to the file in the `--state` command parameter. **Note** The {{site.data.keyword.bpshort}} Workspace supports the `terraform.tfstate` file size of less than 2 MB.|
| `--github-token` or `-g` | Optional |  Enter the functional personal access tokens for HTTPS Git operations. For example, `--github-token ${FUNCTIONAL_GIT_KEY}`.|
| `--agent-id` or `--aid` | Optional | **New** ID of the Agent to bind your new workspace. Agents help you to run your workspace jobs on your infrastructure. For more information, see [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro).|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Workspaces create flags" caption-side="top"}

The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as **Alias 'terraform' will be deprecated. Use 'schematics' or 'sch' in your commands.**
{: note}

#### Create file template in JSON format
{: #json-file-create-template}

 {{site.data.keyword.bpshort}} supports to download the Terraform modules template from the private repository. For more information, see [Supporting to download modules from private remote host](/docs/schematics?topic=schematics-download-modules-pvt-git). 
 
 You can create the JSON file as shared in the `example.json` file for workspace creation and pass the file path along with the file name in `--file` flag. The description of all the parameters of `example.json` as described in the table. 

You need to replace the `<...>` placeholders with the actual values. For example, `"<workspace_name>"` as `"testworkspace"`.
{: note}


**example.json:**

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
        "VAR1":"<val1>"
        },
        {
        "VAR2":"<val2>"
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

**Example JSON for uploading in a `.tar` file later.**

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
        "VAR1":"<val1>"
        },
        {
        "VAR2":"<val2>"
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
| `workspace_name` | Optional | Enter a name for your workspace. The maximum length of character limit is set to less than 1 MB. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace).|
| `terraform_version` | Optional | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v1.1` to use Terraform version 1.1,`terraform_v1.0` to use Terraform version 1.0, and similarly, `terraform_v0.15`, `terraform_v0.14`, `terraform_v0.13`, `terraform_v0.12`. For example, when you specify `terraform_v1.1` means users can have template that are of Terraform `v1.1.0`, `v1.1.1`, or `v1.1.2`, so on. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template. {{site.data.keyword.bpshort}} supports `Terraform_v1.x` and also plans to make releases available after `30 to 45 days` of HashiCorp Configuration Language (HCL) release. |
| `location` | Optional | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} Actions run and where your workspace data is stored. If you do not enter a location, {{site.data.keyword.bpshort}} determines the location based on the {{site.data.keyword.cloud_notm}} region that you targeted. To view the region that you targeted, run `ibmcloud target --output json` and look at the `region` field. To target a different region, run `ibmcloud target -r <region>`. If you enter a location, make sure that the location matches the {{site.data.keyword.cloud_notm}} region that you targeted. |
| `description` | Optional | Enter a description for your workspace. |
| `template_repo.url` | Optional | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. |
| `template_repo.branch` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. Now, in `template_repo`, you can also update URL with more parameters as shown in the block. |
| `template_repo.datafolder` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. |
| `template_repo.release` | Optional | Enter the GitHub or GitLab release that points to your Terraform configuration files. |
| `github_source_repo_url` | Optional | Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. If you choose to create your workspace without a GitHub repository, your workspace is created with a **draft** state. To connect your workspace to a GitHub repository later, you must use the `ibmcloud schematics workspace update` command. If you plan to provide your Terraform template by uploading a tape archive file (`.tar`), leave the URL empty, and use the [ibmcloud schematics workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command after you created the workspace. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning. |
| `env_values` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform action. This field must be provided as a list of key-value pairs. Each entry will be a map with one entry where `key = variable name` and `value = value`. You can define environment variables for {{site.data.keyword.cloud_notm}} catalog offerings that are provisioned by using a bash script files. |
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

Perform an {{site.data.keyword.cloud_notm}} refresh action against your workspace. A refresh action validates the {{site.data.keyword.cloud_notm}} resources in your account against the state that is stored in the Terraform state file of your workspace. If differences are found, the Terraform state file is updated accordingly. 
{: shortdesc}

Syntax

```sh
ibmcloud schematics refresh --id WORKSPACE_ID [--output OUTPUT][--json]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace that you want to refresh and run an action against. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} refresh flags" caption-side="top"}


Example
```sh
ibmcloud schematics refresh --id myworkspace-a1aa1a1a-a11a-11 
```
{: pre}

### `ibmcloud schematics state list`
{: #state-list}

List the `Name`, `Type`, `URL`, and `Taint Status` of the {{site.data.keyword.cloud_notm}} resources that are documented in your Terraform state file (`terraform.tfstate`). 
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
| `--id` or `-i` | Required |  The unique identifier of the workspace for which you want to list the {{site.data.keyword.cloud_notm}} resources that are documented in the Terraform state file. To find the ID of a workspace, run `ibmcloud schematics workspace list` command.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} state list flags" caption-side="top"}

Example

```sh
ibmcloud schematics state list --id myworkspace-a1aa1a1a-a11a-11  
```
{: pre}


### `ibmcloud schematics workspace taint`
{: #schematics-workspace-taint}

Manually marks an instance or resources as tainted, by forcing the resources to be re-created on the next apply. Taint modifies the state file, but not the infrastructure in your workspace. When you perform next plan the changes will display as re-created, and in the next apply the change is implemented.
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
{: caption="{{site.data.keyword.bpshort}} Workspaces taint flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace taint --id myworkspace-lalalalalalala-11 --address null_resource.sleep  
```
{: pre}


### `ibmcloud schematics workspace untaint`
{: #schematics-workspace-untaint}

Manually marks an instance or resources as untainted, by forcing the resources to be restored on the next apply. When you perform next plan the changes will show as restored and in the next apply the change is implemented.
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
| `--address` or `-adr` | Optional | Enter the address of the resource to mark as untaint.|
{: caption="{{site.data.keyword.bpshort}} Workspaces untaint flags" caption-side="top"}

Example

```sh
ibmcloud schematics workspace untaint --id myworkspace-asdff1a1a-42145-11 --address null_resource.sleep  
```
{: pre}

### `ibmcloud schematics workspace update`
{: #schematics-workspace-update}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify {{site.data.keyword.cloud_notm}}, see the [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.

{{site.data.keyword.bplong_notm}} supports 50 API requests per minute, per host, and per customer. The region can be `us-east`, `us-south`, `eu-gb`, or `eu-de` region. You need to wait before calling the command again. Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}	

If you provided your Terraform template by uploading a tape archive file (`.tar`) and you want to update your template, you must use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command.
{: note}

Syntax

```sh
ibmcloud schematics workspace update --id WORKSPACE_ID [--file FILE_NAME] [--github-token GITHUB_TOKEN] [--pull-latest] [--output OUTPUT] [--json]
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
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Workspaces update flags" caption-side="top"}

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
        "VAR1":"<val1>"
        },
        {
        "VAR2":"<val2>"
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
| `name` | Optional | Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). If you update the name of the workspace, the ID of the workspace does not change. |
| `type` | Optional | The Terraform version that you want to use to run your Terraform code. Enter `Terraform_v1.1` to use Terraform version 1.1, `Terraform_v1.0` to use Terraform version 1.0, and similarly, `terraform_v0.15`, `terraform_v0.14`, `terraform_v0.13`, `terraform_v0.12`. For example, when you specify `terraform_v1.1` means users can have template that are of Terraform `v1.1.0`, `v1.1.1`, or `v1.1.2`, so on. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template. |
| `description` | Optional | Enter tags that you want to associate with your workspace. Tags can help you find your workspace faster. |
| `resource_group` | Optional | Enter the resource group where you want to provision your workspace. |
| `workspace_status` | Optional | Freeze or unfreeze a workspace. If a workspace is frozen, changes to the workspace are disabled. |
| `template_repo.url` | Optional | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. |
| `template_repo.branch` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. Now, in template repository, you can also update URL with more parameters as shown in the block. |
| `template_repo.datafolder` | Optional | Enter the GitHub or GitLab branch where your Terraform configuration files are stored. |
| `template_repo.release` | Optional | Enter the GitHub or GitLab release that points to your Terraform configuration files. |
| `github_source_repo_url` | Optional | Enter the link to your GitHub repository. The link can point to the `master` branch, a different branch, or a subdirectory. |
| `template_data.folder` | Optional | Enter the name for the input variable that you declared in your Terraform configuration files. |
| `template_data.type` | Optional | Enter the name for the input variable type that you declared in your Terraform configuration files. |
| `template_data[0].env_values[i].va11` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform job. This field must be provided as a list of key-value pairs, for example, `TF_LOG=debug`. Each entry will be a map with one entry where **key is the environment variable name and value is value**. |
| `template_data[0].env_values[i].val2` | Optional | A list of environment variables that you want to apply during the execution of a bash script or Terraform job. This field must be provided as a list of key-value pairs, for example, `TF_LOG=debug`. Each entry will be a map with one entry where **key is the environment variable name and value is value**. |
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
ibmcloud schematics workspace update --id myworkspace-a1aa1a1a-a11a-11 --file myfile.json --json
```
{: pre}

### `ibmcloud schematics workspace upload`
{: #schematics-workspace-upload}

Provide your Terraform template by uploading a tape archive file (`.tar`) to your {{site.data.keyword.bpshort}} Workspace.
{: shortdesc}

Before you begin, make sure that you [created your workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) without a link to a GitHub or GitLab repository.
{: important}

Syntax

```sh
ibmcloud schematics workspace upload  --id WORKSPACE_ID --file FILE_NAME --template TEMPLATE_ID [--output OUTPUT][--json]
```
{: pre}



Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required |  The unique identifier of the workspace where you want to upload your tape archive file (`.tar`). To find the ID of your workspace, run `ibmcloud schematics workspace list` command.|
| `--file` or `-f` | Required | Enter the full file path on your local machine where your `.tar` file is stored.|
| `--template` or `-tid` | Required |  The unique identifier of the Terraform template for which you want to show the content of the Terraform state file. To find the ID of the template, run `ibmcloud schematics workspace get --id <workspace_ID>` and find the template ID in the **Template Variables for:** field of your command-line output.|
| `--output` or `-o` | Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--json` or `-j` | Deprecated | Prints the output in the JSON format. |
{: caption="{{site.data.keyword.bpshort}} Workspaces upload flags" caption-side="top"}

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


## Agents beta-0 commands
{: #agents-beta0-cmd}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

### `ibmcloud schematics agents bind-workspaces`
{: #schematics-agents-bind-wks}

Create a policy for binding workspace(s) to the agent on {{site.data.keyword.bplong_notm}}.

Syntax

```sh
ibmcloud schematics agents bind-workspaces --agent-id AGENT_ID --workspace-id WORKSPACE_ID [--workspace-id WORKSPACE_ID] [--file FILE] [--output OUTPUT] [--no-prompt]
```
{; pre}

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--agent-id` or `--aid` | Required | ID of the Agent.|
| `--workspace-id` | Required | Workspace ID to bind the Agent. This flag can be used multiple times to bind multiple workspace IDs.|
| `--file` or `-f`| Optional | Path to the JSON file containing the definition of the policy. |
| `--output` or  `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent bind flags" caption-side="bottom"}

Example
```sh
ibmcloud schematics agents bind-workspaces --agent-id AGENT_ID --workspace-id WORKSPACE_ID
```
{: pre}


### `ibmcloud schematics agents get`
{: #schematics-agents-get}

Retrieves the {{site.data.keyword.bpshort}} Agent.

Syntax

```sh
ibmcloud schematics agents get --id AGENT_ID [--profile PROFILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i` | Required | Geographic region supported by {{site.data.keyword.bpshort}} service such as, `us-south`, `us-east`, `eu-de`, `eu-gb`.|
| `--profile` or `p` | Optional |  Set this flag to filter Agents. Valid values are `all`, `saved`, and `new`. If not set defaults to `saved`. |
| `--output` or `-o` |  Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported. |
| `--no-prompt` | Optional |  Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent get flags" caption-side="top"}

Example

```sh
ibmcloud schematics agents get 
```
{: pre}

### `ibmcloud schematics agents list`
{: #schematics-agents-list}

Lists all the Agents. Defaults to show the registered agents.

Syntax

```sh
ibmcloud schematics agents list [--region REGION] [--filter NEW] [--limit LIMIT] [--offset OFFSET] [--output OUTPUT_FORMAT]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--region` or `-r` | Optional | Geographic region supported by {{site.data.keyword.bpshort}} service such as, `us-south`, `us-east`, `eu-de`, `eu-gb`.|
| `--filter`  | Optional | Set this flag to filter Agents. Valid values are `all`, `saved`, and `new`. If not set defaults to `saved`. |
| `--limit` or `-l` | Optional |  Maximum number of workspaces to list. Ignored if a negative number is set. The number must be a positive integer between 1 and 200. The default value is `-1`.|
| `--offset`or `-m`| Optional | Offset in list. Ignored if a negative number is set. The default value is `-1`.|
| `--output` or `-o` |  Optional | Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} Agent list flags" caption-side="top"}

Example

```sh
ibmcloud schematics agents list 
```
{: pre}


### `ibmcloud schematics agents register`
{: #schematics-agent-register}

Register the Agent with {{site.data.keyword.bpshort}} to run your workspace jobs on your Agent infrastructure. For more information about Agent infrastructure, see [Installing {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-agents-intro).

Syntax

```sh
ibmcloud schematics agents register --name AGENT_NAME --profile-id PROFILE_ID --agent-location AGENT_LOCATION --location LOCATION [--description DESCRIPTION] [--resource-group RESOURCE_GROUP] [--tags TAGS] [--file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

Command options

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--name` or `-n` | Required | Unique name of the Agent. |
| `--profile-id` | Required | IAM [Trusted Profile ID](/docs/schematics?topic=schematics-agent-trusted-profile), used by the Agent instance.|
| `--agent-location` | Required | Specify the region of the cluster where Agent service is deployed. For example, `us-south`. |
| `--location` or `-l` | Required | Geographic locations supported by {{site.data.keyword.bpshort}} service such as, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for processing.|
| `--description` or `-d` | Optional | Short description of the Agent.|
| `--resource-group` or `-r` | Optional |  Resource group for the Agent.|
| `--tags` or `-t`| Optional | Agent tags. This flag can be used multiple times and search the Agent related resources faster.|
| `--file` or `-f`| Optional | Path to the JSON file containing the definition of the Agent.|
| `--output` or `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent register flags" caption-side="top"}

Example

```sh
ibmcloud schematics agents register 
```
{: pre}


### `ibmcloud schematics agents unregister`
{: #schematics-agents-unregister}

Unregister the {{site.data.keyword.bpshort}} Agent.

Syntax

```sh
ibmcloud schematics agents unregister --id AGENT_ID [--no-prompt]
```
{: pre}

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--id` or `-i`| Required | ID of the Agent.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent unregister flags" caption-side="bottom"}

Example
```sh
ibmcloud schematics agents unregister --id AGENT_ID
```
{: pre}


### `ibmcloud schematics agents update`
{: #schematics-agents-update}

Updates the {{site.data.keyword.bpshort}} Agent.

Syntax

```sh
ibmcloud schematics agents update --id AGENT_ID [--description DESCRIPTION] [--user-state USER_STATE] [--tags TAGS] [--profile-id PROFILE_ID] [--file FILE] [--output OUTPUT] [--no-prompt]
```
{: pre}

| Flag | Required / Optional |Description |
| ----- | -------- | ------ |
| `--id` or `-i`| Required | ID of the agent.|
| `--tags` or `-t`| Optional | Agent tags. This flag can be used multiple times to search the specific Agent related resources.|
| `--description` or `-d` | Optional | Short description of the agent.|
| `--user-state` | Optional | User defined status of the Agent. It can be either `enable`, or `disable`.|
| `--profile-id` | Optional | IAM [Trusted Profile ID](/docs/schematics?topic=schematics-agent-trusted-profile), used by the Agent instance.|
| `--file` or `-f` | Optional| Path to the JSON file containing the definition of the Agent.|
| `--output` or  `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
| `--no-prompt` | Optional | Set this flag to update an inventory without an interactive command-line session.|
{: caption="{{site.data.keyword.bpshort}} Agent update flags" caption-side="top"}

Example

```sh
ibmcloud schematics agents update --id AGENT_ID
```
{: pre}

### `ibmcloud schematics workspace new with Agent`
{: #schematics-agent-new}

Create the {{site.data.keyword.bpshort}} Workspace to work with your Terraform configuration and bind your Agent to the new workspace. 

Syntax

```sh
ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH  [--agent-id AGENT_ID]  [--github-token GITHUB_TOKEN] [--output OUTPUT] [--json]
```
{: pre}

For more information about the flags see [workspace new](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-workspace-new) command.
{: note}

Example

```sh
ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH 
```
{: pre}

### `ibmcloud schematics workspace get with Agent`
{: #schematics-agent-get}

Retrieve the details of an existing workspace, including the values of all input variables and {{site.data.keyword.bpshort}} Agent.	
{: shortdesc}

Syntax

```sh
ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT][--json]
```
{: pre}

For more information about the flags see [workspace get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-workspace-get) command.
{: note}
