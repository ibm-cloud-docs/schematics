---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-19"

keywords: schematics blueprints, reuse, reusable

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Blueprint reuse and customization 
{: #blueprint-reuse-pipelines}

{{site.data.keyword.bplong}} Blueprints utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, major building blocks and customizations to be applied.
{: shortdesc}

This building analogy also applies to reuse across environments. A builder may build an entire street of houses from the same blueprint drawing. Each house customized by its choice of color, lighting and styling, but all built to the same design.   

Reuse supports a number of usecases:
- Sharing an approved architecture across teams within a business
- Deploying instances across multiple regions to create a highly resilient application deployment
- Software delivery pipelines

## Reuse across environments
{: #blueprint-reuse} 

A blueprint template (house design) is similarly reusable across environments, using a separately maintained [input configuration](/docs/schematics?topic=schematics-glossary#bpi1) to define the customizations for the target environment and usage. The figure illustrates this with deploying dev, stage and prod environments.
{: shortdesc}

![Environments deployed from reusable blueprint template](/images/new/bp-reuse.svg){: caption="Environments deployed from reusable blueprint template" caption-side="bottom"}

Separate input files for the dev, stage and prod define the customizations, for example, scale, configuration and region for each environment.   

Each blueprint environment is uniquely defined by its own [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The configuration defines the template, its location and version, plus the inputs to customize a template for the target environment. The separation of template from its runtime configuration allows a single template to be reused many times to deploy a range of environments such as `dev`, `stage`, and `production` with multiple target regions. 

For details on how to configure blueprints refer to the section [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates). 

## Deployment pipelines
{: #blueprint-pipelines} 

Reuse supports the creation of software delivery deployment pipelines. Here the same or similar environment configurations are required to support the stages of a delivery pipeline to ensure that application code is tested in an environment that is as close to production as possible. 
{: shortdesc}

## Customization best practice
{: #blueprint-customization-bp} 

Blueprints best practice is to for templates to be reusable across the instances of an application environment, from dev to stage and production. This is an outcome of [IaC modular architecture best practice](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity), where the reliability of reusable components increases through testing and hardening via reuse.  External configuration of templates customizes a deployment for a blueprint environment. customization is performed by separately defined, and versioned blueprint input configurations, with hardcoding of specific configurations avoided within the template itself. The template itself being maintained and versioned separately to the environment specific customization.  


### Blueprint customization
{: #blueprint-customization} 

To support template reuse, Blueprints implements layers of configuration that allow for independent versioning, update and maintenance of templates and input configurations. 

  -  **Modules**: Reusable components, implementing the layers of an application architecture. Designed for reuse across multiple architectures, teams and organizations. Lightly opinionated to implement security and organization best practices, while allowing for reuse.  Versioned and maintained. Version section determined by template module definition. 
- **Templates**:  Template developed to address range deployments for a specific architecture and application. Opinionated implementation of an application architecture out of reusable modules. Supports external customization of infrastructure scaling, deployment region, application components, network and security configuration.  Versioned and maintained.  Allows for version controlled deployment to multiple environments, though Git release tags specified at blueprint config create time.  
    - Only user customizable inputs are externalized to users for configuration as `inputs`. Default values allow for use with minimal user configuration.   
- **Inputs**: customization of template for target environment, dev, stage, prod or regions. Provides configuration for scaling, region,  networking etc. Blueprints supports two layers of input customization. Versioned input files for change control of environment specific configuration and un-versioned dynamic (override) inputs. 
    - Version managed input files. Provide version control for all environment configuration parameters in production and regulated environments.  Versioned input files are optional and all parameters can be configured by dynamic (override) inputs. Version controlled deployment managed by Git release tags specified at blueprint config create time.  
    - Dynamic (override) inputs. Typically sensitive input values, like API or SSH keys that should not be maintained in a version control system. In unregulated or development environments, all inputs can be supplied as dynamic inputs. 


Selection of versioning of templates and input files is defined on initial creation of the blueprint config for the environment in Schematics. Review section on [blueprint versioning](/docs/schematics?topic=schematics-blueprint-versioning). Omission of template and input file version information at config create time allows for a ‘relaxed’ usage mode, where any changes to the template or input files are automatically pulled in if a change is found in the source repository. 


## Customizing blueprint environments
{: #blueprint-customization-layers} 

As described, blueprint templates support environment customization and versioning at multiple levels. User customizable values are defined as `inputs` at the blueprint template level. 

The selection of inputs is determined in the following order:

- Dynamic (override) inputs
  - User provided via CLI or UI, un-versioned
  - Value optionally specified
  - Usage: SSH and API keys

- Blueprint input files**
  - From VCS, version controlled by Git tag, branch or un-versioned (pull-latest).
  - Value optionally specified in inputs YAML file.
  - Usage: Environment scaling, region, network and security configuration. 

- Blueprint template    
  - From VCS, version controlled by Git tag, branch or un-versioned (pull-latest). 
  - Input definition:  Default value or no value specified in template YAML file. 
  - Usage: Opinionated implementation of an application architecture.  

Inputs that are not satisfied at any level will result in an error at blueprint config create time. 



### Usage scenarios


Review the section on [blueprint versioning](/docs/schematics?topic=schematics-blueprint-versioning) to understand the Blueprints approach to versioning. 

| Blueprint layer	| Development and unregulated environments	|  Regulated environments | 
| --- | --- | --- 
| Dynamic (override) inputs	| Any input, no version control. Override values specified in input file	| Sensitive values only | 
| Input files	Optional, un-versioned	Versioned (tag or branch) | 
| Templates	| Un-versioned |	Versioned (tag or branch) | 
| Modules	| Un-versioned | 	Versioned (tag) | 



## Blueprint input value precedence 
{: #blueprint-input-precedence} 

All defined template inputs must have an input value at blueprint config create time. Missing input values will result in an error at config create time. 

Blueprints loads inputs in the following order with later sources taking precedence over earlier ones:

- Blueprint template yaml  - no value: one of the following is mandatory   
- Blueprint input file  
- Blueprint (override) input value 

</br> 

- Blueprint template yaml  - default value, one of the following is optional   
- Blueprint input file 
- Blueprint (override) input value 

</br>

- Blueprint template yaml  - value intended for settings of values internal to the template. Can be optionally overridden by one of the following during template development.  
- Blueprint input file 
- Blueprint (override) input value 


