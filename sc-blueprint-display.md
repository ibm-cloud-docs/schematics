---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-04"

keywords: blueprint get, blueprint list, blueprint, get, list,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Listing blueprint environments
{: #list-blueprint-cli}

To list your blueprint environments with the CLI, use the `ibmcloud schematics blueprint list` command. This command requires no arguments, but is region specific and will only list blueprint environments in the selected CLI region. 
{: shortdesc}

For all the blueprint commands, syntax, and option flag details, see [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

## Listing blueprint environments via CLI
{: #listing-bp-cli}
{: cli}

Lists all the blueprint environments.
{: shortdesc}

**`Syntax`**

```sh
ibmcloud schematics blueprint list
```
{: pre}

On successful completion the list command returns a list of blueprint environments.  

**Output:**

The command lists all the environments created in the CLI region. 

```text
Name                        ID                                                   Status   Location   Creator                   Last modified   
Blueprint Basic Example     eu-gb.BLUEPRINT.Blueprint-Basic-Example.f612085f     Normal   eu-gb      steve_strutt@uk.ibm.com   2022-07-03T14:09:02.354Z   
blueprints-complex-inputs   eu-gb.BLUEPRINT.blueprints-complex-inputs.7312c775   Normal   eu-gb      steve_strutt@uk.ibm.com   2022-07-04T13:01:50.313Z   
                                                                                          
OK
```
{: screen}


### Displaying blueprint environments
{: #display-blueprint-cli}

To display the details of a blueprint environment and its configuration with the CLI, use the `ibmcloud schematics blueprint get` command. Four levels of detail are supported with the `--level` option. 
- `summary` blueprint run status
- `detailed` blueprint configuration, settings and source URLs. 
- `modules` Detailed listing of the blueprint template, modules and variables
- `outputs` Output variables returned by the blueprint template on deploying cloud resources.

For a complete listing of options, see the [ibmcloud schematics blueprint get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-get) command.
{: shortdesc}

### Blueprint display summary 
{: #display-blueprint-summary-cli}

**`Syntax`**

```sh
ibmcloud schematics blueprint get -id <blueprint_id>
```
{: pre}

**Output:**

On successful completion the get command returns summary details of the Blueprint and module status.  

```text
BLUEPRINT          
                
Name            Blueprint_Complex   
ID              us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
Status          Normal   
Location        us-south   
Creator         geetha_sathyamurthy@in.ibm.com   
Last modified   2022-07-08T13:56:43.994Z   
                
MODULES

SNO   Workspace Name      Workspace ID                                    Status   Location   Updated   
1     terraform_module1   us-south.workspace.terraform_module1.6cef8e6d   ACTIVE   us-south      
2     terraform_module2   us-south.workspace.terraform_module2.875fda22   ACTIVE   us-south      
      
OK
```
{: screen}


### Blueprint display outputs
{: #display-blueprint-outputs-cli}

Displays the blueprint environment with the summary information.
{: shortdesc}

**`Syntax`**

```sh
ibmcloud schematics blueprint get -id <blueprint_id> -level outputs
```
{: pre}

On successful completion the get command returns the summary details of the blueprint environment and the output values defined by the template.  

**Output:**

```text
BLUEPRINT

Name            Blueprint_Complex
ID              us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
Status          Normal   
Location        us-south   
Creator         geetha_sathyamurthy@in.ibm.com   
Last modified   2022-07-08T13:56:43.994Z   
                
MODULES

SNO   Workspace Name      Workspace ID                                    Status   Location   Updated   
1     terraform_module1   us-south.workspace.terraform_module1.6cef8e6d   ACTIVE   us-south      
2     terraform_module2   us-south.workspace.terraform_module2.875fda22   ACTIVE   us-south      
      
BLUEPRINT OUTPUTS

Key                Value   
blueprint-output   [{   
                     external = 9900   
                     internal = 9900   
                     protocol = "tcp"   
                     }, {   
                     external = 9901   
                     internal = 9901   
                     protocol = "ldp"   
                   }]   
                      
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
ibmcloud schematics blueprint get -id <blueprint_id> -level modules
```
{: pre}

On successful completion the get command returns a detailed listing of the blueprint template, modules and variables

```text
BLUEPRINT          
                
Name            Blueprint_Complex   
ID              us-south.BLUEPRINT.Blueprint_Complex.5448a1c0   
Status          Normal   
Location        us-south   
Creator         geetha_sathyamurthy@in.ibm.com   
Last modified   2022-07-08T13:56:43.994Z   
                
BLUEPRINT
                   
Name            Blueprint_Complex   
Git Repo URL    https://github.com/Cloud-Schematics/blueprint-complex-inputs   
Git Repo File   complex-blueprint.yaml   
Release         latest   
                
BLUEPRINT INPUTS

Key                     Value   
resource_group          -   
region                  -   
sample_var              -   
boolian_var             -   
list_any_flow_scalar    -   
list_any_block_scalar   -   
docker_ports            -   
                           
BLUEPRINT OUTPUTS

Key                Value   
blueprint-output   [{   
                     external = 9900   
                     internal = 9900   
                     protocol = "tcp"   
                     }, {   
                     external = 9901   
                     internal = 9901   
                     protocol = "ldp"   
                   }]   
                      
INPUT FILE
                   
Git Repo URL    https://github.com/Cloud-Schematics/blueprint-complex-inputs   
Git Repo File   complex-input.yaml   
Git Branch      main   
                
INPUTS

Key                     Value   
resource_group          default   
region                  eu-de   
sample_var              testconfig_input_demo   
list_any_flow_scalar    ["36", "mqm-grand", "madison-circle-garden"]   
list_any_block_scalar   [   
                            "36",    
                            "mqm-grand",    
                            "madison-circle-garden"   
                        ]   
                           
docker_ports            [   
                          {   
                            internal = 9900   
                            external = 9900   
                            protocol = "tcp"   
                          },   
                          {   
                            internal = 9901   
                            external = 9901   
                            protocol = "ldp"   
                          }   
                        ]   
                           
boolian_var             false   
                           
MODULES

terraform_module1      
Git Repo URL        https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/tf-inputs-outputs   
Git Repo Branch     main   
                    
INPUTS

Key                     Value   
TF_VERSION              1.0   
sample_var              $blueprint.sample_var   
image_id                ami-image   
list_any_flow_scalar    $blueprint.list_any_flow_scalar   
list_any_block_scalar   $blueprint.list_any_block_scalar   
docker_ports            $blueprint.docker_ports   
                        
OUTPUTS

Key   
nested_complex   
test_tuple   
   
   
terraform_module2      
Git Repo URL        https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/tf-inputs-outputs   
Git Repo Branch     main   
                    
INPUTS

Key                     Value   
TF_VERSION              1.0   
sample_var              $blueprint.sample_var   
image_id                ami-image   
list_any_flow_scalar    $blueprint.list_any_flow_scalar   
list_any_block_scalar   $blueprint.list_any_block_scalar   
docker_ports            $workitem.terraform_module1.nested_complex   
                        
OUTPUTS

Key   
nested_complex   
test_tuple   
   
   
OK

```
{: screen}

## Displaying a Blueprint through UI 
{: #display-blueprint-ui}
{: ui}

Here are the steps to display a Blueprint by using UI.
{: shortdesc}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the Blueprint details.
2. Click **Overview** to view the BLueprint summary, including `Modules`, `Variables`, `Details` and `Recent Job runs` of your Blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of resource modules and their current status. 
    - Optional: Click on **Show details** to view the module details.
    - Optional: Click on the module name to be taken to the modules' `Workspace` page. 
4. Click **Resource** tab to view the resources provisioned by the Blueprint.
5. Click **Variables** tab to view your **Inputs** and **Outputs** variables and values.
6. Click **Jobs history** tab view the job logs for all Blueprint and module operations.
7. Click **Settings** tab to view the configuration settings for the Blueprint.

## Displaying a Blueprint through API 
{: #display-blueprint-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about configuration of the Blueprint and its outputs, refer to, [Display Blueprint](/apidocs/schematics/schematics#get-blueprint) by using API.

Blueprint display API `blueprint_list` runs the configuration of the Blueprint and detailed output.
{: important}

Displays Blueprint list:

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
            "description": "Deploys dev environtment instance in Toronto Region",
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

This API lists the detailed information with respect to the Blueprint ID.
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
    "blueprint_id": "Blueprint-Basic-Test.eaB.bbb9",
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
{: #bp-create-nextsteps}

After displaying the list of Blueprint in {{site.data.keyword.bpshort}}, the next step in displaying the blueprint jobs is to refer to [list-blueprint-jobs](docs/schematics?topic=schematics-list-blueprint-jobs-cli&interface=api) API in the Blueprint. 

Looking for Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample.
