---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics blueprints, define blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Defining blueprints
{: #define-blueprints}

{{site.data.keyword.bplong}} Blueprints is an Infrastructure as Code (IaC) automation solution for large-scale cloud environments. It utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, major building blocks and customizations to be applied. Return to the [overview](/docs/schematics?topic=schematics-blueprint-intro#blueprint-overview) section for an introduction to Blueprints. 

A blueprint [template](/docs/schematics?topic=schematics-glossary#bpb2) determines the architecture and infrastructure topology to be deployed.  Reusable Terraform [modules](/docs/schematics?topic=schematics-glossary#bpb5) implement the layers and components of an infrastructure architecture from well designed, tested and compliant Terraform code. The definition of an architecture using modules and deployment as linked environments is illustrated. 
{: shortdesc} 

![Blueprint templates, modules and inputs define environment](/images/new/bp-overview.svg){: caption="Blueprint templates, modules and inputs define environment" caption-side="bottom"}

As shown, a blueprint template determines the infrastructure architecture, specifying the modules required for the implementation, the infrastructure topology and relationships between modules. Inputs customize the template for the target deployment. Blueprint operations build up the infrastructure by deploying the smaller modular environments defined by the template. Blueprints linking the module environments into the whole, by the sharing and passing of resource dependency data between modules.    


A blueprint environment and the cloud resources to be deployed are defined by three versioned elements:
1. A reusable `blueprint template` to represent the reference infrastructure architecture, the IaC automation modules to be used and the cloud resources that are to be deployed.
2. Versioned `blueprint inputs` to customize a blueprint template for the specific environment to be deployed.
3. Reusable `modules` written in Terraform to implement the infrastructure architecture and deploy the desired cloud resources. 

## Creating templates, inputs and configuration
{: #define-templates-input}

Blueprint environments can be created from reusable user or {{site.data.keyword.IBM_notm}} authored blueprint templates. New templates can be authored to address specific application requirements. Either by creating a new template from scratch, or by modifying existing templates. 

Templates are [reusable across multiple environments](/docs/schematics?topic=schematics-define-blueprints#define-templates-input), with the customizable input values maintained separately from the template as [inputs](/docs/schematics?topic=schematics-glossary#bpi1). In cookie cutter fashion, several environments can be created from the same blueprint template. Each environment has its own [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3) and inputs. This separation of template from its runtime configuration allows a single template to be reused many times to deploy a range of environments such as dev, stage, and production with multiple target regions. Each environment being customized with its own input values. See the section [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) for more details. 

For examples of blueprint templates and inputs, see the [{{site.data.keyword.bplong_notm}} repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint){: external}. Creation and editing of blueprints can be performed in [VSCode with the YAML language extension](/docs/schematics?topic=schematics-edit-blueprints). Refer to the sections [understanding blueprint templates and configurations](/docs/schematics?topic=schematics-blueprint-templates) and [blueprint template reference](/docs/schematics?topic=schematics-bp-template-schema-yaml) for guidance to the blueprint syntax when creating or modifying a template.   

Change in blueprint environments is explicitly managed through version control. Template and input documents are all sourced from a version control system such as GitHub, GitLab or {{site.data.keyword.IBM_notm}} Catalog. 

## Steps to define a blueprint
{: #define-blueprint-steps}

The steps to create a blueprint template and define the versioned inputs are illustrated in the diagram.

![Blueprint template and inputs](/images/new/sc-bp-define.svg){: caption="Blueprint template and inputs" caption-side="bottom"}

1. Defining a cloud environment starts with a high-level reference architecture for the environment. Using the reference architecture as a guide, select the automation modules to implement the infrastructure layers of the architecture. Publicly available modules can be combined with private modules to create a custom architecture. Modules can be sourced from the public repositories containing {{site.data.keyword.IBM_notm}} and third-party authored modules, along with any custom developed modules from private repositories. Refer to the section [using Terraform modules with blueprint templates](/docs/schematics?topic=schematics-blueprint-terraform) for details of how to work with Terraform root and child modules.
    - {{site.data.keyword.IBM_notm}} authored modules can be found in the [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external} GitHub repository.
2. Create and edit a blueprint template to implement the desired reference architecture from the selected modules. Review the sections [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) and [blueprint template YAML schema](/docs/schematics?topic=schematics-bp-template-schema-yaml) for guidance on teh syntax and structure of blueprint templates. 
    - Create and name a new blueprint template YAML file in your favorite editor. [VSCode with the YAML language extension](/docs/schematics?topic=schematics-edit-blueprints) is recommended, as it provides blueprint template syntax validation and auto-complete.   
    - Working blueprint examples and template YAML files can be found in the [Cloud-{{site.data.keyword.bpshort}}](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint) GitHub repository. Or work from the [sample template](https://github.com/Cloud-Schematics/blueprint-sample-template/){: external}.   
    - Review the module metadata and readme file information for the selected [modules](https://github.com/terraform-ibm-modules){: external} to guide writing the template blueprint [module definitions](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-schema). 
    - Define the [variable dependencies](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-inputs-value) between modules to pass the resource dependency data for provisioned resources.
    - Specify the [inputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs) the template requires, guided by the inputs required for module configuration. Review the section on [Customizing environments with inputs](/docs/schematics?topic=schematics-define-blueprints#define-blueprint-steps) to determine which inputs will be defined in the input file or specified as defaults. The input values are specified in step 4. 
    - Specify the template [outputs](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-outputs) that are to be returned when the environment is deployed. The values returned are typically an application URL or the IDs of provisioned resources. 
    - Optional: Create a readme file to document the template, inputs, and outputs. Best practice is to include an image of the reference architecture. 
3. Push the completed blueprint template to a Git repo. If needed, create a Git version release tag for blueprint version management. [Semantic versioning](https://semver.org/){: external} is strongly recommended. 
4. Customize your template with a versioned blueprint input file to configure it for your use case. 
   - Review the section on [Customizing environments with inputs](/docs/schematics?topic=schematics-define-blueprints#define-blueprint-steps) to determine which inputs will be defined in the input file. Undefined inputs, must either have a template default or be specified at runtime by a dynamic input. 
   - Review the sections [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) and [blueprint input YAML schema](/docs/schematics?topic=schematics-bp-input-schema-yaml) for the syntax to define blueprint input files.
   - Again in your favorite editor, create and name a new blueprint input YAML file. Alternatively follow the instructions for [editing templates in VSCode](/docs/schematics?topic=schematics-edit-blueprints).
   - Using the template inputs as a guide, populate the input file with environment-specific input key-value pairs. The variable type of the input value must match that defined in the template YAML file.
   - It is recommended that inputs defining secrets or sensitive values are omitted from the input file.Then specified when the blueprint configuration is created in {{site.data.keyword.bpshort}}.
5. Push the blueprint input YAML file to a Git repo. As required, create a Git version release tag for version management. If input file is to be versioned and updated separately to the template file, the blueprint input file should be versioned in a separate repo to the template.  

## Next steps
{: #define-nextsteps}

The next stage of working with blueprints is [Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints). 
