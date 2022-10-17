---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-17"

keywords: schematics blueprints, work with blueprint, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Working with blueprints
{: #work-with-blueprints}

When you work with cloud environments, hosted applications or services they all follow a lifecycle from creation to end-of-life. An environment starts with the initial definition of a configuration and a set of inputs. Then, onto deployment of the infrastructure and resources. It is updated and maintained through its operational life, which might be hours to years. Finally, to end-of-life when it is torn down, the cloud resources are destroyed, billing gets terminated and the configuration is removed.  
{: shortdesc}

These tasks of working with a blueprint and a deployed environment over its lifecycle can be grouped under four headings, `define`, `deploy`, `update and operate`, and `delete`. Each breaks down into several blueprint operations. For example, composing and editing the blueprint template, running {{site.data.keyword.bpshort}} blueprint operations, monitoring job execution and reviewing the results. 

When working with blueprints, updates and changes during these lifecycle stages are performed in a controlled sequence of steps:
- Creating or updating the blueprint configuration with changes to the inputs, template or module versions
- Planning the change to identify resource changes, additions or deletions 
- Running the plan to apply the changes to create, modify or delete resources


[Defining blueprints](/docs/schematics?topic=schematics-define-blueprints): {{site.data.keyword.bplong}} blueprints utilizes the analogy of building a house from a blueprint drawing. The infrastructure architecture is defined as a reusable [blueprint template](/docs/schematics?topic=schematics-glossary#bpb2). Reusable [modules](/docs/schematics?topic=schematics-glossary#bpb5) implement the layers and components of the architecture from Terraform code. Separately maintained [input configurations](/docs/schematics?topic=schematics-glossary#bpi1) support reuse across dev, stage and prod pipelines and organizations.

[Deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints): A blueprint configuration defines the environment to be deployed. The configuration specifies the blueprint template YAML and input files, versions, and the source Git repositories to be used. The blueprint and its cloud resources are deployed by {{site.data.keyword.bpshort}} running the Terraform automation module code to create the resources. 

[Updating and operating blueprints](/docs/schematics?topic=schematics-update-blueprints): The operational life of a deployed environment and its cloud resources might range from hours to years depends on its usage. During this time, the environment, templates, and inputs may be updated many times to satisfy changing application requirements. Changes are applied to the environment to maintain platform currency or compliance as security policies evolve. Scheduled operations are run for compliance checks and drift detection. 

[Deleting blueprints](/docs/schematics?topic=schematics-delete-blueprints): Finally the application or service that is hosted in the environment gets retired or rehosted into a new environment. The environment is removed by destroying the deployed resources, stopping billing for any chargeable resources and deleting the blueprint configuration from {{site.data.keyword.bpshort}}. 

  
## Next steps
{: #working-bp-nextsteps}

Start your journey of working with blueprints with the section on [defining blueprints](/docs/schematics?topic=schematics-define-blueprints). 

Follow this up with:
- See [blueprints permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to set access permissions to deploy blueprints.
- Explore [deploying {{site.data.keyword.bpshort}} blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a blueprint.
- [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions on blueprints.
- [Beta code for {{site.data.keyword.bpshort}} blueprints](/docs/schematics?topic=schematics-bp-beta-limitations) to provide your feedback and understand Beta limitations.