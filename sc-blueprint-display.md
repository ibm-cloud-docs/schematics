---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-21"

keywords: blueprint get, blueprint list, blueprint, get, list,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# List blueprints
{: #list-blueprint}

To list your blueprints with the CLI, use the `ibmcloud schematics blueprint list` command. This command requires no arguments. It is region specific and will only list blueprints in the selected CLI region. 
{: shortdesc}

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

## Listing blueprints through UI 
{: #display-blueprint-ui}
{: ui}

You can follow these steps to list the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [{{site.data.keyword.cloud_notm}} Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click your blueprint name to view the blueprint details.
2. Click **Overview** to view the blueprint summary, that includes `Modules status`, `Variables summary`, `Blueprint Details` and `Recent Job runs` of your blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of modules and their current status. 
    - Optional: Click **Show details** to view the module details.
    - Optional: Click **Name** that takes to the modules `Workspace` page. 
4. Click **Resources** tab to view the list of resources provisioned status by the blueprint.
5. Click **Variables** tab to view your **Inputs** and **Outputs** variables and values. Optional: you can edit the input variable and click **Save variables**.
6. Click **Jobs history** tab view the job logs of the blueprint and module activities.
7. Click **Settings** tab to view the configuration settings of the blueprint.

## Listing blueprints through CLI
{: #listing-bp-cli}
{: cli}

Lists all the blueprints.
{: shortdesc}

**`Syntax`**

```sh
ibmcloud schematics blueprint list
```
{: pre}

On successful completion the list command returns a list of blueprints.  

**Output:**

The command lists all the environments created in the CLI region. 

```text
sundeepmulampaka@Sundeeps-MacBook-Pro blueprint % ibmcloud schematics blueprint list
Name                                        ID                                                   Source Type   Status               Location   Creator                   Last modified     
Blueprint_Basic                             Blueprint_Basic.eaB.08d1                             GitHub        fulfilment_success   us-east    schematics@in.ibm.com       2022-11-18 10:37:20   
                                                                                                               
OK
```
{: screen}

### Listing blueprints
{: #display-blueprint-cli}

To display the details of a blueprint and its configuration with the CLI, use the `ibmcloud schematics blueprint get` command. Four levels of detail are supported with the `--level` option. 
- `summary` blueprint run status
- `detailed` blueprint configuration, settings and source URLs. 
- `modules` Detailed listing of the blueprint template, modules and variables
- `outputs` Output variables returned by the blueprint template on deploying cloud resources.

For a complete listing of options, see the [ibmcloud schematics blueprint get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-get) command.
{: shortdesc}

### Blueprint display summary 
{: #display-blueprint-summary}

**`Syntax`**

```sh
ibmcloud schematics blueprint get -id <blueprint_ID>
```
{: pre}

**Output:**

On successful completion the get command returns summary details of the blueprint and module status.  

```text
BLUEPRINT          
                
Name            Blueprint_Basic   
ID              Blueprint_Basic.eaB.08d1   
Description     Simple blueprint to demonstrate module linking   
Status          fulfilment_success   
Location        us-east   
Creator         schematics@in.ibm.com   
Last modified   2022-11-18T10:37:20.163Z   
                
MODULES

SNO   Workspace Name         Workspace ID                                      Status    Location   Updated   
1     basic-resource-group   us-east.workspace.basic-resource-group.99503dea   APPLIED   us-east       
2     basic-cos-storage      us-east.workspace.basic-cos-storage.99a35e10      APPLIED   us-east       
      
OK
```
{: screen}

### Blueprint display outputs
{: #display-blueprint-outputs-cli}

Displays the blueprint with the summary information.
{: shortdesc}

**`Syntax`**

```sh
ibmcloud schematics blueprint get -id <blueprint_ID> -level outputs
```
{: pre}

On successful completion the get command returns the summary details of the blueprint and the output values defined by the template.  

**Output:**

```text
BLUEPRINT          
                
Name            Blueprint_Basic   
ID              Blueprint_Basic.eaB.08d1   
Description     Simple blueprint to demonstrate module linking   
Status          fulfilment_success   
Location        us-east   
Creator         schematics@in.ibm.com   
Last modified   2022-11-18T10:37:20.163Z   
                
MODULES

SNO   Workspace Name         Workspace ID                                      Status    Location   Updated   
1     basic-resource-group   us-east.workspace.basic-resource-group.99503dea   APPLIED   us-east       
2     basic-cos-storage      us-east.workspace.basic-cos-storage.99a35e10      APPLIED   us-east       
      
BLUEPRINT OUTPUTS

Key      Value   
cos_id   088afa58-0693-4a8d-b0e1-523e9d5325cf   
            
OK
```
{: screen}

This example shows the returned computed value for the output variable `blueprint-output`  

### Blueprint display summary CLI
{: #display-blueprint-summary-cli}

Displays the output with the module level.
{: shortdesc}

**`Syntax`**

```sh
ibmcloud schematics blueprint get -id <blueprint_ID> -level modules
```
{: pre}

On successful completion the get command returns a detailed listing of the blueprint template, modules and variables

```text
BLUEPRINT          
                
Name            Blueprint_Basic   
ID              Blueprint_Basic.eaB.08d1   
Description     Simple blueprint to demonstrate module linking   
Status          fulfilment_success   
Location        us-east   
Creator         schematics@in.ibm.com   
Last modified   2022-11-18T10:37:20.163Z   
                
BLUEPRINT
                   
Name            Blueprint_Basic   
Description     Simple blueprint to demonstrate module linking   
Git Repo URL    https://github.com/Cloud-Schematics/blueprint-basic-example   
Git Repo File   basic-blueprint.yaml   
Git Branch      main   
                
BLUEPRINT INPUTS

Key                 Value   
cos_instance_name   Blueprint-basic   
cos_storage_plan    lite   
                       
BLUEPRINT OUTPUTS

Key      Value   
cos_id   088afa58-0693-4a8d-b0e1-523e9d5325cf   
            
INPUT FILE
                   
Git Repo URL    https://github.com/Cloud-Schematics/blueprint-basic-example   
Git Repo File   basic-input.yaml   
Git Branch      main   
                
INPUTS

Key                 Value   
cos_instance_name   Blueprint-basic   
cos_storage_plan    lite   
                       
MODULES

basic-resource-group      
Git Repo URL           https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-DefaultResourceGroup   
Git Repo Branch        main   
                       
INPUTS

Key   Value   
      
OUTPUTS

Key   
resource_group_name   
resource_group_id   
   
   
basic-cos-storage      
Git Repo URL        https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-Storage   
Git Repo Branch     main   
                    
INPUTS

Key                   Value   
cos_instance_name     $blueprint.cos_instance_name   
cos_storage_plan      $blueprint.cos_storage_plan   
cos_single_site_loc   ams03   
resource_group_id     $module.basic-resource-group.outputs.resource_group_id   
                      
OUTPUTS

Key   
cos_id   
cos_crn   
   
   
OK
```
{: screen}

## Displaying blueprint through API 
{: #display-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about configuration of the blueprint and its outputs, refer to, [Display blueprint](/apidocs/schematics/schematics#get-blueprint) by using API.

Blueprint display API `blueprint_list` runs the configuration of the blueprint and detailed output.
{: important}

Displays blueprint list:

```json
GET /v2/blueprints/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

**`Output`**

```text
{
    "total_count": 2,
    "limit": 100,
    "offset": 0,
    "blueprints": [
        {
            "name": "Blueprint Basic Test",
            "description": "Deploys a simple two module blueprint",
            "resource_group": "aac37f57b20142dba1a435c70aeb12df",
            "location": "us-south",
            "id": "Blueprint-Basic-Test.soB.9403",
            "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
            "created_at": "2022-09-15T07:04:10.477299538Z",
            "created_by": "smulampa@in.ibm.com",
            "updated_at": "0001-01-01T00:00:00Z",
            "sys_lock": {
                "sys_locked_at": "0001-01-01T00:00:00Z"
            },
            "user_state": {
                "state": "Environment_Create_Init",
                "set_at": "0001-01-01T00:00:00Z"
            },
            "state": {}
        },
        {
            "name": "Blueprint FVT",
            "description": "Deploys dev environment instance in Toronto Region",
            "resource_group": "f8ceaec00ee14de48ee802cf11202a81",
            "tags": [
                "blueprint:Tor-Dev"
            ],
            "location": "us-east",
            "id": "Blueprint-FVT.eaB.b629",
            "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
            "created_at": "2022-09-08T14:15:26.005648449Z",
            "created_by": "Nishu.Bharti1@ibm.com",
            "updated_at": "2022-09-08T14:17:19.002748955Z",
            "sys_lock": {
                "sys_locked_at": "0001-01-01T00:00:00Z"
            },
            "user_state": {
                "state": "Environment_Create_Init",
                "set_at": "0001-01-01T00:00:00Z"
            },
            "state": {
                "status_code": "CREATE_SUCCESS"
            }
        }
        }
    ]
}
```
{: screen}

This API lists the detailed information with respect to the blueprint ID.
{: important}

Example

```json
GET /v2/blueprints/Blueprint-Basic-Test.eaB.bbb9/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

**`Output`**

```text
{
    "name": "Blueprint Basic Test",
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
                    "git_branch": "master"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "inputs": [
                {
                    "name": "resource_group_name",
                    "value": "Default"
                },
                {
                    "name": "provision_rg",
                    "value": "false"
                },
                {
                    "name": "cos_instance_name",
                    "value": "mycos-test-msk"
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
    "location": "us-east",
    "inputs": [
        {
            "name": "resource_group_name",
            "metadata": {}
        },
        {
            "name": "provision_rg",
            "metadata": {}
        },
        {
            "name": "cos_instance_name",
            "metadata": {}
        },
        {
            "name": "cos_storage_plan",
            "metadata": {}
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
                    "git_repo_url": "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup",
                    "git_branch": "main"
                },
                "catalog": {},
                "cos_bucket": {}
            },
            "created_at": "0001-01-01T00:00:00Z",
            "updated_at": "0001-01-01T00:00:00Z",
            "inputs": [
                {
                    "name": "provision",
                    "value": "$blueprint.provision_rg"
                },
                {
                    "name": "name",
                    "value": "$blueprint.resource_group_name"
                }
            ],
            "outputs": [
                {
                    "name": "resource_group_name"
                },
                {
                    "name": "resource_group_id"
                }
            ],
            "last_job": {}
        },
        {
            "module_type": "terraform",
            "name": "basic-cos-storage",
            "layer": "DB",
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
                    "value": "$blueprint.cos_instance_name"
                },
                {
                    "name": "cos_storage_plan",
                    "value": "$blueprint.cos_storage_plan"
                },
                {
                    "name": "cos_single_site_loc",
                    "value": "ams03"
                },
                {
                    "name": "resource_group_id",
                    "value": "$module.basic-resource-group.outputs.resource_group_id"
                }
            ],
            "outputs": [
                {
                    "name": "cos_id"
                },
                {
                    "name": "cos_crn"
                }
            ],
            "last_job": {}
        }
    ],
    "flow": {},
    "blueprint_ID": "Blueprint-Basic-Test.eaB.bbb9",
    "crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:blueprint:Blueprint-Basic-Test.eaB.e03e",
    "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
    "created_at": "2022-09-19T10:33:13.029985037Z",
    "created_by": "smulampa@in.ibm.com",
    "updated_at": "0001-01-01T00:00:00Z",
    "sys_lock": {
        "sys_locked_at": "0001-01-01T00:00:00Z"
    },
    "user_state": {
        "state": "Environment_Create_Init",
        "set_at": "0001-01-01T00:00:00Z"
    },
    "state": {}
}
```
{: screen}

For more information, about how to diagnose and resolve issues if the list job fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-display-nextsteps}

After displaying the list of blueprints in {{site.data.keyword.bpshort}}, refer to [list blueprint jobs](/docs/schematics?topic=schematics-list-blueprint-jobs) for details on displaying blueprint jobs.  


