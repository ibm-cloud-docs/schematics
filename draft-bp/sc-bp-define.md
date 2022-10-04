---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-04"

keywords: schematics blueprints, define blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Defining blueprint environments
{: #define-blueprints}

{{site.data.keyword.bplong}} Blueprints environments and associated cloud resources are defined by two YAML documents: 

- A `blueprint template` to represent the reference infrastructure architecture, the IaC automation modules to be used and the cloud resources that are to be deployed.
- A `blueprint inputs` to customize a blueprint template for the specific environment to be deployed.

Blueprint environments can be created from existing user or {{site.data.keyword.IBM_notm}} authored blueprint templates. Alternatively, new blueprint templates can be authored to address specific application requirements. Either by creating a new template from scratch or modification of an existing blueprint template. Blueprint templates are reusable across multiple environments, as the customizable input values maintained separately from the template. In cookie cutter fashion, several environments can be created from the same blueprint template. Each environment has its own blueprint configuration and blueprint inputs. This separation of template from its inputs allows a single blueprint template to be reused many times to deploy a range of environments such as `dev`, `stage`, and `production` with multiple target regions. Each environment being customized with its own input values.
{: shortdesc}  

See the [{{site.data.keyword.bplong_notm}} repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint){: external} for  examples of blueprint templates and inputs. For guidance on creating or modifying an existing blueprint template, review the sections on [understanding blueprint configurations](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-templates){: external} and [blueprint template reference](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-templates){: external}.  

Change to blueprint environments is explicitly managed through version control. Template and input documents are all sourced from a version control system such as GitHub, GitLab or IBM Catalog. 

The steps to specify a blueprint template and prepare the inputs are illustrated in the diagram.

![Blueprint template and inputs](../images/sc-bp-define.svg){: caption="Blueprint template and inputs" caption-side="bottom"}

1. Defining a blueprint environment starts from a high-level reference architecture for the environment. Using the reference architecture as a guide, select the automation modules to implement the infrastructure layers of the architecture. Publically available modules can be combined with private modules to create a custom architecture. Modules can be sourced from the public repositories containing {{site.data.keyword.IBM_notm}} and third-party authored modules, along with any custom developed modules from private repositories.  
    - {{site.data.keyword.IBM_notm}} authored modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.
2. Create and edit the blueprint template YAML to implement the wanted reference architecture by using the selected IaC modules.
    - Create and name a new blueprint template YAML file in your favorite editor. Example blueprint template YAML files can be found in the [Cloud-{{site.data.keyword.bpshort}}](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint) GitHub repository.  Alternatively, download one of the existing blueprint template YAML files and use it as a basis for a new template.  
    - Review the module metadata and readme file information for the selected IaC modules to guide writing the template blueprint module definitions. 
    - Define the variable dependencies between modules to pass the resource dependency data for provisioned resources.
    - Specify the inputs the blueprint template accepts. The input values are specified in step 4. 
    - Specify the outputs that are to be returned when the blueprint environment is deployed. This is typically an application URL or the IDs of provisioned resources.  
    - Optional: Create a readme file to document the blueprint template, inputs, and outputs. Best practice is to include an image of the reference architecture. 
3. Push the completed blueprint template to a Git repo. If needed, create a Git version release tag for blueprint version management. 
4. Customize your blueprint template with a versioned blueprint input file to configure it for your use case.  
    - Again in your favorite editor, create and name a new blueprint input YAML file.
    - Using the blueprint template inputs as a guide, populate the input file with environment-specific input key-value pairs. The variable type of the input value must match that defined in the template YAML file.
    - Inputs defining secrets or sensitive values are left with null-values. Sensitive values are specified when the blueprint configuration is created in {{site.data.keyword.bpshort}}.
5. Push the blueprint input YAML file to a Git repo. As required, create a Git version release tag for version management. If the blueprint template is intended for resuse across multiple environments, the blueprint input files should be versioned in separate repos to the template.  

## Next steps
{: #define-nextsteps}

The next stage of working with blueprint environments is [Deploying a blueprint environment](/docs/schematics?topic=schematics-deploy-blueprints). 
