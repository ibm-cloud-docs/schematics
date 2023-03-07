---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics blueprints template, blueprints yaml, schema definitions, definitions, yaml,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Blueprint template YAML schema
{: #bp-template-schema-yaml}

This document is the reference of the YAML schema that is used to describe the blueprint template YAML file.

Blueprint templates are written in YAML with a minimum of syntax that specify the automation modules to be used, their versions, source libraries, and relationships for passing resource dependency data between modules. Template editing and schema validation is supported in [VSCode with the YAML language extension](/docs/schematics?topic=schematics-edit-blueprints). 
{: shortdesc}

A blueprint template consists of two sections. A global section contains a default name and description for the environment, and settings related to the whole template. Also it defines the inputs the template requires, and any outputs it returns. This is followed by a modules section containing the module definitions. Resource dependencies are created between modules by interpolation of module input and output values.

Refer to the section [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) for an overview of the template structure. 


Each template file has a global preface:

```yaml
name: dev-blueprint
type: "blueprint"
schema_version: "1.0"
description: "Project to provision Application Service."
tags:
  - "blueprint:trial_dev"
inputs:
  - name: resource_group
    type: string
  - name: region
    type: string
  - name: api_key
    type: string
outputs:
  - name: service-url
    value: $module.bp-vsi-vpc-app.outputs.app_url
settings: 
  - name: TF_VERSION
    value: 1.0
```
{: pre}

## Global preface
{: #bp-parameters}

Following are the supporting parameters that can be used to configure a template

### name
{: #bp-name}

Type:       string

Required:   true

An identifier for this blueprint template. The name given to a blueprint environment is defined at create time.  

### type
{: #bp-type}

Type:       string

Required:   true

Required blueprint template file type identifier.  

Value: `blueprint`

### schema_version
{: #bp-schema-version}

Type: number

Required: true

Blueprint schema version used by this template definition. Only 1.0 is in effect at this time.  

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

A string used to describe the template to provide users with more information about its usage and the solution it describes. This is internal to the template. The blueprint environment description is defined at create time.  

Example 
```yaml
description: "Project to provision Application Service."
```
{: pre}

### tags
{: #bp-tags}

Type: list

Default: []

An optional list of user tags to be attached to all deployed cloud resources, workspaces and the blueprint resource created using this template. These tags are additional to any tags defined at the module level or on the Terraform IaC resource definitions. All environments deployed using this template will have these tags. Environment specific tags can be specified at config create time, to uniquely identify the instance of environment deployed, e.g dev, stage, prod. 

An immutable service tag with the `blueprint_id` for an environment is attached to all deployed resources and can be used to identify all resources associated with the environment.      

Example 
```yaml
 tags:
  - "blueprint:trial_dev"
```
{: pre}


### inputs
{: #bp-inputs}

Type: list

Default: []

A list defining all the inputs that are required to configure the template. Inputs are specified in the [`input file`](/docs/schematics?topic=schematics-bp-input-schema-yaml) file or as dynamic input values when the blueprint is created. Inputs are defined with a `name` key. To understand how Blueprints determines the value for a template input, review the section on [input precedence](/docs/schematics?topic=schematics-blueprint-reuse#blueprint-input-precedence).

Example

```yaml
inputs:
    - name: resource_group
      type: string
    - name: region
      type: string
    - name: api_key
      type: string
```
{: pre}


#### inputs.type
{: #bp-inputs-type}

Type: YAML flow or block scalar 

Default: string 

As templates work with Terraform configs and modules, Terraform variable [type constraints](https://developer.hashicorp.com/terraform/language/expressions/type-constraints#type-constraints){: external} are used to set the type validation for blueprints inputs. 

Type definitions are required to set the variable type of inputs passed to modules at runtime.     

Complex Terraform types are typically represented as multi-line strings, as they are in HCL. They can be similarly rendered in YAML block syntax, using either the literal style is denoted by the “|” indicator, or the folded style is denoted by the “>” indicator. Refer to the section [defining complex inputs](/docs/schematics?topic=schematics-bp-input-schema-yaml#complex-input-value) for detail on the YAML block syntax. 

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


#### inputs.value
{: #bp-inputs-value}

Type: Any valid Terraform variable type

Optional - mutually exclusive with the default key

The value keyword is only used where it is desired to define a static value to be used by the template. It is equivalent to specifying a local variable. When no value keyword is specified, the value is sourced from the blueprint config inputs and input file at run time. Review the section on [input precedence](/docs/schematics?topic=schematics-blueprint-reuse#blueprint-input-precedence) to understand input processing and sourcing of inputs.

Example of a statically defined local value 
```yaml
    - name: provision_ats_instance
      type: boolean            
      value: false
```
{: pre}


#### inputs.default
{: #bp-inputs-default}

Type: Any valid Terraform variable type

Optional - mutually exclusive with the value key

A default value for the input. The default is used when no value is provided at run time from the blueprint configuration input file or dynamic (override) inputs. The default type is as defined by the `inputs.type` option. 

Example of a default value 
```yaml
    - name: place_instance
      type: list(any)         
      default: |
      [
      "36", 
      "mqm-grand", 
      "madison-square-gardens"
      ]
```
{: pre}


#### inputs.sensitive
{: #bp-inputs-secure}

Type: Boolean

Default: false

Flag specifying whether the value is a sensitive variable and must be masked in any displayed output via the UI or CLI. The sensitive attribute for an input can only be set via the template. It cannot optionally be set at create time.  
{: pre}

Example

```yaml
    - name: ssh_key
      sensitive: true            
```
{: pre}


#### inputs.max_length
{: #bp-inputs-max-len}

Type: Number

Number specifying the maximum length of the input value. This attribute is used by the CLI and UI to perform validation and to signify the expected length of the value.  
{: pre}

If `max_length` is not specified, the default maximum length of a variable value allowed by Blueprints is 1000 bytes. A value larger than 1000 bytes, or the specified length will result in the error `Length for variable <variable name> greater than the given length`   

Example

```yaml
    - name: json_override
      max_length: 10000            
```
{: pre}



#### inputs.min_length
{: #bp-inputs-min-len}

Type: Number

Optional

Number specifying the minimum length of the input value. Attribute is used by the UI to perform input validation. 
{: pre}


### outputs
{: #bp-outputs}

Type: list

Default: [] 
 
Optional

A list defining all the outputs that are returned by the template to the user. Each output is identified by a key-value pair. 

Refer to the section [modules inputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-inputs-value) for details on defining the `$module` interpolation to pass output values.  

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

Optional

Environment variables are used to modify the behavior and execution of Terraform and Ansible without modifying the IaC code itself. Refer to the section [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information about the environment variables that can be passed to configure Terraform runtime behavior in Schematics. 

A list of the global environment variables (env-vars) to be made available in the module execution environment at run time. They are defined as key-value pairs. Two common env-vars are listed here. 
{: pre}

Settings defined at this level apply to all modules. Additionally env-var settings can also be set at the module level. 


#### settings.TF_VERSION
{: #bp-tf-version}

Type:       number

Optional

Blueprint templates set the Terraform version to be used at module execution time based on the value of TF_Version. This value can be used to pin the version of Terraform used by {{site.data.keyword.bpshort}} to remain compatible with the template supported version. Updating this value will change the Terraform version that is used on the next execution. 

When not specified the required Terraform version is determined by inspecting the TF module code for a `required_version` definition. Based on the specified version or range of versions, Blueprints will used the most recent version of Terraform supported by Schematics. 

Options:    Terraform version in `SemVer` format 

Example

```yaml
settings: # Master settings for all modules 
  - name: TF_VERSION
    value: 1.0 
```
{: pre}

#### settings.TF_LOGS
{: #bp-logs}

Type:       string

Optional 

Configure the Terraform logging level for all modules. See [Debugging Terraform](https://developer.hashicorp.com/terraform/internals/debugging){: external} for detailed usage and [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism)

Example

```yaml
settings: # Master settings for all modules 
  - name: TF_LOGS
    value: "DEBUG"
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
        type: string
        value: "ams03"
      - name: resource_group_id
        type: string
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

The name that is used within {{site.data.keyword.bpshort}} to identify the blueprint module created to manage the resources created by the Terraform config or module specified on the `modules.source.git.git_url` statement. This name is used as the module identifier in all interpolation references, UI and CLI outputs. 

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

Options: `git`, `catalog` 

Specifies the module source from a version control system, for example, a Github or Gitlab repository, or {{site.data.keyword.cloud_notm}} Catalog. The following source parameters must match the source_type. 
{: pre}

### modules.source.git.git_repo_url
{: #bp-modules-git-repo-url}

Type: url string

Required: true

URL for the Terraform module or config in its content repository. This is the full path to the module, in the root folder of the repo or the path to a module in sub-folder in the repo. Multiple modules/configs can exist in sub-folders of the repo. 
`"https://github.com/Cloud-Schematics/blueprint-example-modules/IBM-ResourceGroup"`

Blueprints supports using the Git object path syntax to specifying the URL to your module, using the `tree/<branch>` syntax, including the branch and sub-folders. 
`https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup`

Example

```yaml
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
```
{: pre}

### modules.source.git.git_branch
{: #bp-modules-git-branch}

Type: string

Default: main

If content is in Git, the branch containing the revision of the Terraform config or module to be used. This option is mutually exclusive with the `git_release` option. It can be omitted if the branch is specified via the `git_repo_url`. 
{: pre}

### modules.source.git.git_release
{: #bp-modules-git-release}

Type: string

Default: `implicitly latest`

If content type is `git`, the release tag of the version of the Terraform config or module to be used. This option is mutually exclusive with the `git_branch` option. If not specified, {{site.data.keyword.bpshort}} will default to always pulling the latest commit during a `blueprint update` operation. 

Value:  Release in SemVer format 
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
{: #bp-modules-settings}

Type: list

Default: []

Optional

Environment variables are used to modify the behavior and execution of Terraform and Ansible without modifying the IaC code itself. Refer to the section [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information about the environment variables that can be passed to configure Terraform runtime behavior in Schematics. 

A list of the global environment variables (env-vars) to be made available in the module execution environment at run time. They are defined as key-value pairs. Two common env-vars are listed here. 
{: pre}

#### modules.settings.TF_VERSION
{: #bp-modules-tf-version}

Type:       number

Optional

Blueprint templates set the Terraform version to be used at module execution time based on the value of TF_Version. This value can be used to pin the version of Terraform used by {{site.data.keyword.bpshort}} to remain compatible with the template supported version. Updating this value will change the Terraform version that is used on the next execution. 

When not specified the required Terraform version is determined by inspecting the TF module code for a `required_version` definition. Based on the specified version or range of versions, Blueprints will used the most recent version of Terraform supported by Schematics. 

Value:    Terraform version in `SemVer` format 

Example

```yaml
    settings: # Master settings for all modules 
      - name: TF_VERSION
        value: 1.0 
```
{: pre}

#### modules.settings.TF_LOGS
{: #bp-modules-logs}

Type:       string

Optional 

Configure the Terraform logging level for this module. See [Debugging Terraform](https://developer.hashicorp.com/terraform/internals/debugging){: external} for detailed usage and [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism)

Example

```yaml
    settings: # Master settings for all modules 
      - name: TF_LOGS
        value: "DEBUG"
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
  - name: cos_single_site_loc
    type: string
    value: "ams03"
  - name: resource_group_id
    type: string
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

The module input type constraint must match the variable type in the Terraform config for the value to be passed successfully at execution time. 

### modules.inputs.type
{: #module-inputs-type}

The value type is as determined by the `modules.inputs.type` option. Refer to the [inputs.type](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs-type) section for details of how to specify input types. 

The type definition is only required for statically defined module input values or `$module` references. The type definition is ignored for values sourced from the template input section, as these already have a type definition on the template input.  

Example type definition for an input interpolated from the module IBM-Resource-Group using `$module`. 
```yaml
    - name: docker_ports
      type: |
        list(object({
        internal = number
        external = number
        protocol = string
        })
      value:  $module.App-deploy.outputs.docker_ports  
```
{: pre}

### modules.inputs.value
{: #bp-modules-inputs-value}

Type: Any valid Terraform variable type

Required: true

The value field sources the input value for a module from one of three sources:
- Statically defined values specified on the name value pair statement of the module, in YAML syntax
- An input to the template defined in the global preface section and sourced at run time from the inputs defined in the blueprint configuration. 
- An output value from another module defined in the template file. 

Functions and operators on input values are not supported in blueprint schema 1.0.0. This is being considered for a future release. Values are passed as is from the source to the module input without manipulation.

The value type is as determined by the `modules.inputs.type` option. 

#### Statically defined values 
{: #stat-value}

Type definition IS required to set the type passed to Terraform. 

Example of statically defined values 
```yaml
    - name: provision_ats_instance
      type: boolean            
      value: false
```
{: pre}

#### Template input
{: #mod-template-input}

Identified by the `$blueprint` prefix. Appended with the template input name. 

Type definition is NOT required to set the type passed to Terraform. 

Example value sourced from an input statement in the template `inputs` section
```yaml
    - name: logdna_sts_region
      value: $blueprint.region
```
{: pre}

#### Module reference
{: #mod-reference}

A resource output value from another module defined in the template file. Required to pass resource dependency information between modules.   

- Identified by the `$module` prefix. 
- The format is the token, `$module` followed by the name of the module sourcing the output value, the token `outputs`, followed by the source module output name. 
- In the following style: `$module.<module_name>.outputs.<output_name>`
   
Type definition is required to set the type passed to Terraform.    

Example value sourced from an output `resource_group_name` on the module `IBM-Resource-Group`.

```yaml
- name: resource_group_name
  type: string            
  value: $module.IBM-Resource-Group.outputs.resource_group_name
```
{: pre}

### module.outputs
{: #bp-module-outputs}

Type:       list

Default:    []  

A list defining all the outputs that are returned by the module to be used as inputs to other modules or output from the blueprint. Each output is identified by the label `name`. It must match an [output declaration](https://developer.hashicorp.com/terraform/language/values/outputs){: external} in the Terraform template for the value to be retrieved at execution time from the module. The name can be copied from the [module metadata](https://github.com/terraform-ibm-modules){: external} or from inspecting the Terraform `.tf` files. 

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
