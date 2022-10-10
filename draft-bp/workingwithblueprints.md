---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-10"

keywords: schematics blueprints, work with blueprint, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the beta release.
{: beta}

# Overview
{: #work-with-blueprints}

{{site.data.keyword.bplong}} Blueprints is an Infrastructure as Code (IaC) deployment and lifecycle management service for large-scale cloud environments. It utilises the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout and the major building blocks.  

{{site.data.keyword.bplong}} blueprint environments are deployed using Terraform based IaC automation modules designed for the IBM Cloud, with the reference architecture and module usage defined by a blueprint template. The blueprint separation of automation module code from the template defining the architecture makes it possible for teams to easily build large and complex environments from resuable modular components. Then, continue to update these environments over their lifecycle, cradle-to-grave, using IAC best practices and versioning of the templates and modules. 

During the lifecycle of a blueprint environment, {{site.data.keyword.bpshort}} Blueprints manages change through the controlled application of changes and versioning of all elements of a blueprint. The template defining the infrastructure architecture and layout, the Terraform IaC automation modules used to deploy and configure cloud resources, and the configuration of the blueprint environment. 

Operations on blueprint environments follow the sequence:
- Creating or updating the blueprint configuration with changes to the inputs, template or module versions
- Planning the change to identify resource changes, additions, or deletions 
- Executing the plan to apply the changes 
{: shortdesc}

## Lifecycle of cloud environments
{: #lifecycle-of-iac}

When you work with cloud environments, hosted applications, or services they all follow a lifecycle from creation to end-of-life. IaC managed cloud environments implicitly go through several lifecycle states. This starts with the initial definition of an IaC configuration and a set of environment inputs. Then, onto deployment of the environment, its infrastructure and resources. The environment is maintained through its operational life, which might be hours to years. Finally, deletion when all associated resources are destroyed, billing gets terminated, and the environment and all configuration is removed.  
{: shortdesc}

{{site.data.keyword.bpshort}} Blueprints refers to these life stages as the `define`, `deploy`, `update and operate`, and `delete` lifecycle stages. 

The lifecycle stages of a blueprint environment are illustrated in the diagram. Each stage is composed of multiple blueprint tasks. For example, composing and editing the blueprint template, running {{site.data.keyword.bpshort}} blueprint operations, monitoring job execution, and reviewing the results. 

![Life stages of {{site.data.keyword.bpshort}} Blueprints](../images/bp-life-stages.svg){: caption="Life stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

The four life stages of working with {{site.data.keyword.bpshort}} Blueprints to deploy and manage cloud environments are outlined in the table.

| Stages | Description |
| -- | -- |
| Define | The reference architecture is specified as a reusable blueprint template YAML file using an editor or IDE. The resulting template and inputs are saved in a version-controlled source code repository. |
| Deploy | A blueprint configuration is created in {{site.data.keyword.bpshort}}. The configuration specifies the blueprint template YAML and input files, versions, and the source Git repositories to be used. The blueprint environment and its cloud resources are deployed by {{site.data.keyword.bpshort}} running the Terraform automation module code to create the resources. |
| Update and Operate | The operational life of a deployed environment and its cloud resources might range from hours to years depends on its usage. During this time, the blueprint environment, templates, and inputs may be updated many times to satisfy changing application requirements. Changes need to be applied to the environment to maintain platform currency or compliance as security policies evolve. Scheduled operations might be run for compliance checks and drift detection on the environment. |
| Delete | Finally the application or service that is hosted in the environment gets retired or rehosted into a new environment. Now, the environment is deleted by destroying the deployed resources, stopping billing for any chargeable resources and deleting the blueprint configuration from {{site.data.keyword.bpshort}}. |
{: caption="Stages of {{site.data.keyword.bpshort}} Blueprints" caption-side="bottom"}

Each of these lifecycle stages of working with blueprint environments is explored in the following sections. 

1. [Defining blueprint environments](/docs/schematics?topic=schematics-define-blueprints) 
2. [Deploying blueprint environments](/docs/schematics?topic=schematics-deploy-blueprints)
3. [Updating and operating blueprint environments](/docs/schematics?topic=schematics-update-blueprints)
4. [Deleting blueprint environments](/docs/schematics?topic=schematics-delete-blueprints) 
  
## Next steps
{: #working-bp-nextsteps}

Start your journey of working with Blueprints with the section on [Defining blueprint environments](/docs/schematics?topic=schematics-define-blueprints). 
