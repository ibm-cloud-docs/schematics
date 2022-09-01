---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-30"

keywords: schematics blueprints definition, blueprints yaml, schema definitions, definitions, yaml,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Blueprint definition YAML Schema
{: #bp-definition-schema-yaml}

This document is the reference of the YAML schema used to describe the `blueprint.yaml` file.

The `blueprint.yaml` file defines all modules in the Blueprint. Every Blueprint consists of a list of automation modules from which the solution architecture is composed. 

Each `blueprint.yaml` file has a configuration preface:

```yaml

name: schematics-dev-blueprint
type: "blueprint"
schema_version: "1.0"
description: "Project to provision Schematics Service."
inputs:
  - name: resource_group
  - name: region
  - name: api_key
outputs:
  - name: schematics-service-url
    value: $module.bp-vsi-vpc-app.outputs.app_url
settings: 
  - name: TF_VERSION
    value: 1.0
```
{: pre}

## Supporting setting parameters
{: #bp-parameters}

Following are the supporting setting parameters that can be used to configure `blueprint.yaml`.

### name
{: #bp-name}

Type:       string

Required:   true

Name that is used to identify the blueprint definition in use 

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

A string used to describe the Blueprint to provide users with more information about its usage and the solution it describes.

Example 
```yaml
description: "Project to provision Application Service."
```
{: pre}

### inputs
{: #bp-inputs}

Type: list

Default: []

A list defining all the inputs required by the Blueprint. Inputs are specified in the [input.yaml](/docs/schematics?topic=schematics-bp-input-schema-yaml) file or as configuration parameters when the Blueprint is configured in {{site.data.keyword.bpshort}}. Inputs are defined with a `name:` key. 

Example

```yaml
inputs:
    - name: resource_group
    - name: region
    - name: api_key
```
{: pre}

### outputs
{: #bp-outputs}

Type: list

Default: [] 
 
A list defining all the outputs that are returned by the Blueprint to the user. Each output is identified by a key-value pair. 

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

A list of the settings to be used by the Blueprint defined as key-value pairs. Each setting is identified by the key `name`.

The only supported setting is `TF_VERSION`.

### settings.TF_VERSION
{: #bp-tf-version}

Type:       number

Blueprints sets the Terraform version to be used at Workspace execution time based on the value of TF_Version. This value can be used to pin the version of Terraform used by {{site.data.keyword.bpshort}} to remain compatible with the Blueprint supported version. Updating this value will change the Terraform version used on the next execution. 

Options:    Terraform version in SemVer format 

Example

```yaml
settings: # Master settings for all modules 
  - name: TF_VERSION
    value: 1.0 
```
{: pre}

### modules schema
{: #bp-modules-schema}

The `modules` block defines the Terraform, and Ansible modules from which the solution is constructed. 

Each module list entry is defined by a name, the source for the module, inputs, and outputs.   

```yaml
modules:
  - name: bp-vsi-resource-group
    module_type: terraform 
    source:
      source_type: github
      git: 
        git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/master/IBM-ResourceGroup"
        git_branch: master
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
        git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/master/IBM-Storage"
        git_branch: master
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

The name that are used within {{site.data.keyword.bpshort}} to identify the workspace that will be created to manage the group of resources created by the Terraform config specified on the `modules.source.git.git_url` statement. 

### modules.module_type
{: #bp-modules-moduletype}

Type: string

Required: true

String specifying the IaC type of the automation module. Only Terraform is supported at this time.   

Options: `terraform`

### modules.source options
{: #bp-modules-sourceoptions}

The source where the Terraform, or Ansible config will be downloaded from. 

```yaml
source:
  source_type: github
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/master/IBM-ResourceGroup"
    git_branch: master
```
{: pre}

### modules.source.git.source_type
{: #bp-modules-source-type}

Type: string

Required: true

Type of Git source repository. Only GitHub validated at this time. 

Options: `github` 

### modules.source.git.git_repo_url
{: #bp-modules-git-repo-url}

Type: url string

Required: true

URL for the automation module in its content repository. If the config exists in a sub directory the folder name is appended. 

### modules.source.git.git_branch
{: #bp-modules-git-branch}

Type: string

Default: master

If content is in git, the branch containing the users Terraform config. This option is mutually exclusive with the `git_release` option. 

### modules.source.git.git_release
{: #bp-modules-git-release}

Type: string

Default: latest

If content is git, the release tag for the repo containing the users Terraform config. This option is mutually exclusive with the `git_branch` option.

Options: `latest` or release in SemVer format 

### modules.inputs options
{: #bp-modules-inputs-options}

Type: list

Default: []

A list defining all the inputs required by the module. 

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
    type: |
      list(object({
        internal = number
        external = number
        protocol = string
      })
```
{: pre}

### modules.inputs.name
{: #bp-modules-inputs-name}

Type: string

Required: true

Name of variable to be passed to Terraform Workspace or Ansible playbook. This must match the value in the target template for the value to be passed at execution time to the Workspace. 

### modules.inputs.type
{: #bp-modules-inputs-type}

Type: YAML flow or block scalar 

Default: string 

As Blueprints primarily works with Terraform configurations, Terraform variable type constraints are used to perform type validation for Blueprints inputs. https://www.terraform.io/language/expressions/type-constraints#type-constraints  The type constraint must match the variable type in the target config for the value to be passed successfully at execution time to the Workspace. The type can be copied from the module metedata or the Terraform `variables.tf` file. 

As complex Terraform types are typically represented as multi-line strings, YAML block syntax can be used.   

Options: Any valid Terraform variable type

Example
```yaml
- name: docker_ports
  value: $blueprint.docker_ports
  type: |
    list(object({
    internal = number
    external = number
    protocol = string
  })

```
{: pre}

### modules.inputs.value
{: #bp-modules-inputs-value}

Type: Any valid Terraform variable type

Required: true

The value field sources the input value for modules from three sources:
- Statically defined values specified on the name value pair statement of the module, in yaml syntax
- An input to the Blueprint defined in the settings prefix and sourced at runtime from an input file. Identified by the $blueprint prefix
- An output value from another module defined in this blueprint.yaml file. 
    - Identified by the `$module` prefix and must be included in the output section of another module.
    - The format is the token, `$module` followed by the module name, the token `outputs`, followed by the module output name. 

The value type is as defined by the `modules.inputs.type` option. 

Example of statically defined values 
```yaml
- name: provision_ats_instance
  type: boolean            
  value: false
```
{: pre}

Example value sourced from an input statement in the blueprint `inputs` section
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

### modules.inputs.secure
{: #bp-modules-inputs-secure}

Type: Boolean

Default: false

Flag specifying if the value is a sensitive variable and must be masked in the output

### module.outputs
{: #bp-module-outputs}

Type:       list

Default:    []  

A list defining all the outputs that will be returned by the module to be utilized as inputs to other workspaces or output from the blueprint. Each output is identified by the label `name`.  This must match the value in the module template for the value to be retrieved at execution time from the Workspace. The name can be copied from the module metedata or from inspecting the Terraform `.tf` files. 

Example
```yaml
outputs:
    - name: log_analysis_instance_name
    - name: sysdig_instance_name
```
{: pre}

### module.injectors options
{: #bp-modules-outputs-injector}

Type:         list 

Default:      []

The injectors block is an optional block to configure the parameters required by {{site.data.keyword.bpshort}} to inject the template files into the module automation repo. The primary use with Blueprints is to enable direct use of Terraform modules with Blueprints, by the injection of `provider` and `terraform` blocks.

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

URL of the Git repo containing the template files used for injection. 

### module.injectors.tft_name
{: #bp-module-tft-name}

Type: string

Required: true

Name of the template file to use

Options: `ibm` or `kubernetes`

### module.injectors.injection_type
{: #bp-module-injection-type}

Type: string

Required: true

Two modes of injection are supported with Terraform configurations. Definitions can be injected as additional files if its believed there is no conflict with any existing HCL statements. Alternatively they can be injected as [HCL override files](https://www.terraform.io/language/files/override).

Options: `override` or `inject`

### module.injectors.tft_parameters
{: #bp-modules-tft-parameters}

Type: list

Required: true

A list defining the inputs as name/value pairs, required as input to the selected template. At this time only static values are supported.  
 
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
