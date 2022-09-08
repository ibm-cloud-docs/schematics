---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-08"

keywords: schematics blueprints, define blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Defining Blueprint environments
{: #define-blueprints}

{{site.data.keyword.bplong}} Blueprints IaC managed environments are defined by using two YAML documents: 

- A `Blueprint definition` to represent the solution infrastructure topology, the IaC automation modules to be used and the cloud resources to be deployed.
- A `set of input values` that customizes the Blueprint definition for the specific environment instance to be deployed.

Blueprint managed environments can be created from existing user or IBM authored Blueprints definitions. Alternatively, new Blueprint definitions authors to address specific application requirements, either by creation from scratch or modification of an existing Blueprint definition. Blueprint IaC definitions are designed to be reusable, with the customisable input values maintained separately from the resource and topology definition. In cookie cutter fashion, multiple environment instances can be created from the same Blueprint definition, each having its own custom input configuration. A single Blueprint definition can be used to deploy a range of environments such as `dev`, `stage`, and `production` with multiple target regions. Each environment instance is customised with its own input values.
{: shortdesc}  

See the [{{site.data.keyword.bplong_notm}} repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint){: external} for the examples of the Blueprint definitions and input values. For guidance on creating or modifying an existing Blueprint definition, review the sections on [understanding Blueprint configurations](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-definitions){: external} and [Blueprints definition reference](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-definitions){: external}.  

Ensure that Blueprint definitions and input values are version managed, Blueprints only supports retrieving these documents from a source control system such as GitHub or GitLab. 

The steps to specify a Blueprint definition and prepare the input values for an application environment are illustrated in the diagram.

![Blueprint definition and input value application](../images/sc-bp-define.svg){: caption="Blueprint definition and input value application" caption-side="bottom"}

1. Select the required IaC modules to implement the infrastructure layers of the solution architecture. These are sourced from the public repositories containing IBM and third-party authored modules, and any custom developed modules from private repositories.  
    - IBM authored modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.
2. Create and edit the Blueprint definition to implement the desired solution architecture by using the selected IaC modules.
    - Create and name a new Blueprint definition YAML file in your favourite editor. Example Blueprint definition YAML files can be found in the [Cloud-{{site.data.keyword.bpshort}}](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint) GitHub repo.  Alternatively, download one of the existing Blueprint definition YAML files and use this as a basis for a new definition.  
    - Review the module meta-data and readme information for the selected IaC modules to guide specifying the Blueprint module definitions. Also define the variable dependencies between modules to pass the resource dependency data for of provisioned resources.
    - Specify the inputs the Blueprint definition will accept.  The input values are specified in step 4. 
    - Specify the outputs that are to be returned when the Blueprint is deployed. This is usually application URLs and the IDs of provisioned resources.  
    - Optionally create a readme markdown file to document the Blueprint definition, its inputs and outputs.
3. Push the completed Blueprint definition to a Git repo. If required, create a Git version release tag for Blueprint version management. 
4. Customize your Blueprint definition with a versioned set of input values to configure it for an environment instance  
    - Again in your favourite editor, create and name a new input values YAML file.
    - Using the Blueprint definition inputs as a guide, populate the input file with instance-specific input key-value pairs. The variable type of the input value must match that defined in the definition YAML file.
    - Inputs defining secrets or sensitive values should be left with values. Sensitive values are specified during deployment when the Blueprint is created in {{site.data.keyword.bpshort}}.       
5. Push the input values YAML file to a Git repo. If required, create a Git version release tag for version management. 

## Next steps
{: #define-nextsteps}

The next stage of working with Blueprints is [Deploying a Blueprint IaC managed environment](/docs/schematics?topic=schematics-deploy-blueprints). 
