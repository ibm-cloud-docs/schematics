---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: blueprint get, blueprint list, blueprint, get, list,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Displaying blueprints
{: #list-blueprint}


## Displaying blueprints using the UI 
{: #display-blueprint-ui}
{: ui}

You can follow these steps to list the blueprints in your account using {{site.data.keyword.cloud_notm}} console.

1. From the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click your blueprint name to view the blueprint details.
2. Click **Overview** to view the blueprint summary, that includes `Modules status`, `Variables summary`, `Blueprint Details` and `Recent Job runs` of your blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of modules and their current status. 
    - Optional: Click **Show details** to view the module details.
    - Optional: Click **Name** to go to the modules `Workspace` page. 
4. Click **Resources** tab to view the list of resources provisioned status by the blueprint.
5. Click **Variables** tab to view your **Inputs** and **Outputs** variables and values. Optional: you can edit the input variables and click **Save variables**.
6. Click **Jobs history** tab view the job logs of the blueprint and module activities.
7. Click **Settings** tab to view the configuration settings of the blueprint.

## Displaying blueprints using the CLI
{: #listing-bp-cli}
{: cli}

To list the blueprints available in your account with the CLI, use the `ibmcloud schematics blueprint list` command. This command requires no arguments. It is region specific and will only list blueprints in the selected CLI region. 
{: shortdesc}

For all the blueprint commands, syntax, and option flag details, see [blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

Syntax

```sh
ibmcloud schematics blueprint list
```
{: pre}

On successful completion the list command returns a list of blueprints. 

Output

The command lists all the environments created in the CLI region. 

```text
sundeepmulampaka@Sundeeps-MacBook-Pro blueprint % ibmcloud schematics blueprint list
Name                                        ID                                                   Source Type   Status               Location   Creator                   Last modified     
Blueprint_Basic                             Blueprint_Basic.eaB.08d1                             GitHub        job_finished   us-east    schematics@in.ibm.com       2022-11-18 10:37:20   
                                                                                                               
OK
```
{: screen}

### Displaying blueprint details
{: #display-blueprint-cli}

To display the details of a blueprint and its configuration with the CLI, use the `ibmcloud schematics blueprint get` command. Four levels of detail are supported with the `--level` option. 
- `summary` displays the blueprint status
- `detailed` displays the blueprint configuration, settings and source URLs. 
- `modules` A detailed listing of the blueprint template, modules and input values
- `outputs` Displays the output variables returned by the blueprint template on deploying cloud resources.

For a complete listing of options, see the [ibmcloud schematics blueprint get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-get) command.
{: shortdesc}

### Get blueprint summary 
{: #display-blueprint-summary}

Syntax

```sh
ibmcloud schematics blueprint get -id <blueprint_ID>
```
{: pre}

Output

On successful completion the get command returns summary details of the blueprint status and the module deployment status. 

```text
BLUEPRINT          
                
Name            Blueprint_Basic   
ID              Blueprint_Basic.eaB.08d1   
Description     Simple blueprint to demonstrate module linking   
Status          job_finished   
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

### Get blueprint outputs
{: #display-blueprint-outputs-cli}

Displays the output variables returned by the blueprint template after deploying cloud resources with the apply command.  
{: shortdesc}

Syntax

```sh
ibmcloud schematics blueprint get -id <blueprint_ID> -level outputs
```
{: pre}

On successful completion the get command returns the summary details of the blueprint and the output values defined by the template. The output values are returned after the summary information at the bottom of the screen. 

Output

```text
BLUEPRINT          
                
Name            Blueprint_Basic   
ID              Blueprint_Basic.eaB.08d1   
Description     Simple blueprint to demonstrate module linking   
Status          job_finished   
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

This example shows the blueprint returned the value for the output variable `cos_id`. In this example the resource id for the COS instance created by the blueprint.   

### Get blueprint module details
{: #display-blueprint-summary-cli}

Returns a detail view of the blueprint template, the modules and input and output variables. 
{: shortdesc}

Syntax

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
Status          job_finished   
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

## Displaying blueprint using the API 
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

Output

```text
{
    "total_count": 3,
    "limit": 100,
    "offset": 0,
    "blueprints": [
        {
            "name": "Blueprint Basic Json",
            "description": "Deploys a simple two module blueprint Updated",
            "source_type": "git_hub",
            "source": {
                "source_type": "",
                "git": {},
                "catalog": {},
                "cos_bucket": {}
            },
            "resource_group": "aac37f57b20142dba1a435c70aeb12df",
            "location": "us-east",
            "id": "Blueprint-Basic-Json.eaB.937a",
            "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
            "created_at": "2022-11-21T08:22:40.34093899Z",
            "created_by": "test@in.ibm.com",
            "updated_at": "2022-11-21T10:45:57.329556962Z",
            "updated_by": "test@in.ibm.com",
            "sys_lock": {
                "sys_locked_at": "0001-01-01T00:00:00Z"
            },
            "user_state": {
                "set_at": "0001-01-01T00:00:00Z"
            },
            "state": {
                "status_code": "UPDATE_INIT",
                "config_status": "CONFIG_DRAFT",
                "plan_status": "PLAN"
            }
        },
        {
            "name": "Blueprint Basic Json",
            "description": "Deploys a simple two module blueprint",
            "source_type": "git_hub",
            "source": {
                "source_type": "",
                "git": {},
                "catalog": {},
                "cos_bucket": {}
            },
            "resource_group": "aac37f57b20142dba1a435c70aeb12df",
            "location": "us-east",
            "id": "Blueprint-Basic-Json.eaB.c234",
            "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
            "created_at": "2022-11-21T06:26:30.521761577Z",
            "created_by": "test@in.ibm.com",
            "updated_at": "2022-11-21T06:36:05.159936827Z",
            "sys_lock": {
                "sys_locked_at": "0001-01-01T00:00:00Z"
            },
            "user_state": {
                "set_at": "0001-01-01T00:00:00Z"
            },
            "state": {
                "status_code": "job_finished",
                "config_status": "CONFIG_DRAFT",
                "plan_status": "PLANNED",
                "run_status": "RUN_APPLY_COMPLETE"
            }
        },
        {
            "name": "Blueprint Basic Test API",
            "description": "Deploys a simple two module blueprint",
            "source_type": "git_hub",
            "source": {
                "source_type": "",
                "git": {},
                "catalog": {},
                "cos_bucket": {}
            },
            "resource_group": "aac37f57b20142dba1a435c70aeb12df",
            "location": "us-south",
            "id": "Blueprint-Basic-Test-API.soB.5a2a",
            "account": "1f7277194bb748cdb1d35fd8fb85a7cb",
            "created_at": "2022-11-23T13:57:35.042946592Z",
            "created_by": "test@in.ibm.com",
            "updated_at": "2022-11-23T13:57:33.71320227Z",
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
        },
    ]
}
```
{: screen}

This API lists the detailed information with respect to the blueprint ID.
{: important}

Example

```json
GET /v2/blueprints/Blueprint-Basic-Test-API.soB.347a/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

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

For more information, about how to diagnose and resolve issues if the list job fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-create-fails&interface=cli).

## Next steps
{: #bp-display-nextsteps}

After displaying the list of blueprints in {{site.data.keyword.bpshort}}, refer to [list blueprint jobs](/docs/schematics?topic=schematics-list-blueprint-jobs) for details on displaying blueprint jobs. 


