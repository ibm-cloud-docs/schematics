---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-27"

keywords: blueprint create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Create a blueprint configuration 
{: #create-blueprint-config}

Deploying a blueprint environment and cloud resources using a blueprint template is a two-step process. The first step is creating a blueprint configuration in {{site.data.keyword.bpshort}}, the second step deploys this configuration with a `blueprint apply` operation. See [Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints) for an overview of deploying and change manage in blueprint environments.

The creation of a blueprint config takes as its input the blueprint template YAML and input YAML file that were created during the [defining blueprints](/docs/schematics?topic=schematics-define-blueprints) lifecycle stage. 
{: shortdesc} 

The first step in deploying cloud resources is [creating a blueprint configuration](/docs/schematics?topic=schematics-create-blueprint-config) in {{site.data.keyword.bpshort}}. This saves the blueprint configuration for future operations. The blueprint config specifies the Git source and release of the blueprint template, input files, and any input values that are used to create cloud resources. 

Blueprints creates a blueprint module resource in {{site.data.keyword.bpshort}} for each module definition in the template. Blueprint module resources are initialized with the Terraform module source from the Git repository specified in the module definition, and module inputs.

The second [blueprint apply](/docs/schematics?topic=schematics-apply-blueprint&interface=cli) step executes the automation modules in dependency order and runs the module Terraform code to deploy cloud resources. 


## Creating a blueprint configuration through the CLI 
{: #create-blueprint-cli}
{: cli}

Create your blueprint config with the CLI. The Create command requires a name and the Git URL of a blueprint template and other arguments. For a complete listing of options, see [blueprint create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

To work with {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.5`.
{: important}

Before your begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.5`.
- Select the {{site.data.keyword.cloud_notm}} region that you wish to use to manage your {{site.data.keyword.bpshort}}. Set the region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

The command example shown here creates a blueprint configuration in {{site.data.keyword.bpshort}}, with the template file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. With this basic two module example, the first module reads the existing default resource group and the second, creates a Cloud Object Storage (COS) instance and bucket in the default resource group. 

For all the blueprint commands, syntax, and option flag details, see the section [blueprint commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}


Full `blueprint create` command syntax:

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml 
```
{: pre}

On successful completion, the config create returns **`create_success`** and the unique ID of the blueprint. This ID is needed as input for all future operations against this environment. For more information, see [blueprint create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.


### Verifying blueprint config creation 
{: #verify-blueprint-create-cli}

Verify that the blueprint configuration was created successfully. When you create the configuration using the CLI, the command displays details of the blueprint modules to be created, and continuously updates the progress of the {{site.data.keyword.bpshort}} jobs initializing the modules. The command only returns on completion.

```text
Created blueprint ID: Blueprint_Basic.eaB.08d1

Modules to be created
SNO   Module Type   Name   
1     Workspace     basic-resource-group   
2     Workspace     basic-cos-storage   
      
Blueprint job us-east.JOB.Blueprint_Basic.4cad1d5b started at 2022-11-18 15:58:34

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint_Basic.4cad1d5b completed at 2022-11-18 16:00:02

Module Type   Name                   Status           Job ID   
Blueprint     Blueprint_Basic        CREATE_SUCCESS   us-east.JOB.Blueprint_Basic.4cad1d5b   
Workspace     basic-resource-group   INITIALISED         
Workspace     basic-cos-storage      INITIALISED         
              
Blueprint ID Blueprint_Basic.eaB.08d1 create_success at 2022-11-18 16:00:03
OK
```
{: screen}

On successful completion, the config create returns **`create_success`** and the unique ID of the blueprint. This ID is needed as input for all future blueprint operations. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).


## Creating a blueprint configuration using the UI 
{: #bp-ui-create}
{: ui}

You can follow these steps to create a blueprint config using {{site.data.keyword.cloud_notm}} console.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click on **Schematics** in the left hand navigator pane, then click **Blueprints** 
3. On the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}, click the **Create Blueprint** button
    - In **Blueprint Details** section:
        - **Name** `<Provide unique name for your blueprint>`.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this blueprint.
        - **Resource group** as `Default` or other resource group for this blueprint. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.
        - **Tags** as `<Provide the tag name for your blueprint>`.
        - **Description** for the blueprint. Supports maximum character range from `0 - 2048`.
        - Click **Next**.
    - In **Blueprint URL** section:
        - **Repository URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-blueprint.yaml`. Review the [blueprint URL FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-url) for details of the URL format. 
        - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
        - Check the information that is entered are correct to create a blueprint.
        - Click **Next and save as draft**. Observe that a blueprint is created with a Blueprint ID and is in `Draft` Status.
           Validation takes a few seconds to fetch the template details from the Git repo. 
           {: note}

    - In **Input Variables** section:
        - Select **Import input file** drop down only when you want to import the new `inputs` YAML file for the blueprint.
            - In **Import input file (Optional)** section:
               -  **Input file GIT URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-input.yaml`. Review the [blueprint URL FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-url) for details of the URL format. 
               - **Source name** - Used to set the source name the input file values are identified by in the UI. 
               - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
               - Click **Import values**.
        - Observe that the input variables from the `inputs.yaml` file are imported. Optionally, you can edit the variables.
           Enter variable values into the table by typing them in or by importing them. Prefilled default values, if any, were pulled from the blueprint template, but can be changed. If there is a dropdown, select a value from the dropdown.
           {: important}

        - Click **Done editing**, if the editing is done.
        - Click **Save draft** only if you need to save the draft to continue edit the input variables again later.
4. Click **Create Blueprint** that will redirect to the blueprint overview page. 

### Verifying blueprint config creation using the UI 
{: #verify-blueprint-create-ui}

Here are the steps to verify your blueprint config creation was successful.
{: shortdesc}

1. Click your blueprint that is listed in the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the create operation. 
2. Click **Overview** tab to see the blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` will show the summary details of the blueprint create job. 
3. Click **Modules** tab to see the status of the resource modules. These will be in an `Initialised` state, indicating that the module workspaces have been created, but no resources created. 
4. Click **Jobs history** tab view the result of the blueprint create job and the operations that are performed against the resource modules. 
5. Click **Settings** tab to view the summary of the new blueprint configuration.

For more information, see the [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails).

## Creating a blueprint configuration using the API
{: #create-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Create a blueprint config](/apidocs/schematics/schematics#create-blueprint) by using API.

The blueprint create API runs `blueprint create`, and `blueprint jobs` `APIs` together, to run create blueprint operations.
{: important}

Example

```json
POST /v2/blueprints HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer 

{
    "name": "Blueprint Basic Test API",
    "schema_version": "1.0.0",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "master"
        }
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "master"
                }
            }
        }
    ],
    "inputs": [
        {
            "name": "provision_rg",
            "value": "false"
        },
        {
            "name": "resource_group_name",
            "value": "myrg4"
        },
        {
            "name": "cos_instance_name",
            "value": "myrg4"
        }
    ],
    "description": "Deploys a simple two module blueprint",
    "resource_group": "Default"
}
```
{: codeblock}

### Verifying blueprint create using the API
{: #verify-bp-create-api}

Verify that the blueprint is created successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "name": "Blueprint Basic Test API",
    "source": {
        "source_type": "git_hub",
        "git": {
            "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
            "git_repo_folder": "basic-blueprint.yaml",
            "git_branch": "main",
            "git_commit": "68ce0e62f2e1b33c2341fc35fb125ffe998128d6",
            "git_commit_timestamp": "2022-11-15 11:08:48 +0000 UTC"
        },
        "catalog": {},
        "cos_bucket": {}
    },
    "config": [
        {
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
                    "git_repo_folder": "basic-input.yaml",
                    "git_branch": "main",
                    "git_commit": "68ce0e62f2e1b33c2341fc35fb125ffe998128d6",
                    "git_commit_timestamp": "2022-11-15 11:08:48 +0000 UTC"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "inputs": [
                {
                    "name": "cos_instance_name",
                    "value": "mycos4"
                },
                {
                    "name": "cos_storage_plan",
                    "value": "standard"
                }
            ]
        }
    ],
    "description": "Deploys a simple two module blueprint",
    "resource_group": "aac37f57b20142dba1a435c70aeb12df",
    "location": "us-south",
    "inputs": [
        {
            "name": "cos_instance_name",
            "value": "mycos4",
            "metadata": {
                "source": "userinput"
            }
        },
        {
            "name": "cos_storage_plan",
            "value": "standard",
            "metadata": {
                "source": "userinput"
            }
        }
    ],
    "settings": [
        {
            "name": "TF_VERSION",
            "value": "1.0",
            "metadata": {}
        }
    ],
    "outputs": [
        {
            "name": "cos_id",
            "value": "$module.basic-cos-storage.outputs.cos_id",
            "metadata": {}
        }
    ],
    "modules": [
        {
            "module_type": "terraform",
            "name": "basic-resource-group",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-DefaultResourceGroup",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "outputs": [
                {
                    "name": "resource_group_name",
                    "metadata": {}
                },
                {
                    "name": "resource_group_id",
                    "metadata": {}
                }
            ],
            "last_job": {}
        },
        {
            "module_type": "terraform",
            "name": "basic-cos-storage",
            "source": {
                "source_type": "git_hub",
                "git": {
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-Storage",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "inputs": [
                {
                    "name": "cos_instance_name",
                    "value": "$blueprint.cos_instance_name",
                    "metadata": {}
                },
                {
                    "name": "cos_storage_plan",
                    "value": "$blueprint.cos_storage_plan",
                    "metadata": {}
                },
                {
                    "name": "cos_single_site_loc",
                    "value": "ams03",
                    "metadata": {}
                },
                {
                    "name": "resource_group_id",
                    "value": "$module.basic-resource-group.outputs.resource_group_id",
                    "metadata": {}
                }
            ],
            "outputs": [
                {
                    "name": "cos_id",
                    "metadata": {}
                },
                {
                    "name": "cos_crn",
                    "metadata": {}
                }
            ],
            "last_job": {}
        }
    ],
    "flow": {},
    "blueprint_id": "Blueprint-Basic-Test-API.soB.347a",
    "crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:blueprint:Blueprint-Basic-Test-API.soB.347a",
    "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
    "created_at": "2022-11-23T14:32:45.897540037Z",
    "created_by": "test@in.ibm.com",
    "updated_at": "2022-11-23T14:32:45.897540037Z",
    "sys_lock": {
        "sys_locked_at": "0001-01-01T00:00:00Z"
    },
    "user_state": {
        "set_at": "0001-01-01T00:00:00Z"
    },
    "state": {
        "status_code": "CREATE_INIT",
        "config_status": "CONFIG_DRAFT",
        "plan_status": "PLAN_NONE"
    }
}
```
{: screen}

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-create-nextsteps}

The next step in deploying the cloud resources that are defined by the blueprint configuration is to [apply](/docs/schematics?topic=schematics-apply-blueprint) the blueprint config. 
