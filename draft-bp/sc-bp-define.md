---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-12"

keywords: schematics blueprints, define blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Defining Blueprint environments
{: #define-blueprints}

{{site.data.keyword.bplong}} Blueprints environments and associated cloud resources are defined by using two YAML documents: 

- A `Blueprint definition` to represent the solution infrastructure topology, the IaC automation modules to be used and the cloud resources that are deployed.
- A `input values` customize the Blueprint definition for the specific environment instance to be deployed.

Blueprint environments can be created from existing user or {{site.data.keyword.IBM_notm}} authored Blueprints definitions. Alternatively, new Blueprint definitions can be authored to address specific application requirements. Either by creating a new definition from scratch or modification of an existing Blueprint definition. Blueprint definitions are reusable, with the customizable input values maintained separately from the resource and topology definition. In cookie cutter fashion, multiple environment instances can be created from the same Blueprint definition. Where each instance has its own custom input configuration. The separation of definition from input configuration allows a single Blueprint definition can be reused many times to deploy a range of environments such as `dev`, `stage`, and `production` with multiple target regions. With each environment instance customized with its own input values.
{: shortdesc}  

See the [{{site.data.keyword.bplong_notm}} repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint){: external} for the examples of the Blueprint definitions and input values. For guidance on creating or modifying an existing Blueprint definition, review the sections on [understanding Blueprint configurations](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-definitions){: external} and [Blueprints definition reference](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-definitions){: external}.  

To ensure that change to Blueprint environments is managed, Blueprints supports the storage of its definition and input documents in a source control system such as GitHub or GitLab. 

The steps to specify a Blueprint definition, and prepare the input values are illustrated in the diagram.

![Blueprint definition and input value application](../images/sc-bp-define.svg){: caption="Blueprint definition and input value application" caption-side="bottom"}

1. Defining a Blueprint starts from a high-level solution architecture for the environment. Using the solution architecture as a guide, select the IaC modules to implement the infrastructure layers of the architecture. They are sourced from the public repositories containing the {{site.data.keyword.IBM_notm}}, and third-party authored modules, and any custom developed modules from private repositories.  
    - {{site.data.keyword.IBM_notm}} authored modules can be found in the GitHub repository [Terraform IBM Modules](https://github.com/terraform-ibm-modules){: external}.
2. Create and edit the Blueprint definition to implement the wanted solution architecture by using the selected IaC modules.
    - Create and name a new Blueprint definition YAML file in your favorite editor. Example Blueprint definition YAML files can be found in the [Cloud-{{site.data.keyword.bpshort}}](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint) GitHub repository.  Alternatively, download one of the existing Blueprint definition YAML files and use as a basis for a new definition.  
    - Review the module metadata and readme file information for the selected IaC modules to guide specifying the Blueprint module definitions. 
    - Define the variable dependencies between modules to pass the resource dependency data for provisioned resources.
    - Specify the inputs the Blueprint definition accepts. The input values are specified in step 4. 
    - Specify the outputs that are to be returned when the Blueprint is deployed. It is usually an application URL and the IDs of provisioned resources.  
    - Optional: Create a readme file to document the Blueprint definition, needed inputs, and outputs. It is to include an image of the solution architecture. 
3. Push the completed Blueprint definition to a Git repo. If needed, create a Git version release tag for Blueprint version management. 
4. Customize your Blueprint definition with a versioned set of input values to configure it for an environment instance.  
    - Again in your favorite editor, create and name a new input values YAML file.
    - Using the Blueprint definition inputs as a guide, populate the input file with instance-specific input key-value pairs. The variable type of the input value must match that defined in the definition YAML file.
    - Inputs defining secrets or sensitive values are left with null-values. Sensitive values are specified during the deployment stage when the Blueprint is created in {{site.data.keyword.bpshort}}.       
5. Push the input values YAML file to a Git repo. If needed, create a Git version release tag for version management. 

## Next steps
{: #define-nextsteps}

The next stage of working with Blueprints is [Deploying a Blueprint IaC managed environment](/docs/schematics?topic=schematics-deploy-blueprints). 
