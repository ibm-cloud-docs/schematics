---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-08"

keywords: blueprint create, create blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Creating a Blueprint 
{: #create-blueprint}

Deploying cloud resources with Blueprints is a two step process, create and install. The first step creates a Blueprint in {{site.data.keyword.bpshort}}. The second [install](/docs/schematics?topic=schematics-sc-blueprint-install) step executes the IaC automation modules to deploy cloud resources. Refer to [Blueprints lifecycle](/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands such as `create`, `update`, and `delete`.
{: shortdesc} 

The first step in deploying cloud resources is the creation of a Blueprint in {{site.data.keyword.bpshort}}. This saves the Blueprint configuration for future operations. The Blueprint config specfies the Git source and release of the Blueprint definition, input files, and any optional input values that will be used to create cloud resources. A linked Workspace is created for each module in the Blueprint definition, initialised from the modules IaC source repository and module inputs.

## Creating a Blueprint from the UI 
{: #create-blueprint-ui}
{: ui}

1. Open the [{{site.data.keyword.bpshort}} Blueprints page](https://cloud.ibm.com/schematics/blueprints){: external}. 
2. Click **Create Blueprint via CLI**.
    Currently, you can only create Blueprint from command-line. Follow the [create command](/docs/schematics?topic=schematics-create-blueprint&interface=cli) to create a Blueprint and [install](/docs/schematics?topic=schematics-install-blueprint) commands.
    {: note}

## Verify Blueprint creation from the UI 
{: #bp-verify-create-ui}

1. Click your Blueprint that are listed from the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Overview** to view the summary such as `Modules`, `Variables`, `Details`, `Recent Job runs` of your Blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of resource modules that are in `Active` status.
4. Click **Resource** tab to view your provisioned resources list.
5. Click **Variables** tab to view your **Inputs** and **Outputs** configurations.
6. Click **Jobs history** tab view all Blueprints, and module activities in the jobs log.
7. Click **Settings** tab to view the summary of the deployed Blueprint.

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Creating a Blueprint from the CLI 
{: #create-blueprint-cli}
{: cli}

To create and deploy your Blueprint with the CLI, use the `ibmcloud schematics blueprint create` command. This command requires a name and the Git URL of a Blueprint definition and other optional arguments. For a complete listing of options, see the [ibmcloud schematics blueprint create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

Before your begin

- Install and log in to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- Select the {{site.data.keyword.cloud_notm}} region you wish to use to manage your {{site.data.keyword.bpshort}} Blueprints environment from. Set the region by running the command `ibmcloud target --region <us-south>`.
- Install the [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) plug-in, or [update the command line plug-in](/docs/schematics?topic=schematics-setup-cli#schematics-cli-update) to access the {{site.data.keyword.bpshort}} Blueprints commands.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.

The following command creates a Blueprint by using the definition file `basic-blueprint.yaml` and input file `basic-input.yaml` from the source Git repository `https://github.com/Cloud-Schematics/blueprint-basic-example`. This Blueprint definition requires the two inputs `provision_rg=true` and `resource_group_name=default` are passed. 

**Syntax:**

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=default
```
{: pre}

On successful completion the create command returns **create_success** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint.  

For more information, about the command options, see [Create command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create).

## Verify Blueprint creation 
{: #bp-verify-create}

Verify that the Blueprint has been created successfully. When you create the Blueprint from the CLI, the command displays details of the linked Workspaces to be created and a continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initalising the Workspaces. The command only returns on completion.

```text
Created Blueprint ID: eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
Modules to be created
SNO   Type        Name   
1     Workspace   basic-resource-group   
2     Workspace   basic-cos-storage   
      
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.da1b13ca
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   

Type        Name                      Status           Job ID   
Blueprint   Blueprint Basic Example   CREATE_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.da1b13ca   
Workspace   basic-resource-group      INACTIVE            
Workspace   basic-cos-storage         INACTIVE            
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 create_success at Mon Jun 27 16:18:47 BST 2022
OK
```
{: screen}

On successful completion the create command will return **create_success** and the unique ID of the Blueprint created. This ID is required as input for all future `schematics blueprint` operations against this Blueprint. 

For more information, about how to diagnose and resolve issues if the create fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).


## Creating a Blueprint from the API
{: #create-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. [Create a Blueprint](/apidocs/schematics/schematics#create-blueprint) by using API.

Blueprint create API runs `Blueprint create`, and `Blueprint jobs` APIs together, to performs the create and install Blueprint operations.
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
