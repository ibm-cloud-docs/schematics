---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-04"

keywords: schematics blueprints, reuse, reusable

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Reuse and customization 
{: #blueprint-reuse}

{{site.data.keyword.bplong}} Blueprints utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, major building blocks and customizations to be applied.
{: shortdesc}

This building analogy also applies to reuse across environments. A builder may build an entire street of houses from the same blueprint drawing. Each house customized by its choice of color, lighting and styling, but all built to the same design.   

Blueprint template reuse supports a number of usecases:
- Sharing an approved architecture across teams within a business
- Deploying instances across multiple regions to create a highly resilient application deployment
- Software delivery pipelines, dev, stage, prod

## Reuse across environments
{: #blueprint-reuse-env} 

A blueprint template (house design) is reusable across environments, using a separately maintained [input configuration](/docs/schematics?topic=schematics-glossary#bpi1) to define the customizations for the target environment and usage. The figure illustrates this with deploying dev, stage and prod environments.
{: shortdesc}

![Environments deployed from reusable blueprint template](/images/new/bp-reuse.svg){: caption="Environments deployed from reusable blueprint template" caption-side="bottom"}

A common reusable template defines the architecture, with separate input files for the dev, stage and prod for define the customizations. For example, the scale, configuration and region for each environment. All based off the same template. 

Each blueprint environment is uniquely defined by its own [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The configuration defines the template, its location and version, plus the inputs to customize a template for the target environment. The separation of template from its runtime configuration allows a single template to be reused many times to deploy a range of environments such as dev, stage, and production with multiple target regions. 

For details on how to configure a blueprint refer to the section [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates). 

### Deployment pipelines
{: #blueprint-pipelines} 

Reuse supports the creation of software delivery deployment pipelines. Here the same or similar environment configurations are required to support the stages of a delivery pipeline to ensure that application code is tested in an environment that is as close to production as possible. 
{: shortdesc}

The prior figure illustrates the reuse of a template across the stages of a delivery pipeline. A single template provides a common architecture pattern against which application code can be developed and tested in production like environments. The configuration for each environment is provided by the inputs, customizing each stage of the pipeline, adjusting for scaling, region and  networking.

## Customization best practice
{: #blueprint-customization-bp} 

Blueprints best practice is to for templates to be reusable across the instances of an application environment, from dev to stage and production. This is an outcome of [IaC modular architecture best practice](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity), where the reliability of reusable components increases through testing and hardening via reuse.  External configuration of templates customizes a deployment for a blueprint environment. customization is performed by separately defined, and versioned blueprint input configurations, with hardcoding of specific configurations avoided within the template itself. The template itself being maintained and versioned separately to the environment specific customization.  


### Blueprint customization
{: #blueprint-customization} 

Blueprints is built around reuse. Blueprints' implements layers of configuration that build on reusable shared modules, with layers of customization targeting specific use cases. This provides for independent versioning of components, update and maintenance of templates and of the (externalized) configurations. These layers are described below in increasing order of customization: 

- **Modules**: Reusable components, implementing the layers of an application architecture. Designed for reuse across multiple architectures, teams and organizations. Modules are lightly opinionated to implement security and organization best practices, while allowing for reuse. They are actively maintained and semantically versioned. Module version selection is determined at the template layer. 
- **Templates**: Templates are written to address a range deployments for a specific architecture and application. It is an opinionated implementation of an application architecture out of reusable modules. Templates support external customization of infrastructure scaling, deployment region, application components, network and security configuration. They are semantically versioned and maintained through update of module versions.   
    - User customizable inputs are externalized for configuration as `inputs`. Default template input values allow for use with minimal user configuration.
    - Versioning is defined by Git release tags and branches.
    - Template version selection is managed by Git release tags specified at blueprint config create time.      
- **Inputs**: Inputs customize a template for a target environment: dev, stage, prod or regions. They provide configuration for scaling, region and networking etc. Blueprints' supports two layers of input customization. Versioned input files for controlled change of environment specific configuration and un-versioned dynamic (override) inputs. 
    - Versioned input files. These provide versioned control over environment configuration parameters in production and regulated environments. Versioned input files are optional and for less regulated development environments all parameters can be configured by dynamic (override) inputs. 
      - Versioning is defined by Git release tags and branches.   
      - Input file version selection is managed by Git release tags specified at blueprint config create time.  
    - Dynamic (override) inputs. Typically sensitive input values, like API or SSH keys that should not be maintained in a version control system. In unregulated or development environments, all inputs can be supplied as dynamic inputs. 


Selection of template and input file versions is defined on initial creation of the blueprint config for the environment in Schematics. Review the section on [blueprint versioning](/docs/schematics?topic=schematics-blueprint-versioning). The omission of template and input file version information at config create time allows for a ‘relaxed’ usage mode. Where any changes to the template or input files are automatically pulled in if {{site.data.keyword.bpshort}}detects a change in the source repository. 


## Customizing environments with inputs
{: #blueprint-customization-layers}

As described, blueprint templates support environment customization and versioning at multiple levels. User customizable values are defined as `inputs` at the blueprint template level. 

The selection of inputs is determined in the following precedence order, lower to higher: 

- Blueprint template
   - From VCS, version controlled by Git tag, branch or un-versioned (pull-latest). 
   - Input definition:  **Default** value or **no value** specified in template YAML file. 
   - Usage: Opinionated implementation of an application architecture. 

- Blueprint input files
   - From VCS, version controlled by Git tag, branch or un-versioned (pull-latest).
   - Value **optionally** specified in inputs YAML file.
   - Usage: Environment scaling, region, network and security configuration. 

- Dynamic (override) inputs
   - User provided via CLI or UI, un-versioned
   - Value **optionally** specified
   - Usage: SSH and API keys

Inputs that are not satisfied at any level will result in an error at blueprint config create time. 

## Using versioned inputs
{: #bp-version-input}

Blueprints uses versioning of templates and input files to manage infrastructure change. 
Review the section on [blueprint versioning](/docs/schematics?topic=schematics-blueprint-versioning) to understand how Blueprints versioning can be used. 

The use of input versioning is summarized in the table. 

| Blueprint layer	| Development and unregulated environments	|  Regulated environments | 
| --- | --- | --- 
| Dynamic (override) inputs	| Any input, no version control. Override values specified in input file	| Sensitive values only | 
| Input files	Optional, un-versioned	Versioned (tag or branch) | 
| Templates	| Un-versioned |	Versioned (tag or branch) | 
| Modules	| Un-versioned | 	Versioned (tag) |
{: caption="Usage of input version" caption-side="bottom"}

## Blueprint input value precedence 
{: #blueprint-input-precedence} 

All template inputs must have an input value at blueprint config create time. Missing template input values will result in an error at config create time. 

A template input can have one of three value definitions:
- No value 
- Default value
- Specified value

Blueprints loads input values in the following order with later sources taking precedence over earlier ones:

### No value
{: #bp-precedence-novalue}

- Blueprint template input  - no value: one of the following is **mandatory**   
- Blueprint input file value 
- Blueprint (override) input value 

### Default value
{: #bp-precedence-defaultvalue}

- Blueprint template input  - default value, one of the following is optional   
- Blueprint input file 
- Blueprint (override) input value 

### Specified value
{: #bp-precedence-specifiedvalue}

- Blueprint template input  - value intended for settings of values internal to the template. Can be optionally overridden by one of the following during template development.  
- Blueprint input file 
- Blueprint (override) input value 
