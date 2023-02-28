---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: parallelism, schematics parallelism, environment variables, command-line configuration, env vars

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Using environment variables with workspaces
{: #set-parallelism}

Terraform uses various environment variables to customize different aspects of its behavior. For example these environment variables are used to increase the output verbosity for debugging or to set rarely used in runtime parameters. 

Parallelism is one of the environment variable with a number flag range between `1 and 256`, the default value is `10`. Parallelism is used to configure infrastructure providers that error on concurrent operations or use during non-standard rate limiting, when you execute `terraform plan` and `terraform apply` at runtime.
{: shortdesc}

## Usage
{: #parelleism-usage}

### Passing TF_CLI_ARGS
{: #passing-cli-args}

You can pass Terraform command-line arguments `TF_CLI_ARGS` as environment variables, like `TF_CLI_ARGS_plan`, and `TF_CLI_ARGS_apply` in the {{site.data.keyword.bpshort}} Workspaces to customize operations. 

Terraform reads these environment variables and applies them runtime. For more information about Terraform command-line arguments, see [`TF_CLI_ARGS and TF_CLI_ARGS_name`](https://developer.hashicorp.com/terraform/cli/config/environment-variables#tf_cli_args-and-tf_cli_args_name){: external}. 

### Example setting parallelism or TF_LOGS 
{: #parallelism-example}

The examples shown here can be used to set any environment variable to be passed to Terraform. 

#### Setting parallelism at create time
{: #parallelism-example-create}

The code block is the sample payload for creating workspace with the Schematics [Create API](https://cloud.ibm.com/apidocs/schematics/schematics#create-workspace){: external} with parallelism passed as an environment variable.

```json
{
    "name": "bb",
    "type": [
        "terraform_v1.0"
    ],
    "template_repo": {
        "url": "url"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "terraform_v1.0",
        "variablestore": [
        {
          "value": "<val>",
          "name": "ibmcloud_api_key",
          "type": "string",
          "secure": true
        }
        ],
        "env_values": [
        {
          "TF_LOG": "debug"
        },
        {
          "TF_CLI_ARGS_apply": "-parallelism=20"
        },
        {
          "TF_CLI_ARGS_plan": "-parallelism=20"
        }
        ]
    }
    ]
}
```
{: codeblock}

#### Setting parallelism after create
{: #parallelism-example-update}

A sample `env_values` block in the payload to update environment variables using the Schematics Workspace [Update API](https://cloud.ibm.com/apidocs/schematics/schematics#replace-workspace){: external}. For a more detailed example, see [Update workspace input variables](/apidocs/schematics/schematics#replace-workspace-inputs){: external} API.

```json
"env_values": [
    {
        "name": "TF_CLI_ARGS_plan",
        "value": "-parallelism=20",
        "secure": false,
        "hidden": false
    },
    {
        "name": "TF_CLI_ARGS_apply",
        "value": "-parallelism=20",
        "secure": false,
        "hidden": false
    }
    ]
```
{: codeblock}

#### Setting parallelism for Catalog users
{: #parallelism-example-catalog}

For IBM Catalog generated Terraform workspaces, you can pass a special variable `TF_PARALLELISM` in `tf_values` to set parallelism.

A sample `tf_values` block in the create payload.

```json
    "tf_values":{
    "sample_var": "hello",
    "sleepy_time": "5",
    "TF_VERSION" : "1.0",
    "TF_PARALLELISM": "20"
}
```
{: codeblock}

## List of Terraform environment variables
{: #list-special-env-vars}

{{site.data.keyword.bplong_notm}} supports following environment variables for debugging purpose. For more information about special environment variables, see [Environment variables](https://developer.hashicorp.com/terraform/cli/config/environment-variables). 

| Variable | Description | Usage |
| ----  | ----- | ----- |
| `TF_LOG` | The detailed logs that appear on standard error. Support values are **TRACE, DEBUG, INFO, WARN, or ERROR** | `"TF_LOG": "TRACE"` |
| `TF_LOG_PROVIDER` | For debugging Terraform provider issues, see [Managing Log Output](https://developer.hashicorp.com/terraform/plugin/log/managing){: external}. | `"TF_LOG_PROVIDER": "TRACE"` |
| `TF_INPUT` | Command to disable prompts for entering the input value. The default value is **false** or **0**.| `"TF_INPUT": "0"` |
| `TF_CLI_ARGS` and `TF_CLI_ARGS_name` | The `TF_CLI_ARGS` specify additional arguments to the command-line. This allows easier automation in cloud infrastructure environments. Also to modify the default behavior of the Terraform on your own system. `TF_CLI_ARGS` and `TF_CLI_ARGS_name` is only for non content catalog.| `"TF_CLI_ARGS_apply": "-parallelism=20"`|
| `TF_REGISTRY_DISCOVERY_RETRY` | Set the maximum number of request retries the remote registry client can attempt for client connection errors.| `"TF_REGISTRY_DISCOVERY_RETRY": "10"`|
| `TF_REGISTRY_CLIENT_TIMEOUT` | Set to increase the extraneous circumstances. The default value for the remote registry is `10 seconds`.| `"TF_REGISTRY_CLIENT_TIMEOUT": "15"`|
| `TF_IGNORE` | Output the debug messages to display ignored files and folders. This is useful when you debug large repositories with `.terraformignore` files. The default value is **trace**.| `"TF_IGNORE": "trace"`|
| `TF_PARALLELISM` | Read parallelism environment variable in runtime action and reset the parallelism value on all the {{site.data.keyword.bpshort}} Actions only for content catalog. `TF_PARALLELISM` is only for content catalog. |`"TF_PARALLELISM": "20"`|
{: caption="Supported environment variables" caption-side="top"}

Additional environment variables are supported for debugging Terraform provider issues, see [Managing Log Output](https://developer.hashicorp.com/terraform/plugin/log/managing){: external}. 
