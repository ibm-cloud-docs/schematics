---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-14"

keywords: schematics blueprints template, blueprints yaml, schema definitions, definitions, yaml,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Blueprint template YAML schema
{: #bp-template-schema-yaml}

This document is the reference of the YAML schema that is used to describe the blueprint template YAML file.

Blueprint templates are written in YAML with a minimum of syntax that specify the automation modules to be used, their versions, source libraries, and relationships for passing resource dependency data between modules. 
{: shortdesc}

A blueprint template consists of two major sections. A `global settings` section contains a default name and description for the environment, and settings related to the whole template. Also it defines the inputs the template requires, and any outputs it returns. This is followed by a `modules` section containing the module definitions. Dependencies are created between modules by interpolation of module input and output values.

Each template file has a global settings preface:

```yaml

name: dev-blueprint
type: "blueprint"
schema_version: "1.0"
description: "Project to provision Application Service."
tags:
  - "blueprint:dev"
inputs:
  - name: resource_group
  - name: region
  - name: api_key
outputs:
  - name: service-url
    value: $module.bp-vsi-vpc-app.outputs.app_url
settings: 
  - name: TF_VERSION
    value: 1.0
```
{: pre}

## Global settings
{: #bp-parameters}

Following are the supporting setting parameters that can be used to configure a template

### name
{: #bp-name}

Type:       string

Required:   true

Name that is used to identify the blueprint template in use 

### type
{: #bp-type}

Type:       string

Required:   true

Required file type identifier 

Value: `blueprint`

### schema_version
{: #bp-schema-version}

Type: number

Required: true

Schema version used by this file. 

Options: 1.0

Example 

```yaml
schema_version: "1.0"
```
{: pre}


### description
{: #bp-description}

Type: string

Default: []

A string used to describe the template to provide users with more information about its usage and the solution it describes.

Example 
```yaml
description: "Project to provision Application Service."
```
{: pre}

### tags
{: #bp-tags}

Type: list

Default: []

A list of tags to be attached to all deployed cloud resources and the blueprint resource. All environments deployed using this template will have these tags. Additional tags can be specified at config create time specific to the environment to be deployed.   

Example 
```yaml
 tags:
  - "blueprint:dev"
```
{: pre}

tags:
  - "blueprint:dev"


### inputs
{: #bp-inputs}

Type: list

Default: []

A list defines all the inputs that are required by the template. Inputs are specified in the [`input file`](/docs/schematics?topic=schematics-bp-input-schema-yaml) file or as input values when the blueprint is created in {{site.data.keyword.bpshort}}. Inputs are defined with a `name:` key. 

Example

```yaml
inputs:
    - name: resource_group
    - name: region
    - name: api_key
```
{: pre}


### inputs.type
{: #bp-inputs-type}

Type: YAML flow or block scalar 

Default: string 

As templates work with Terraform configs and modules, Terraform variable [type constraints](https://developer.hashicorp.com/terraform/language/expressions/type-constraints#type-constraints){: external} are used to set the type validation for blueprints inputs. 

Complex Terraform types are typically represented as multi-line strings. They can be similarly rendered in YAML block syntax, using either the literal style is denoted by the “|” indicator, or the folded style is denoted by the “>” indicator.  

Options: Any valid Terraform variable type

Example
```yaml
- name: docker_ports
  type: |
    list(object({
    internal = number
    external = number
    protocol = string
  })

```
{: pre}


### inputs.value
{: #bp-inputs-value}

Type: Any valid Terraform variable type

Optional

The value keyword is only used where it is desired to define a static value to be used by the template. It is equivalent to specifying a local variable. When not specified the value is sourced from the blueprint config inputs and input file at run time.  

Example of statically defined local value 
```yaml
- name: provision_ats_instance
  type: boolean            
  value: false
```
{: pre}


### inputs.default
{: #bp-inputs-default}

Type: Any valid Terraform variable type

Optional

A default value for the input. The default is used when no value is provided at run time from the blueprint configuration inputs and input file. The default type is as defined by the `inputs.type` option. 

Example of a default value 
```yaml
- name: provision_ats_instance
  type: boolean            
  value: false
```
{: pre}


### inputs.sensitive
{: #bp-modules-inputs-secure}

Type: Boolean

Default: false

Flag specifying whether the value is a sensitive variable and must be masked in the output
{: pre}

### inputs.max_length
{: #bp-inputs-max-len}

Type: Number

Number specifying the maximum length of the input value. Attribute is used by the UI to perform validation.  
{: pre}

### inputs.min_length
{: #bp-inputs-min-len}

Type: Number

Number specifying the minimum length of the input value. Attribute is used by the UI to perform validation.  
{: pre}


### outputs
{: #bp-outputs}

Type: list

Default: [] 
 
A list defines all the outputs that are returned by the template to the user. Each output is identified by a key-value pair. 

Example

```yaml
outputs:
    - name: schematics-service-url
      value: $module.bp-vsi-vpc-app.outputs.app_url
```
{: pre}

### settings
{: #bp-settings}

Type: list

Default: []

A list of the global environment-variables (env-vars) to be made available in the module execution environment at run time.  They are defined as key-value pairs. Two common env-vars are listed here. More env-vars can be found in the [{{site.data.keyword.bpshort}} docs](https://cloud.ibm.com/docs/schematics?topic=schematics-set-parallelism). 
{: pre}


#### settings.TF_VERSION
{: #bp-tf-version}

Type:       number

Blueprint templates set the Terraform version to be used at module execution time based on the value of TF_Version. This value can be used to pin the version of Terraform used by {{site.data.keyword.bpshort}} to remain compatible with the template supported version. Updating this value will change the Terraform version that is used on the next execution. 

Options:    Terraform version in `SemVer` format 

Example

```yaml
settings: # Master settings for all modules 
  - name: TF_VERSION
    value: 1.0 
```
{: pre}

#### settings.TF_LOGS
{: #-logs}

Type:       string

Configure the Terraform logging level for all modules. See [Debugging Terraform](https://developer.hashicorp.com/terraform/internals/debugging){: external} for detailed usage. 

Example

```yaml
settings: # Master settings for all modules 
  - name: TF_LOGS
    value: "debug"
```
{: pre}



## Module parameters
{: #bp-modules-schema}

The `modules` block defines the Terraform, and Ansible modules from which the solution is constructed. 

Each module list entry is defined by a name, followed by the source for the module, inputs, and expected outputs. 

Review the automation module metadata and readme file information for the [modules](https://github.com/terraform-ibm-modules){: external} to guide defining the module block.     

```yaml
modules:
  - name: bp-vsi-resource-group
    module_type: terraform 
    source:
      source_type: github
      git: 
        git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
        git_branch: main
    inputs:
      - name: provision
        value: $blueprint.provision_rg
      - name: name
        value: $blueprint.resource_group_name
    outputs:
      - name: resource_group_name
      - name: resource_group_id
  - name: bp-vsi-cos-storage
    module_type: terraform 
    source:
      source_type: github
      git:
        git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-Storage"
        git_branch: main
    inputs:
      - name: cos_instance_name
        value: $blueprint.cos_instance_name
      - name: cos_single_site_loc
        value: "ams03"
      - name: resource_group_id
        value: $module.bp-vsi-resource-group.outputs.resource_group_id
    outputs:
      - name: cos_id
      - name: cos_crn
```
{: pre}

### modules.name
{: #bp-modules-name}

Type: string

Required: true

The name that is used within {{site.data.keyword.bpshort}} to identify the blueprint module created to manage the resources  created by the Terraform config or module specified on the `modules.source.git.git_url` statement. 

### modules.module_type
{: #bp-modules-moduletype}

Type: string

Required: true

String specifying the IaC type of the automation module. Only Terraform is supported at this time.   

Options: `terraform`
{: pre}

### modules.source options
{: #bp-modules-sourceoptions}

The version control source library where the Terraform config, or Ansible playbook are located. 

```yaml
source:
  source_type: github
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
```
{: pre}

### modules.source.source_type
{: #bp-modules-source-type}

Type: string

Required: true

Type of Git source repository. 

Options: `github`, `git`, `catalog` 

Specifies the module source from a version control system, e.g. a Github or Gitlab repository, or IBM Cloud Catalog. The following source parameters must match the source_type. 
{: pre}

### modules.source.git.git_repo_url
{: #bp-modules-git-repo-url}

Type: url string

Required: true

URL for the automation module in its content repository. If the config exists in a sub directory the folder name is appended. 
{: pre}

### modules.source.git.git_branch
{: #bp-modules-git-branch}

Type: string

Default: main

If content is in Git, the branch containing the version of the Terraform config or module to be used. This option is mutually exclusive with the `git_release` option. 
{: pre}

### modules.source.git.git_release
{: #bp-modules-git-release}

Type: string

Default: latest

If content type is `git`, the release tag of the version of the Terraform config or module to be used. This option is mutually exclusive with the `git_branch` option. If not specified, {{site.data.keyword.bpshort}} will default to always pulling the latest commit during a `blueprint config update` operation. 

Options: `latest` or release in SemVer format 
{: pre}


### modules.source.git.git_token
{: #bp-modules-git-token}

Type: string

Default: []

Access templates in private Git repos by specifying the Git user token of the repo. The value for `git_token` can be sourced from the template inputs using the `$blueprint` token.  

Example

```yaml
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
    git_token: $blueprint.git_token
```
{: pre}



### modules.settings
{: #bp-settings}

Type: list

Default: []

A list of the environment-variables (env-vars) to be made available in the module execution environment at run time.  They are defined as key-value pairs. Two common env-vars are listed here. More env-vars can be found in the [{{site.data.keyword.bpshort}} docs](https://cloud.ibm.com/docs/schematics?topic=schematics-set-parallelism). 

{: pre}


#### modules.settings.TF_VERSION
{: #bp-tf-version}

Type:       number

The Terraform version to be used at module execution time can be set using TF_Version parameter. This value can be used to pin the version of Terraform used by {{site.data.keyword.bpshort}} to remain compatible with the module supported version. Updating this value will change the Terraform version that is used on the next execution. 

Options:    Terraform version in `SemVer` format 

Example

```yaml
module:
  -name: xxxxx
    settings: # Master settings for all modules 
      - name: TF_VERSION
        value: 1.0 
```
{: pre}

#### modules.settings.TF_LOGS
{: #-logs}

Type:       string

Configure the Terraform logging level during module execution. See [Debugging Terraform](https://developer.hashicorp.com/terraform/internals/debugging){: external} for detailed usage. 

Example

```yaml
module:
  -name: xxxxx
    settings: # Master settings for all modules 
      - name: TF_LOGS
        value: "debug"
```
{: pre}


### modules.inputs options
{: #bp-modules-inputs-options}

Type: list

Default: []

A list that defines all the inputs that are required by the module. 

```yaml
inputs:
  - name: list_any_flow_scalar
    value: $blueprint.list_any_flow_scalar
    type: list(any)
  - name: cos_single_site_loc
    value: "ams03"
  - name: resource_group_id
    value: $module.bp-vsi-resource-group.outputs.resource_group_id
  - name: docker_ports
    value: $blueprint.docker_ports
```
{: pre}

### modules.inputs.name
{: #bp-modules-inputs-name}

Type: string

Required: true

Name of variable to be passed to Terraform module or Ansible playbook. It must match the value in the Terraform template for the value to be passed at execution time to the module. 
{: pre}

The module input type constraint must match the variable type in the Terraform config for the value to be passed successfully at execution time. The module input type is inherited from the blueprint inputs type. 


### modules.inputs.value
{: #bp-modules-inputs-value}

Type: Any valid Terraform variable type

Required: true

The value field sources the input value for a module from three sources:
- Statically defined values specified on the name value pair statement of the module, in YAML syntax
- An input to the template defined in the settings prefix and sourced at run time from the [inputs](/docs/schematics?topic=schematics-glossary#bpi1) defined by the blueprint configuration. Identified by the `$blueprint` prefix
- An output value from another module defined in the template file. 
    - Identified by the `$module` prefix and must be included in the output section of another module.
    - The format is the token, `$module` followed by the module name, the token `outputs`, followed by the module output name. 
    - In the following style: `$module.<module_name>.outputs.<output_name>`

The value type is as defined by the `modules.inputs.type` option. 

Functions and operators on input values are not supported in blueprint schema 1.0.0. This is being considered for a future release. Values are passed as is from the source to the module input without manipulation.      

Example of statically defined values 
```yaml
- name: provision_ats_instance
  type: boolean            
  value: false
```
{: pre}

Example value sourced from an input statement in the template `inputs` section
```yaml
- name: logdna_sts_region
  type: string
  value: $blueprint.region
```
{: pre}

Example value sourced from an output statement on the module `IBM-Resource-Group`. 
```yaml
- name: resource_group_name            
  value: $module.IBM-Resource-Group.outputs.resource_group_name
```
{: pre}

### modules.inputs.sensitive
{: #bp-modules-inputs-secure}

Type: Boolean

Default: false

Flag specifying whether the value is a sensitive variable and must be masked in the output
{: pre}

### module.outputs
{: #bp-module-outputs}

Type:       list

Default:    []  

A list defines all the outputs that are returned by the module to be used as inputs to other modules or output from the blueprint. Each output is identified by the label `name`.  It must match an [output declaration](https://developer.hashicorp.com/terraform/language/values/outputs){: external} in the Terraform template for the value to be retrieved at execution time from the module. The name can be copied from the [module metadata](https://github.com/terraform-ibm-modules){: external} or from inspecting the Terraform `.tf` files. 

Example
```yaml
outputs:
    - name: log_analysis_instance_name
    - name: sysdig_instance_name
```
{: pre}

### module.injectors options
{: #bp-modules-injector}

Type:         list 

Default:      []

The injectors block is an optional block to configure the parameters that are required by {{site.data.keyword.bpshort}} to inject additional files into the automation module source at execution time. The primary use with blueprint templates is to enable direct use of Terraform child modules, by the injection of `.tf` files containing `provider` and `terraform` blocks. Review section [blueprints provider injection](/docs/schematics?topic=schematics-blueprint-terraform#bp-provider-injection) for guidance on the use of the injectors block. 

```yaml
injectors:
  - tft_git_url: "https://github.com/Cloud-Schematics/tf-templates"
    tft_name: "ibm"
    injection_type: override
    tft_parameters:
    - name: provider_version
      value: 1.42.3
    - name: provider_source
      value: IBM-Cloud/ibm
    - name: region
      value: us-south
```
{: pre}

### module.injectors.tft_git_url
{: #bp-module-tft-git-url}

Type: URL

Required: true

URL of the Git repository that contain the templating files used for injection. 
{: pre}

### module.injectors.tft_name
{: #bp-module-tft-name}

Type: string

Required: true

Name of the templating file to use

Options: `ibm` or `kubernetes`
{: pre}

### module.injectors.injection_type
{: #bp-module-injection-type}

Type: string

Required: true

Two modes of injection are supported by Terraform. Definitions can be injected as extra files, only if it is believed that no conflict with any existing HCL statements. Alternatively they can be injected as [HCL override files](https://developer.hashicorp.com/terraform/language/files/override){: external}.

Options: `override` or `inject`
{: pre}

### module.injectors.tft_parameters
{: #bp-modules-tft-parameters}

Type: list

Required: true

A list that defines the templating inputs as name-value pairs. At this time, only static values are supported.  
 
Example
```yaml
- name: provider_version
  value: 1.42.3
- name: provider_source
  value: IBM-Cloud/ibm
- name: region
  value: us-south
```
{: pre}
