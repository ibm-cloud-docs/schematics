---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-22"

keywords: schematics command-line reference, schematics commands, schematics command-line, schematics reference, command-line

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Creating blueprints via the CLI using a config file
{: #create-blueprint-file}

Alternative to creating a blueprint via the CLI using parameters and flags, a JSON config file can be provided to pass blueprint create API parameters directly. This is a direct interface via the API and the JSON file format is not guaranteed to remain compatible on API changes. Use the CLI With parameters and flags if long term compatibility is required.    

Create a blueprint config using the `ibmcloud schematics blueprint create -file` command. The blueprint config is created from a user provided configuration supplied as a JSON file that specifies the source of the blueprint template in a Git repository, the source of any input files and optional dynamic or override inputs. The file operation passes the configuration directly to the Schematics API and uses the API parameter definitions. The definition of the config file uses the Schematics API payload, which differs from to the `schematics blueprint create` CLI syntax.  
{: shortdesc}

Refer to the [Schematics IBM API Docs](https://cloud.ibm.com/apidocs/schematics/schematics#create-blueprint) for details on how to create the API payload. 

**Syntax to create using command with the config file option:**

```sh
ibmcloud schematics blueprint create --name BLUEPRINT_NAME --file CONFIG_FILE_PATH [--output OUTPUT]
```
{: pre}


Command options

| Flag | Required / Optional | Description |
| ----- | -------- | ------- |
| `--name`or `-n`| Required | Name of the blueprint. |
| `--resource-group` or `-r` | Required | The management resource group for the blueprint.|
| `description` or `--desc` | Optional | The description of the blueprint. |
| `--file` or `-f` | Optional | Local path and file name for a config JSON file to create the blueprint. Exclusive with other options. This approach supporting passing complex input variables. |
| `--output` or  `-o` | Optional |Returns the command-line output in JSON format. Currently only `JSON` file format is supported.|
{: caption="{{site.data.keyword.bpshort}} blueprint create flags" caption-side="bottom"}

Example

```sh
ibmcloud schematics blueprint create -file config.json
```
{: pre}

#### Using a config file
{: #bp-create-config}

Alternative to use command line parameters, you can provide a config JSON file to specify the parameters for the blueprint create. Pass the file name to the command by using the `--file` command option. This approach supports passing complex input variables at create time. 
{: shortdesc}

You need to replace the `<...>` placeholders with the actual values. To pass double quotes as required by Terraform for variables, double quotes must be correctly escaped in the JSON as `\\\"` . 
{: note}

Refer to the [Schematics IBM API Docs](https://cloud.ibm.com/apidocs/schematics/schematics#create-blueprint) for details on how to create the API payload. 

- The location of the blueprint template is defined using the `source` block.
- The location of the blueprint input files is defined by the `config` block. 

Example config JSON files can be found in the folder 'test' for many of the blueprint examples in the [Cloud-Schematics Github repos](https://github.com/Cloud-Schematics){: external}


**Syntax when using a config JSON file:**
```json
{
  "name": "<PROVIDE BLUEPRINT NAME>",
  "schema_version": "<blueprint template VERSION>",
  "source": {
    "source_type": "<BLUEPRINT TEMPLATE REPOSITORY, FOR EXAMPLE, `git_hub`>",
    "git": {
      "git_repo_url": "<BLUEPRINT INPUT FILE ABSOLUTE PATH>",
      "git_repo_folder": "<subfolder>/<blueprint template FILE>.<extension>",
      "git_branch": "main"
    }
  },
  "inputs" :[
    {
      "name": "region",
      "value": "us-east"
    },
    {
      "name" :  "api_key",
      "value": "<PROVIDE YOUR api_key VALUE>"
    },
    {
      "name" :  "complex-list(any)",
      "value": "[\\\"36\\\", \\\"mqm-grand\\\", \\\"madison-square-garden\\\"]"
    }
  ],
  "config": [
    {
      "source": {
        "source_type": "<BLUEPRINT TEMPLATE REPOSITORY, FOR EXAMPLE, `git_hub`>.<extension>",
        "git": {
          "git_repo_url": "<BLUEPRINT TEMPLATE ABSOLUTE PATH>",
          "git_repo_folder": "<sub-folder>/<BLUEPRINT INPUT FILE>",
          "git_branch": "master"
        }
      }
    }
  ],
  "description": "<ENTER THE DESCRIPTION>",
  "resource_group": "<ENTER YOUR RESOURCE GROUP THAT HAS BLUEPRINT PERMISSIONS>"
}

```
{: codeblock}

Example

```json
{
    "name": "Blueprint Basic",
    "schema_version": "1.0.0",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "main"
        }
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "main"
                }
            }
        }
    ],
    "inputs": [
        {
            "name": "provision_rg",
            "value": "true"
        },
        {
            "name": "resource_group_name",
            "value": "myrg4"
        }
    ],
    "description": "Deploys a simple two module blueprint",
    "resource_group": "Default"
}
```
{: codeblock}


Example:

```sh
ibmcloud schematics blueprint create --file createtest.json --github-token <ENTER YOUR GIT TOKEN>
```
{: pre}

Output:

```text
Created blueprint ID: Blueprint-Basic.eaB.b5f7

Modules to be created
SNO   Module Type   Name   
1     Workspace     basic-resource-group   
2     Workspace     basic-cos-storage   
      
Blueprint job us-east.JOB.Blueprint-Basic.a5013d5b started at 2022-12-21 12:54:50

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint-Basic.a5013d5b completed at 2022-12-21 12:57:40

Module Type   Name                   Status           Job ID   
Blueprint     Blueprint Basic        CREATE_SUCCESS   us-east.JOB.Blueprint-Basic.a5013d5b   
Workspace     basic-resource-group   INITIALISED         
Workspace     basic-cos-storage      INITIALISED         
              
Blueprint ID Blueprint-Basic.eaB.b5f7 create_success at 2022-12-21 12:57:41
OK
```
{: screen}

