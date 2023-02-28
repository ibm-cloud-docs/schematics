---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics blueprints, work with blueprint, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Understanding blueprints and environments
{: #work-with-blueprints}

{{site.data.keyword.bpshort}} Blueprints brings [infrastructure as code (IaC) practices](/docs/schematics?topic=schematics-infrastructure-as-code) to the creation and lifecycle management of large-scale cloud environments. It uses the analogy of a blueprint used in construction, to define and deploy cloud environments from reusable modules of Terraform code. Blueprint operations take cloud environments from their initial creation, through maintenance and ops to final decommissioning and clean up of all allocated resources. 

Review the section [About {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro) for more background. A key feature of Blueprints is support to take cloud environments through their life, from creation to final removal. 

Blueprints enables users to define and deploy cloud environments using modules of reusable and well-defined [Terraform](https://www.terraform.io) automation code. This builds on the IaC best practice of [modular architectures](/docs/schematics?topic=schematics-infrastructure-as-code#iac-bp-modularity), where reusable modules implement the layers and components of an infrastructure architecture from well designed, tested and compliant Terraform code.


## Cloud infrastructure lifecycle 
{: #cloud-infra-lifecycle}

{{site.data.keyword.bpshort}} Blueprints follows an lifecycle operations model. Cloud environments, hosted applications or services all follow a lifecycle from creation to end-of-life. The life of a blueprint environment starts with the initial definition of an infrastructure architecture and configuration. Then, onto deployment of the environment and resources. It will be updated and maintained through its operational life. Which might be hours to years. Finally, to end-of-life when it is torn down, the cloud resources are destroyed, billing gets terminated and the configuration is removed. 
{: shortdesc}

The different stages of working with the blueprint lifecycle come under four headings, `define`, `deploy`, `maintain`, and `delete`. In software lifecycle terms these correspond to Day-0 define, Day-1 deploy, Day-2 operate, through to deletion of the environment. 

![{{site.data.keyword.bpshort}} Blueprints lifecycle stages](/images/new/bp-lifecycle.svg){: caption="{{{site.data.keyword.bpshort}} Blueprints lifecycle stages" caption-side="bottom"}

Each of these life stages is supported by blueprint operations. For example, composing and editing the blueprint template, running blueprint operations, monitoring job execution and reviewing the results. 

All updates and changes to environments during these lifecycle stages are performed in a controlled sequence of steps:
- Creating or updating the blueprint configuration with changes to the inputs, template or module versions
- Planning the change to identify resource changes, additions or deletions 
- Running the plan to apply the changes to create, modify or delete resources

## Working with environments and the blueprint lifecycle 
{: #bp-working-env}

Explore the following sections to get into the details of working with blueprints and environments over their lifecycle.   

### Defining blueprints
{: #bp-define}

[Defining blueprints](/docs/schematics?topic=schematics-define-blueprints): The infrastructure architecture for an environment is defined as a reusable blueprint template. Reusable modules implement the layers and components of the architecture from Terraform code. 

### Deploying blueprints
{: #bp-deploy}

[Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints): A blueprint configuration defines the environment to be deployed. In cookie cutter fashion, several environments can be created from the same blueprint template to deploy a range of environments such as dev, stage, and production. 

### Maintaining blueprints
{: #bp-maintain}

[Maintaining blueprints](/docs/schematics?topic=schematics-update-op-blueprints): During the operational life of a deployed environment, the templates, and inputs may be versioned and updated many times to satisfy changing application requirements. Changes will be applied to the environment to maintain platform currency and compliance as security policies evolve. Scheduled operations are run for compliance checks and drift detection. 

### Deleting blueprints
{: #bp-delete}

[Deleting blueprints](/docs/schematics?topic=schematics-delete-blueprints): Finally the application or service that is hosted in the environment gets retired or rehosted into a new environment. The environment is removed by destroying the deployed resources, stopping billing for any chargeable resources and deleting the blueprint configuration from {{site.data.keyword.bpshort}}. 
  
## Next steps
{: #working-bp-nextsteps}

Start your journey of working with blueprints with an [overview of blueprints](/docs/schematics?topic=schematics-blueprint-intro) and onto the first lifecycle stage of [defining blueprints](/docs/schematics?topic=schematics-define-blueprints). 

Follow this up with:
- [Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints) and environments. 
- [beta code for {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand beta limitations.
