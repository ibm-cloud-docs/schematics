---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-29"

keywords: parallelism, schematics parallelism, environment variables, command-line configuration, env vars

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Supporting Terraform environment variables in {{site.data.keyword.bplong_notm}}
{: #set-parallelism}

Terraform uses various environment variables to customize different aspects of its behavior. These environment variables are used to increase the output verbosity for debugging or rarely used in runtime. 

Parallelism is one of the environment variable with a number flag range between `1 and 256`, the default value is `10`. Parallelism is used to fix infrastructure providers that error on concurrent operations or use during non-standard rate limiting, when you execute `terraform plan` and `terraform apply` at runtime.

Now {{site.data.keyword.bplong}} supports setting a custom value for parallelism.
{: shortdesc}

## Usage
{: #parelleism-usage}

You can pass Terraform command-line arguments `TF_CLI_ARGS` as an environment variables with the specific name such as `TF_CLI_ARGS_plan`, and `TF_CLI_ARGS_apply` in the {{site.data.keyword.bpshort}} Workspaces for a customized experience. Terraform reads these environment variables and apply parallelism at runtime. For more information, about Terraform command-line arguments, see [`TF_CLI_ARGS and TF_CLI_ARGS_name`](https://www.terraform.io/cli/config/environment-variables#tf_cli_args-and-tf_cli_args_name){: external}. 

### Example using parallelism
{: #parallelism-example}

The code block is the sample payload for creating workspace with parallelism passed as an environment variable.

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

A sample `env_values` block in the payload to update environment variable by using variable update API. For more information, see [Update workspace input variables](/apidocs/schematics/schematics#replace-workspace-inputs) API.

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

For content catalog Terraform workspaces, you can pass a special variable `TF_PARALLELISM` in `tf_values` to set parallelism.

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

## List of environment variables
{: #list-special-env-vars}

{{site.data.keyword.bplong_notm}} supports following environment variables for debugging purpose. For more information, about special environment variables, see [Environment variables](https://www.terraform.io/cli/config/environment-variables). 

| Variable | Description | Usage |
| ----  | ----- | ----- |
| `TF_LOG` | The detailed logs that appear on standard error. Support values are **TRACE, DEBUG, INFO, WARN, or ERROR** | `"TF_LOG": "TRACE"` |
| `TF_INPUT` | Command to disable prompts for entering the input value. The default value is **false** or **0**.| `"TF_INPUT": "0"` |
| `TF_CLI_ARGS` and `TF_CLI_ARGS_name` | The `TF_CLI_ARGS` specify additional arguments to the command-line. This allows easier automation in cloud infrastructure environments. Also to modify the default behavior of the Terraform on your own system. `TF_CLI_ARGS` and `TF_CLI_ARGS_name` is only for non content catalog.| `"TF_CLI_ARGS_apply": "-parallelism=20"`|
| `TF_REGISTRY_DISCOVERY_RETRY` | Set the maximum number of request retries the remote registry client can attempt for client connection errors.| `"TF_REGISTRY_DISCOVERY_RETRY": "10"`|
| `TF_REGISTRY_CLIENT_TIMEOUT` | Set to increase the extraneous circumstances. The default value for the remote registry is `10 seconds`.| `"TF_REGISTRY_CLIENT_TIMEOUT": "15"`|
| `TF_IGNORE` | Output the debug messages to display ignored files and folders. This is useful when you debug large repositories with `.terraformignore` files. The default value is **trace**.| `"TF_IGNORE": "trace"`|
| `TF_PARALLELISM` | Read parallelism environment variable in runtime action and reset the parallelism value on all the {{site.data.keyword.bpshort}} Actions only for content catalog. `TF_PARALLELISM` is only for content catalog. |`"TF_PARALLELISM": "20"`|
{: caption="Supported environment variables" caption-side="top"}



