---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-21"

keywords: blueprint create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Creating a Blueprint 
{: #create-blueprint}

Deploying cloud resources by using a solution pattern with the Blueprints service is a two-step process such as `Create`, and `Apply`. See [Deploying Blueprints](/docs/schematics?topic=sc-bp-deploy) for an overview of the Blueprints deployment lifecycle stage and the two-phase approach to managing deployments, and change in Blueprint environments.

Create takes as its input the Blueprint definitions and input configurations that are created during the [Defining Blueprints](/docs/schematics?topic=sc-bp-define) lifecycle stage.  
{: shortdesc} 

The first step in deploying cloud resources is the [creating](/docs/schematics?topic=schematics-apply-blueprint#create-blueprint-cli) of a Blueprint in {{site.data.keyword.bpshort}}. It saves the Blueprint configuration for future operations. The Blueprint config specifies the Git source and release of the Blueprint definition, input files, and any input values that are used to create cloud resources. A linked Workspace is created for each module in the Blueprint definition. The Workspaces are initialized from the modules Terraform source Git repository and module inputs.

The second [Apply](/docs/schematics?topic=schematics-apply-blueprint&interface=cli) step runs the automation modules and the Terraform code to deploy cloud resources. 


## Creating a Blueprint from the CLI 
{: #create-blueprint-cli}
{: cli}

Create your Blueprint with the CLI. The Create command requires a name and the Git URL of a Blueprint definition and other arguments. For a complete listing of options, see [create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

To work with {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.11.0`.
{: important}

Before your begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.11.0`.
- Select the {{site.data.keyword.cloud_notm}} region that you want to use to manage your {{site.data.keyword.bpshort}} Blueprint. For example, to set the region use [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

The command example that is used here creates a Blueprint in {{site.data.keyword.bpshort}} the definition file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. With this basic two module example, the first module creates a resource group and the second, create a Cloud Object Storage instance and bucket in the specified resource group. 

If your definition file `basic-blueprint.yaml` and input file `basic-input.yaml` are stored in a `subfolder` of the Git repository, then you need to provide complete path of the URL. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/<subfolder>`. 
{: note}

This example also demonstrates by using more inputs at create time to customize the deployment. In this example, the inputs `provision_rg` and `resource_group_name` are used to customize the deployment and demonstrate the use of inputs to modify module execution behavior. These additional inputs allow the Blueprint deployment to be customized to a user account setup and IAM permissions to access or create resource groups. The input `provision_rg` enables or disables provisioning of a resource group. The input `resource_group_name` specifies the name of the resource group that must be created or the name of an existing group to be used.

| Operation | IAM permissions | provision_rg |  resource_group_name | 
| -- | -- | -- | -- |
| Reuse resource group | Access `default` group | false | default | 
| Create resource group | Create resource groups  | true  | my_resource_group |
{: caption="IAM permissions" caption-side="top"}

For all the Blueprints commands, syntax, and option flag details, see [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

### Reuse existing resource group 
{: #reuse-blueprint-rg-cli}

Syntax of the `inputs` flag to use the existing `default` resource group, and create Cloud Object Storage instance with the bucket in `default` resource group.

`-inputs provision_rg=false,resource_group_name=default`

Full `blueprint create` command syntax:

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resourcegroup default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basicexample -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml \
-inputs provision_rg=false,resource_group_name=default
```
{: pre}

### Create new resource group
{: #create-blueprint-rg-cli}

Syntax of the `inputs` flag to create a new resource group `my_resource_group` and create the Cloud Object Storage bucket in this group. 

`-inputs provision_rg=true,resource_group_name=my_resource_group`

Full `blueprint create` command syntax:

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml \
-inputs provision_rg=true,resource_group_name=my_resource_group
```
{: pre}

On successful completion, create command returns **`create_success`** and the unique ID of the Blueprint created. This ID is needed as input for all future `schematics blueprint` operations against this Blueprint. For more information, see [Create command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create).

### Verify Blueprint create 
{: #verify-blueprint-create-cli}

Verify that the Blueprints are created successfully. When you create the Blueprint from the CLI, the command displays details of the linked Workspaces to be created, and continuously updates the progress of the {{site.data.keyword.bpshort}} jobs initializing the Workspaces. The command only returns on completion.

```text
Created Blueprint ID: Blueprint_Basic.eaB.5cd9

Modules to be created
SNO Type Name
1 terraform basic-resource-group
2 terraform basic-cos-storage

Blueprint job running us-east.JOB.Blueprint_Basic.bb553ac5
Waiting:0 Draft:0 Connecting:0 In Progress:0 Inactive:2 Active:0
Failed:0
Type Name Status Job ID
Blueprint Blueprint_Basic CREATE_SUCCESS useast.JOB.Blueprint_Basic.bb553ac5
terraform basic-resource-group INACTIVE
terraform basic-cos-storage INACTIVE

Blueprint ID Blueprint_Basic.eaB.5cd9 create_success at 2022-08-03
21:19:16

OK
```
{: screen}

On successful completion, create command returns **`create_success`** and the unique ID of the Blueprint created. This ID is needed as input for all future `schematics blueprint` operations against this Blueprint. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a Blueprint from the UI 
{: #create-blueprint-ui}
{: ui}

Currently, you can create a Blueprint from command line by using the [Create command](/docs/schematics?topic=schematics-create-blueprint&interface=cli). Followed by [Apply](/docs/schematics?topic=schematics-apply-blueprint) command to create cloud resources.
{: note}

### Verify Blueprint creation from the UI 
{: #verify-blueprint-create-ui}

Here the steps to verify your Blueprint creation.
{: shortdesc}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the create operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` must show the summary details of the Blueprint create job. 
3. Click **Modules** tab to see the status of the resource modules in an `Inactive` state.
4. Click **Jobs history** tab view the result of the Blueprint create job and operations that are performed against the resource modules.  
5. Click **Settings** tab to view the summary of the new Blueprint configuration.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a Blueprint from the API
{: #create-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Create a Blueprint](/apidocs/schematics/schematics#create-blueprint) by using API.

The Blueprint create API runs `blueprint create`, and `blueprint jobs` `APIs` together, to set the create Blueprint operations.
{: important}

Example

```json
POST /v2/blueprints HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer 

{
    "name": "Blueprint Basic Test",
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
    "description": "Deploys a simple two module Blueprint",
    "resource_group": "Default"
}
```
{: codeblock}

### Verify Blueprint create from the API
{: #verify-bp-update-api}

**`Output`**

```text
{
    “name”: “Blueprint Basic Example”,
    “source”: {
        “source_type”: “git_hub”,
        “git”: {
            “git_repo_url”: “https://github.com/KshamaG/blueprint-basic-example”,
            “git_repo_folder”: “basic-blueprint.yaml”
        },
        “catalog”: {},
        “cos_bucket”: {}
    },
    “config”: [
        {
            “source”: {
                “source_type”: “git_hub”,
                “git”: {
                    “git_repo_url”: “https://github.com/KshamaG/blueprint-basic-example”,
                    “git_repo_folder”: “basic-input.yaml”,
                    “git_branch”: “master”
                },
                “catalog”: {},
                “cos_bucket”: {}
            },
            “inputs”: [
                {
                    “name”: “resource_group_name”,
                    “value”: “bp-rg-test1"
                },
                {
                    “name”: “provision_rg”,
                    “value”: “true”
                },
                {
                    “name”: “cos_instance_name”,
                    “value”: “bp-cos-test1"
                }
            ]
        }
    ],
    “description”: “Deploys a simple two module Blueprint”,
    “resource_group”: “aac37f57b20142dba1a435c70aeb12df”,
    “location”: “us-south”,
    “inputs”: [
        {
            “name”: “resource_group_name”,
            “metadata”: {}
        },
        {
            “name”: “provision_rg”,
            “metadata”: {}
        },
        {
            “name”: “cos_instance_name”,
            “metadata”: {}
        }
    ],
    “settings”: [
        {
            “name”: “TF_VERSION”,
            “value”: “1.0”,
            “metadata”: {}
        }
    ],
    “outputs”: [
        {
            “name”: “cos_id”,
            “value”: “$module.basic-cos-storage-test1.outputs.cos_id”,
            “metadata”: {}
        }
    ],
    “modules”: [
        {
            “module_type”: “terraform”,
            “name”: “basic-resource-group-test1”,
            “layer”: “RG”,
            “source”: {
                “source_type”: “git_hub”,
                “git”: {
                    “git_repo_url”: “https://github.com/Cloud-Schematics/blueprint-basic-example/tree/master/IBM-ResourceGroup”,
                    “git_branch”: “master”
                },
                “catalog”: {},
                “cos_bucket”: {}
            },
            “created_at”: “0001-01-01T00:00:00Z”,
            “updated_at”: “0001-01-01T00:00:00Z”,
            “inputs”: [
                {
                    “name”: “provision”,
                    “value”: “$blueprint.provision_rg”
                },
                {
                    “name”: “name”,
                    “value”: “$blueprint.resource_group_name”
                }
            ],
            “outputs”: [
                {
                    “name”: “resource_group_name”
                },
                {
                    “name”: “resource_group_id”
                }
            ],
            “last_job”: {}
        },
        {
    ........,
    “flow”: {},
    “blueprint_id”: “blueprint-basic-testdev-test.soB.13f7",
    “crn”: “crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:blueprint:blueprint-basic-testdev-smulampa.soB.13f7",
    “account”: “1f7277194bb748cdb1d35fd8fb85a7cb”,
    “created_at”: “2022-09-14T08:00:32.029373639Z”,
    “created_by”: “test@in.ibm.com”,
    “updated_at”: “0001-01-01T00:00:00Z”,
    “sys_lock”: {
        “sys_locked_at”: “0001-01-01T00:00:00Z”
    },
    “user_state”: {
        “state”: “Environment_Create_Init”,
        “set_at”: “0001-01-01T00:00:00Z”
    },
    “state”: {}
}
```
{: screen}

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-create-nextsteps}

The next step in deploying the cloud resources that are defined by the Blueprint is to [Apply](/docs/schematics?topic=schematics-apply-blueprint) the Blueprint. 

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
