---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-17"

keywords: blueprint create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Creating a Blueprint 
{: #create-blueprint}

Deploying cloud resources with Blueprints is a two step process, create and install. The first step creates a Blueprint in {{site.data.keyword.bpshort}}. The second [install](/docs/schematics?topic=schematics-install-blueprint&interface=cli) step executes the IaC automation modules to deploy cloud resources. Refer to [Blueprints lifecycle](/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands such as `create`, `update`, and `delete`.
{: shortdesc} 

The first step in deploying the cloud resources is the creation of a Blueprint in {{site.data.keyword.bpshort}}. This saves the Blueprint configuration for future operations. The Blueprint config specifies the Git source and release of the Blueprint definition, input files, and any optional input values that will be used to create cloud resources. A linked Workspace is created for each module in the Blueprint definition, initialized from the modules IaC source repository and module inputs.

## Creating a Blueprint from the CLI 
{: #create-blueprint-cli}
{: cli}

Create and deploy your Blueprint with the CLI. Create command requires a name and the Git URL of a Blueprint definition and other optional arguments. For a complete listing of options, see the [ibmcloud schematics blueprint create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.11.0` version.
{: important}

Before your begin

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.11.0` version.
- Select the {{site.data.keyword.cloud_notm}} region you want to use to manage your {{site.data.keyword.bpshort}}. For example, to set the region use [`ibmcloud target -r <provide your region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

The following command creates a Blueprint by using the definition file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. This Blueprint definition requires the two inputs `provision_rg=true` and `resource_group_name=default` are passed during the Blueprint creation. You can create Blueprint using `default` or new resource group `mynew-resourcegroup`, both the syntax are provided.

If your definition file `basic-blueprint.yaml` and input file `basic-input.yaml` are stored in a `subfolder` of the Git repository, then you need to provide complete path of the URL. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/<subfolder>`. 
{: note}

**Syntax: To create a resource group named `mynewrgdemo`, and create COS instance with the bucket in `mynewrg` resource group**

For all the Blueprints commands, syntax, and option flag details, refer to, [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basicexample -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-gitbranch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=mynewrgdemo
```
{: pre}

**Syntax: To use the `default` resource group, and create COS instance with the bucket in `default` resource group**

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resourcegroup default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basicexample -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-gitbranch main -input-git-file basic-input.yaml -inputs provision_rg=false,resource_group_name=default
```
{: pre}

On successful completion the create command returns **`create_success`** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint. For more information, about the command options, see [Create command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create).

### Verify Blueprint create 
{: #verify-blueprint-create-cli}

Verify that the Blueprint has been created successfully. When you create the Blueprint from the CLI, the command displays details of the linked Workspaces to be created and a continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initalizing the Workspaces. The command only returns on completion.

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

On successful completion the create command will return **`create_success`** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint. 

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a Blueprint from the UI 
{: #create-blueprint-ui}
{: ui}

Currently, you can only create a Blueprint from command-line by using the [create command](/docs/schematics?topic=schematics-create-blueprint&interface=cli). Followed by [install](/docs/schematics?topic=schematics-install-blueprint) command to create cloud resources.
{: note}

### Verify Blueprint creation from the UI 
{: #verify-blueprint-create-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the create operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` should show the summary details of the Blueprint create job. 
3. Click **Modules** tab to see the status of the resource modules. These will be in `Inactive` state.
4. Click **Jobs history** tab view the result of the Blueprint create job and operations performed against the resource modules.  
5. Click **Settings** tab to view the summary of the new Blueprint configuration.

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a Blueprint from the API
{: #create-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about Blueprint update, refer to, [Create a Blueprint](/apidocs/schematics/schematics#create-blueprint) by using API.

Blueprint create API runs `Blueprint create`, and `Blueprint jobs` `APIs` together, to perform the create, and install Blueprint operations.
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

Output:

```text
{
  "name": "Blueprint Basic Example",
  "source": {
    "source_type": "git_hub",
    "git": {
      "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-basic-example",
      "git_repo_folder": "basic-blueprint.yaml"
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
          "git_branch": "main"
        },
        "catalog": {},
        "cos_bucket": {}
      },
      "inputs": [
        {
          "name": "cos_instance_name",
          "value": "Blueprint-basic"
        },
        {
          "name": "resource_group_name",
          "value": "myrg4"
        },
        {
          "name": "provision_rg",
          "value": "true"
        }
      ]
    }
  ],
  "description": "Simple two module blueprint. Deploys Resource Group and COS bucket",
  "resource_group": "47ecbb1f38ea4b8aa0a091edb1e4e909",
  ........
  "flow": {},
  "blueprint_id": "us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d",
  "crn": "crn:v1:bluemix:public:schematics:us-south:a/16a85b7b99a6622e7c186fb6503781a0:17e412e5-dfac-486a-804c-907d21a4454b:blueprint:us-south.BLUEPRINT.Blueprint-Basic-Example.b14a205d",
  "account": "16a85b7b99a6622e7c186fb6503781a0",
  "created_at": "2022-07-01T08:21:30.145Z",
  "created_by": "kgurudut@in.ibm.com",
  "updated_at": "1901-01-01T00:00:00.000Z",
  "sys_lock": {
    "sys_locked_at": "1901-01-01T00:00:00.000Z"
  },
  "user_state": {
    "state": "Environment_Create_Init",
    "set_at": "1901-01-01T00:00:00.000Z"
  },
  "state": {}
}
```
{: screen}

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-create-nextsteps}

After creating the Blueprint in {{site.data.keyword.bpshort}}, the next step in deploying the cloud resources defined by the Blueprint is to [install](/docs/schematics?topic=schematics-install-blueprint) the Blueprint. 

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
