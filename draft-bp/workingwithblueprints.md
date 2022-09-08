---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-08"

keywords: schematics blueprints, work with blueprint, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Working with Blueprints
{: #work-with-blueprints}

{{site.data.keyword.bplong}} Blueprints implements robust cradle-to-grave management of large-scale cloud environments that redefine by using infrastructure solution patterns. It is composed of reusable open source Infrastructure as Code (IaC) automation modules. {{site.data.keyword.bpshort}} Blueprints support environment configuration and controlled change through the versioning of all elements of a solution definition such as the infrastructure pattern, IaC automation modules, and the environment configuration.
{: shortdesc}

## Lifecycle of IaC managed cloud infrastructure
{: #lifecycle-of-iac}

When you are working with Blueprints IaC environments, they can be considered to have four life stages such as `define`, `deploy`, `operate`, and `delete`. These stages take a cloud environment from its initial design and specification as an IaC definition. Deployment of the environment and its cloud resources. Through its operational life, which might be hours to years. Finally, deletion when all resources are deleted and the environment is removed.
{: shortdesc}

The lifecycle stages of working with a Blueprint environment are illustrated in the diagram. Each stage consists of multiple tasks, for example create and edit the Blueprint definition, run {{site.data.keyword.bpshort}} Blueprint operations, monitoring job execution and reviewing the results.

![Life stages of {{site.data.keyword.bpshort}} Blueprints](../images/bp-life-stages-Lifecycle.svg){: caption="Life stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

The four life stages of working with {{site.data.keyword.bpshort}} Blueprints to deploy and manage cloud environments are discussed in the table.

| Stages | Description |
| -- | -- |
| Define | The solution architecture is specified as a reusable Blueprint definition YAML file by using an editor or IDE. The resulting definition and inputs are saved in a version-controlled source code repository. |
| Deploy | From a user-supplied Blueprint configuration, versioned Blueprint definition, and input files are retrieved from Git repositories and a Blueprint instance is created in {{site.data.keyword.bpshort}}. In a separate step, the cloud environment and its resources are deployed by {{site.data.keyword.bpshort}} from the specified versions of the Blueprint definition and inputs. |
| Operate | The operational life of a deployed environment and its cloud resources might range from hours to years depend on its usage. During this time, the versions of the elements of the Blueprint might be updated many times. Changes that are applied to the cloud resources to satisfy changing application requirements. Maintain platform currency or compliance as security policies evolve. Scheduled operations might be run for compliance checks and drift detection on the environment. |
| Delete | Finally, when the application or service that are hosted in the environment is retired or rehosted into a new environment. The cloud infrastructure is deleted by destroying the associated resources, stopping billing for any chargeable resources, and delete the {{site.data.keyword.bpshort}} Blueprint. |
{: caption="Stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

Each of these life stages of working with Blueprints is explored in more detail in the following sections.  

1. [Defining Blueprint IaC managed environments](/docs/schematics?topic=schematics-define-blueprints) 
2. [Deploying a Blueprint IaC managed environment](/docs/schematics?topic=schematics-deploy-blueprints)
3. [Operating Blueprint managed IaC environments](/docs/schematics?topic=schematics-operate-blueprints)
4. [Deleting Blueprint IaC managed environments](/docs/schematics?topic=schematics-delete-blueprints) 
  
## Next steps
{: #working-bp-nextsteps}

You can see working with Blueprints by [Defining a Blueprint IaC managed environment](/docs/schematics?topic=schematics-define-blueprints). 
