---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-12"

keywords: schematics blueprints, work with blueprint, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Working with Blueprints
{: #work-with-blueprints}

{{site.data.keyword.bplong}} Blueprints is an Infrastructure as Code (IaC) pattern-based deployment and lifecycle management service for large-scale cloud environments. Using Terraform based IaC automation modules designed for the IBM Cloud, Blueprints make it possible for teams to reliably and repeatedly stand up large and complex environments that are composed from modular components. Then, continue to use versioning and IaC to manage these environments over their lifecycle, cradle-to-grave. 

During the lifecycle of a cloud environment, {{site.data.keyword.bpshort}} Blueprints manages configuration and change through the controlled application of changes and versioning of all elements of a solution pattern. It includes the infrastructure architecture and topology, IaC automation modules that are used to deploy and configure cloud resources, and the environment configuration. Change is applied in two stages. First, as updates to the Blueprint configuration and definition in {{site.data.keyword.bpshort}}, then as changes to the environment and cloud resources. 
{: shortdesc}

## Lifecycle of cloud environments
{: #lifecycle-of-iac}

All cloud environments and hosted applications or services have a lifecycle. When you work with cloud environments, they implicitly go through several life states. It starts with the initial definition and specification of an IaC configuration and a set of environment inputs. Then, onto deployment of the environment, its' infrastructure and resources. The environment is maintained through its operational life, which might be hours to years. Finally, deletion when all associated resources are destroyed, billing gets terminated, and the environment and definitions are removed.  
{: shortdesc}

In {{site.data.keyword.bpshort}} Blueprints these life stages are referred to as the `define`, `deploy`, `operate`, and `delete` lifecycle stages. 

The Blueprints lifecycle stages are illustrated in the diagram. Each Blueprint lifecycle stage encompasses multiple tasks. For example, composing and editing the Blueprint definition, running {{site.data.keyword.bpshort}} Blueprint operations, monitoring job execution and reviewing the results. 

![Life stages of {{site.data.keyword.bpshort}} Blueprints](../images/bp-life-stages-Lifecycle.svg){: caption="Life stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

The four life stages of working with {{site.data.keyword.bpshort}} Blueprints to deploy and manage cloud environments are outlined in the table.

| Stages | Description |
| -- | -- |
| Define | The solution architecture is specified as a reusable Blueprint definition YAML file by using an editor or IDE. The resulting definition and inputs are saved in a version-controlled source code repository. |
| Deploy | From a user-supplied Blueprint configuration a Blueprint instance is created in {{site.data.keyword.bpshort}}. The configuration specifies the Blueprint definition YAML and input files, needed versions, and the source Git repositories to be used. In a separate step, the cloud environment and its resources are deployed by {{site.data.keyword.bpshort}} from the specified versions of the Blueprint definition and inputs. |
| Operate | The operational life of a deployed environment and its cloud resources might range from hours to years depends on its usage. During this time, the Blueprint environment, definitions, and inputs are updated many times to satisfy changing application requirements. Changes need to be applied to the environment to maintain platform currency or compliance as security policies evolve. Scheduled operations might be run for compliance checks and drift detection on the environment. |
| Delete | Finally the application or service that is hosted in the environment gets retired or rehosted into a new environment. Now, the environment is deleted by destroying the deployed resources, stopping billing for any chargeable resources and deleting the Blueprint from {{site.data.keyword.bpshort}}. |
{: caption="Stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

Each of these life stages of working with Blueprints, the tasks that are performed and supporting Blueprint operations are explored in the following sections. 

1. [Defining Blueprint environments](/docs/schematics?topic=schematics-define-blueprints) 
2. [Deploying Blueprint environments](/docs/schematics?topic=schematics-deploy-blueprints)
3. [Operating Blueprint environments](/docs/schematics?topic=schematics-operate-blueprints)
4. [Deleting Blueprint environments](/docs/schematics?topic=schematics-delete-blueprints) 
  
## Next steps
{: #working-bp-nextsteps}

Start your journey of working with Blueprints with the section on [Defining Blueprint environments](/docs/schematics?topic=schematics-define-blueprints). 
