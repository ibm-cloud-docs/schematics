---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-17"

keywords: blueprint config create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Create a blueprint configuration 
{: #create-blueprint-config}

Deploying a blueprint environment and cloud resources using a blueprint template is a two-step process. The first step is creating a blueprint configuration in {{site.data.keyword.bpshort}}, the second step deploys this configuration  with a `blueprint run apply` operation. See [Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints) for an overview of managing deployments, and change in blueprint environments.

Creating a configuration takes as its input the blueprint template YAML and input YAML file that were created during the [defining blueprints](/docs/schematics?topic=schematics-define-blueprints) lifecycle stage.  
{: shortdesc} 

The first step in deploying cloud resources is [creating a blueprint configuration](/docs/schematics?topic=schematics-create-blueprint-config) in {{site.data.keyword.bpshort}}. This saves the blueprint configuration for future operations. The blueprint config specifies the Git source and release of the blueprint template, input files, and any input values that are used to create cloud resources. A blueprint module is created in {{site.data.keyword.bpshort}} for each module definition in the template. Each is initialized with the Terraform module source from the Git repository specified in the module definition, and module inputs.

The second [blueprint run apply](/docs/schematics?topic=schematics-apply-blueprint&interface=cli) step excutes the automation modules in dependency order and runs the module Terraform code to deploy cloud resources. 


## Creating a blueprint configuration through CLI 
{: #create-blueprint-cli}
{: cli}

Create your blueprint config with the CLI. The Create command requires a name and the Git URL of a blueprint template and other arguments. For a complete listing of options, see [blueprint config create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

To work with {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.3`.
{: important}

Before your begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.3`.
- Select the {{site.data.keyword.cloud_notm}} region that you want to use to manage your blueprint. For example, to set the region use [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

The command example shown here creates a blueprint configuration in {{site.data.keyword.bpshort}}, with the template file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. With this basic two module example, the first module creates a resource group and the second, creates a Cloud Object Storage (COS) instance and bucket in the specified resource group. 

If your template file, for example, `basic-blueprint.yaml` and input file `basic-input.yaml` are stored in a `subfolder` of the Git repository, then you need to provide complete path of the URL. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/<subfolder>`. 
{: note}

This example also demonstrates using dynamic inputs at create time to customize the deployment. In this example, the inputs `provision_rg` and `resource_group_name` are used to customize the deployment and demonstrate the use of inputs to modify module execution behavior. These additional inputs allow this blueprint config to be customized to a users account setup where the user does not have IAM permissions to create resource groups. The input `provision_rg` enables or disables provisioning of a resource group. The input `resource_group_name` specifies the name of the resource group that must be created or the name of an existing group to be used.

Only `string` values are supported for dynamic inputs. 

Where the user has the required IAM access to create resources groups, use the parameters from the 'create resource group' line. Where a user only has access to an existing group, use the parameters from the 'reuse resource group' line.  

| Operation | IAM permissions | `provision_rg` |  `resource_group_name` | 
| -- | -- | -- | -- |
| Reuse resource group | Access `default` group | false | `default` | 
| Create resource group | Create resource groups  | true  | `my_resource_group` |
{: caption="IAM permissions" caption-side="top"}

For all the blueprint commands, syntax, and option flag details, see the section [blueprint commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

### Create new resource group
{: #create-blueprint-rg-cli}

It is assumed that the user has IAM permissions to create resource groups. Syntax of the `inputs` flag to create a new resource group `my_resource_group` and create the Cloud Object Storage bucket in this group is shown. 

`-inputs provision_rg=true,resource_group_name=my_resource_group`

Full `blueprint config create` command syntax:

```sh
ibmcloud schematics blueprint config create -name Blueprint_Basic -resource-group default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml \
-inputs provision_rg=true,resource_group_name=my_resource_group
```
{: pre}

On successful completion, the config create returns **`create_success`** and the unique ID of the blueprint. This ID is needed as input for all future operations against this environment. For more information, see [blueprint config create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.



### Reuse existing resource group 
{: #reuse-blueprint-rg-cli}

Where a user does not have permissions to create a new resource group and only has access to an existing group. Syntax of the `inputs` flag to use the existing `default` resource group, and create Cloud Object Storage instance with the bucket in `default` resource group is shown. The name of the group can be amended to any group the user has view permissions. 

`-inputs provision_rg=false,resource_group_name=default`

Full `blueprint config create` command syntax:

```sh
ibmcloud schematics blueprint config create -name Blueprint_Basic -resourcegroup default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basicexample -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml \
-inputs provision_rg=false,resource_group_name=default
```
{: pre}



### Verifying blueprint config creation 
{: #verify-blueprint-create-cli}

Verify that the blueprint configuration was created successfully. When you create the configuration through CLI, the command displays details of the blueprint modules to be created, and continuously updates the progress of the {{site.data.keyword.bpshort}} jobs initializing the modules. The command only returns on completion.

```text
Created blueprint ID: Blueprint_Basic.eaB.5cd9

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

On successful completion, the config create returns **`create_success`** and the unique ID of the blueprint. This ID is needed as input for all future blueprint operations. 

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating blueprint through UI 
{: #create-blueprint-ui}
{: ui}

You can follow these steps to create the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Schematics** > **Blueprints** > **Create Blueprint**.
    - In **Blueprint Details** section:
        - **Name** `<Provide unique name for your blueprint>`.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this blueprint.
        - **Resource group** as `Default` or other resource group for this blueprint. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.
        - **Tags** as `<Provide the tag name for your blueprint>`.
        - **Description** for the blueprint. Supports maximum character range from `0 - 2048`.
        - Click **Next**.
    - In **Blueprint URL** section:
        - **Repository URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example`.
        - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
        - Check the information that is entered are correct to create a blueprint.
        - Click **Next and save as draft**. Observe the Blueprint ID is created and is in `Draft` Status.
           Validation takes few seconds to fetch the input variables from the blueprint configuration file.
           {: note}

    - In **Input Variables** section:
        - Select **Import input file** drop down only when you want to import the new `.yaml` file for the blueprint.
            - In **Import input file (Optional)** section:
               -  **Input file GIT URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-blueprint.yaml`.
               - **Source name** - Used to display from which source name the value is imported.
               - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
               - Click **Import values**.
        - Observe that the input variables from the blueprint `.yaml` file are imported. Optionally, you can edit the variables.
           Enter variable values into the table by typing them in or by importing them. Prefilled values if any were pulled from the Blueprint definition, but can be changed. If there is a dropdown, select value from the dropdown.
           {: important}

        - Click **Done editing**, if the editing is done.
        - Click **Save draft** only if you need to edit the input variables.
3. Click **Create Blueprint** that redirects to your blueprint page. 

### Verifying blueprint creation through UI 
{: #verify-blueprint-create-ui}

Here the steps to verify your blueprint creation.
{: shortdesc}

1. Click your blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the create operation. 
2. Click **Overview** tab to see the blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` must show the summary details of the blueprint config create job. 
3. Click **Modules** tab to see the status of the resource modules in an `Inactive` state.
4. Click **Jobs history** tab view the result of the blueprint config create job and operations that are performed against the resource modules.  
5. Click **Settings** tab to view the summary of the new blueprint configuration.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a blueprint through API
{: #create-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Create a blueprint config](/apidocs/schematics/schematics#create-blueprint) by using API.

The blueprint config create API runs `blueprint config create`, and `blueprint jobs` `APIs` together, to run create blueprint operations.
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
    "description": "Deploys a simple two module blueprint",
    "resource_group": "Default"
}
```
{: codeblock}

### Verifying blueprint create through API
{: #verify-bp-create-api}

Verify that the blueprint is created successfully as shown in the output.
{: shortdesc}

**Output:**

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
    “description”: “Deploys a simple two module blueprint”,
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
    “blueprint_ID”: “blueprint-basic-testdev-test.soB.13f7",
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

The next step in deploying the cloud resources that are defined by the blueprint configuration is to [apply](/docs/schematics?topic=schematics-apply-blueprint) the blueprint config. 
