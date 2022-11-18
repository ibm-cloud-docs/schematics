---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-18"

keywords: schematics blueprints infrastructure, blueprints schema, schema definitions, definitions, yaml

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Understanding blueprint templates and configuration
{: #blueprint-templates}

{{site.data.keyword.bplong}} Blueprints utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout and the major building blocks. 


## Blueprint configuration 
 Cloud environments are deployed from a blueprint configuration, containing a blueprint template and customizable input values. [Templates](/docs/schematics?topic=schematics-glossary#bpb2) determine the infrastructure architecture, specifying the modules (building blocks) to deploy the cloud resources of an environment. The section [using Terraform modules with blueprint templates](/docs/schematics?topic=schematics-blueprint-terraform) illustrates how public modules can be combined with user developed modules to create custom solutions.  
{: shortdesc} 

Templates are reusable across multiple environments, with the customizable input values maintained separately from the template as [inputs](/docs/schematics?topic=schematics-glossary#bpi1). In cookie cutter fashion, several environments can be created from the same blueprint template. Each environment has its own [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3) and inputs. This separation of template from its deploy time configuration is illustrated in the figure. Here a template is reused many times to deploy a range of environments such as `dev`, `stage`, and `production`. Each environment is customized with its own input values, all based on the same template.   

![Environments deployed from templates and inputs](/images/bp-largescale-env.svg){: caption="Environments deployed from templates and inputs" caption-side="bottom"}

A blueprint environment and the cloud resources to be deployed, are defined by three versioned elements:
1. A blueprint template file specifying the resource topology, infrastructure architecture, IaC automation modules and dependencies.
2. Input values to configure and customize the blueprint template at deployment time.
3. Automation modules written in Terraform to deploy the desired cloud resources. 

A blueprint configuration links a template with a set of inputs values to specify the environment and cloud resources. These elements and  relationships are illustrated in the figure.  

![Blueprint configuration, templates and inputs](/images/bp-configuration.svg){: caption="Blueprint configuration, templates and inputs" caption-side="bottom"}

A blueprint configuration defines the blueprint template YAML file to be used and its Git source and version. The input value files, source and version. Plus an additional inputs required by the blueprint. The template file identifies the required Terraform modules. 



## Blueprint template overview
{: #template-overview}

The infrastructure architecture of a solution is defined by a blueprint template: 
- Blueprint templates are written in YAML with a minimum of syntax that specify the automation modules to be used, their versions, source libraries, and relationships for passing resource dependency data between modules. 
- The resource management and provisioning functionality of a blueprint is implemented by automation modules written for the familiar open source Terraform automation tool. 
- Input variable files customize the reusable blueprint templates to create cloud environments.
{: shortdesc}

Input files, blueprint templates, and modules are all maintained in Git source control. A blueprint configuration defines these version controlled components on blueprint creation. The configuration defines the source of the versioned blueprint template in source control and the input files to customize the blueprint. Also additional parameters for naming the blueprint, blueprint access control and additional inputs. 

### Blueprint template YAML file
{: #blueprint-yaml-file}

The text box shows a simplified view of a blueprint template YAML file. It identifies the key elements of inputs and outputs, the choice of modules, and the dependencies and variable linkage between modules. Definitions follow standard YAML syntax. For more details, see the [blueprint template YAML schema](/docs/schematics?topic=schematics-bp-template-schema-yaml) reference.
{: shortdesc} 


```yaml
name: "basic"
description: "Basic blueprint, RG & COS, no API key"
inputs:
  - name: resource_group_name
outputs:
  - name: cos_id
    value: $module.basic-cos-storage.outputs.cos_id
modules:
  - name: basic-resource-group
    module_type: terraform
    source:
      source_type: github
      git: 
        git_repo_url: "https://github.ibm.com/steve-strutt/blueprint-examples-modules/tree/master/IBM-ResourceGroup"
        git_branch: master
    inputs:
      - name: provision
        value: $blueprint.provision_rg
      - name: name
        value: $blueprint.resource_group_name
    outputs:
      - name: resource_group_name
      - name: resource_group_id
  - name: basic-cos-storage
    module_type: terraform
    source:
      source_type: github
      git:
        git_repo_url: "https://github.ibm.com/steve-strutt/blueprint-examples-modules/tree/master/IBM-Storage"
        git_branch: master
        git_release: latest
    inputs:
      - name: cos_instance_name
        value: $blueprint.cos_instance_name
      - name: cos_single_site_loc
        value: "ams03"
      - name: resource_group_id
        value: $module.basic-resource-group.outputs.resource_group_id
    outputs:
      - name: cos_id
      - name: cos_crn
```
{: codeblock}

A blueprint template consists of a two major sections. An global settings section which contains a default name and description for the environment, and related template settings. Also it defines the [inputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs) the template requires and any [outputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-outputs) it generates and returns to the user through {{site.data.keyword.bpshort}}. This is followed by a list of `modules` containing the [module definitions](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-schema). 

Dependencies are created between modules by interpolation of module input and output values. In the `basic-cos-storage` module definition above, the input `resource_group_id` specifies the interpolated value `$module.basic-resource-group.outputs.resource_group_id` which creates a dependency on the `resource_group_id` output value from the module `basic-resource-group`.

### Input statements
{: #blueprint-input-statements}

All variables required by a blueprint template must be defined in the [inputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs) section. Values can be statically defined in the template. If the value is omitted, it is assumed that the input is satisfied by a user defined input value from the blueprint configuration.
{: shortdesc}  

```yaml
inputs:
  - name: resource_group_name
  - name: region
    value: us-south
```
{: codeblock}

### Module statements
{: #blueprint-module}

Templates support the use of both Terraform [root modules](https://developer.hashicorp.com/terraform/language/modules#the-root-module){: external} and Terraform [child](https://developer.hashicorp.com/terraform/language/modules#child-modules){: external} modules. See the section [Using Terraform modules with blueprint templates](/docs/schematics?topic=schematics-blueprint-terraform). 

[Module statements](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-schema) define the modules utilized by the template. Each statement defines the source repository for the automation modules, the inputs and outputs for passing of resource information between dependent modules. Typically they contain 3 statement blocks:
1. source
2. inputs
3. outputs
{: shortdesc} 

Terraform root modules (configs and templates) with provider blocks can be used directly with the parameters described above. Terraform child modules require the addition of an [injector block](/docs/schematics?topic=schematics-blueprint-terraform#bp-provider-injection) to specify an IBM Cloud provider block. 

#### Module Source
{: #blueprint-module-source}

Blueprint modules are sourced from version controlled Git source repositories. Following IaC principles, versioned modules can be explicitly selected by release tags, branch, or by a relaxed latest commit approach. See [source](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-sourceoptions) schema reference.
{: shortdesc} 


```yaml
    source_type: github
    git:      
      git_repo_url: "https://github.ibm.com/steve-strutt/blueprint-examples-modules/tree/master/IBM-Storage"
      git_release: 1.0.1
```
{: codeblock}

#### Module inputs
{: #blueprint-module-inputs}

Module inputs are defined in the input block of a module and follow the same type convention as Terraform HCL. The supported types are the same as the [Terraform variable types](https://developer.hashicorp.com/terraform/language/expressions/types). If the type is omitted the default is `string`. For an example showing data types represented in YAML, see [blueprint complex inputs](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} and the code snippet below.
{: shortdesc} 

```yaml
inputs: 
  - name: list_any_flow_scalar
    value: $blueprint.list_any_flow_scalar
    type: list(any)
    secure: false
  - name: docker_ports
    value: $module.terraform_module1.outputs.nested_complex
    type: |
          list(object({
             internal = number
             external = number
             protocol = string
           })
    secure: false
```
{: codeblock}

Four keywords define the variable attributes: `name`, `value`, `type`, and `secure`. These correspond with the [Terraform variable definitions](https://developer.hashicorp.com/terraform/language/values/variables#arguments){: external}

Similar to Terraform, module inputs can be specified as static values or interpolated references to template inputs or other modules by using the `$` symbol as shown in the sample snippet. 

Two input variable reference types are [supported](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-inputs-value). Inter-module references, mapping module inputs to the outputs of other modules using the `$module` token. Additionally, templates input references can be defined using the `$blueprint` token. Check the readme and metadata for the [automation modules](https://github.com/terraform-ibm-modules){: external} to determine the supported inputs and variable types. 

To reference the output value of another module, the format is `$module` followed by the dependent module name, the token `outputs`, followed by referenced output name.

```yaml
name: res_grp_id            
value: $module.accounts.outputs.res_grp_id
```
{: codeblock}

To reference an input value from the template inputs, the name of the input variable must be defined in the template inputs block. The reference takes the format `$blueprint` followed by the referenced input variable name.
{: shortdesc} 

```yaml
inputs:
    -name: region


modules:
  - name: account 
inputs:
  - name: logdna_sts_region
    value: $blueprint.region
```
{: codeblock}

#### Module outputs
{: #blueprint-module-outputs}

{{site.data.keyword.cloud_notm}} automation modules implemented in Terraform must contain HCL output statements to pass data and resource information to dependent modules. Check the readme and metadata for the [module](https://github.com/terraform-ibm-modules){: external} to determine the supported outputs and data types.
{: shortdesc} 

```yaml
outputs:
 - name: resource_group_name
 - name: resource_group_id

```
{: codeblock}


### Template outputs
{: #blueprint-output}

Values to be returned from a template are defined by [output statements](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-outputs) and use the same interpolation syntax as module inputs.
{: shortdesc}  

```yaml
outputs:
  - name: cos_id
    value: $module.basic-cos-storage.outputs.cos_id
```
{: codeblock}

### Template inputs
{: #template-inputs}

The inputs required by a template are defined in the [inputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs) section. Where no value is specified in the input section, a template accepts input values from two sources specified by the blueprint configuration at create time:
{: shortdesc} 

- Version controlled input variable YAML files
- *Dynmamic* input variables, which are not version controlled 

## Input files
{: #blueprint-input-file}

Input files define the version controlled input values used for template customization. The variable type must match the module input type in the template template. For more information on creating and using input files, see the [input file schema reference](/docs/schematics?topic=schematics-bp-input-schema-yaml) and [create a blueprint configuration](/docs/schematics?topic=schematics-create-blueprint-config).
{: shortdesc} 

```yaml
resource_group: default
region: us-south
docker_ports: | 
  [
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
```
{: codeblock}

## Dynamic inputs
{: #blueprint-dynamic-input}

Dynamic inputs are set at [blueprint config create](/docs/schematics?topic=schematics-create-blueprint-config) time to pass inputs to dynamically customize the template and over ride inputs from an a version controlled input file sourced from a Git repo. They can be used to pass input values that would be a security exposure if written to a Git repository. API keys and SSH keys would typically be passed as dynamic inputs at config create time. See the [blueprint FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-secure-inputs) for using dynamic inputs to pass sensitive variables. 
{: shortdesc} 

Blueprint templates only support input variables of type `string` as dynamic inputs. 

Example of passing dynamic inputs at creation time. 

```sh
ibmcloud schematics blueprint config create -name <name> -resource_group <resource_group> -bp_git_url <blueprint_url> -input_git_url <input_url> -inputs provision_rg=<value>,resource_group_name=<value>
```
{: pre} 

## What's next
{: #bp-def-whatsnext}

In this section you have learned about {{site.data.keyword.bpshort}} templates and configuration. Now you can: 
- Explore [deploying {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-(/docs/schematics?topic=schematics-destroy-blueprint).

- Refer to, [blueprint template YAML](/docs/schematics?topic=schematics-bp-template-schema-yaml) and [blueprint input YAML](/docs/schematics?topic=schematics-bp-input-schema-yaml) for more information about the parameters used in the YAML files.

