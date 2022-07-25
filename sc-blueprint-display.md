---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-25"

keywords: blueprint get, blueprint list, blueprint, get, list,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Listing Blueprints
{: #list-blueprint-cli}
{: cli}

To list your Blueprints with the CLI, use the `ibmcloud schematics blueprint list` command. This command takes requires no arguments, but is region specific and will only list Blueprints in the selected CLI region. 
{: shortdesc}

**Syntax:**

```sh
ibmcloud schematics blueprint list
```
{: pre}

On successful completion the list command returns a list of Blueprints  

### Blueprint list output
{: #list-blueprint-output-cli} 

The command lists all the Blueprints created in the CLI region. 

```text
Name                        ID                                                   Status   Location   Creator                   Last modified   
Blueprint Basic Example     eu-gb.BLUEPRINT.Blueprint-Basic-Example.f612085f     Normal   eu-gb      steve_strutt@uk.ibm.com   2022-07-03T14:09:02.354Z   
blueprints-complex-inputs   eu-gb.BLUEPRINT.blueprints-complex-inputs.7312c775   Normal   eu-gb      steve_strutt@uk.ibm.com   2022-07-04T13:01:50.313Z   
                                                                                          
OK
```
{: screen}


## Displaying Blueprints
{: #display-blueprint-cli}
{: cli}

To display the details of Blueprints and their configuration with the CLI, use the `ibmcloud schematics blueprint get` command. Four levels of detail are supported with the `--level` option. 
- `summary` Blueprint and module status
- `detailed` Blueprint configuration, settings and source URLs. 
- `modules` Detailed listing of the Blueprint definition, modules and variables
- `outputs` Output variables returned by Blueprint on deploying cloud resources.

For a complete listing of options, see the [ibmcloud schematics blueprint get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-get) command.
{: shortdesc}


### Blueprint display summary 
{: #display-blueprint-summary-cli}

**Syntax:**

```sh
ibmcloud schematics blueprint get -id <blueprint_id>
```
{: pre}

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

**Syntax:**

```sh
ibmcloud schematics blueprint get -id <blueprint_id> -level outputs
```
{: pre}

On successful completion the get command returns the summary details of the Blueprint and the output variables returned by the Blueprint.  

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

**Syntax:**

```sh
ibmcloud schematics blueprint get -id <blueprint_id> -level modules
```
{: pre}

On successful completion the get command returns a detailed listing of the Blueprint definition, modules and variables

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



## Displaying a Blueprint from the UI 
{: #display-blueprint-ui}
{: ui}


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

