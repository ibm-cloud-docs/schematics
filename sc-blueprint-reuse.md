
---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-29"

keywords: schematics blueprints, reuse, reusable

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Reusing blueprints and pipelines
{: #blueprint-reuse-pipelines}

{{site.data.keyword.bplong}} Blueprints utilizes the analogy of building a house from a blueprint drawing. Where a blueprint defines the architecture, layout, major building blocks and customizations to be applied.  

This building analogy also applies to reuse across environments. A builder may build an entire street of houses from the same blueprint drawing. Each house customized by its choice of color, lighting and styling, but all built to the same design.    

Reuse supports a number of usecases:
- Sharing an approved architecture across teams within a business
- Deploying instances across multiple regions to create a highly resilient application deployment
- Software delivery pipelines
{: shortdesc} 

## Reuse across environments
{: #blueprint-reuse} 
A blueprint template (house design) is similarly reusable across environments, using a separately maintained [input configuration](/docs/schematics?topic=schematics-glossary#bpi1) to define the customizations for the target environment and usage. The figure illustrates this with deploying dev, stage and prod environments. 


![Environments deployed from reusable blueprint template](/images/new/bp-reuse.svg){: caption="Environments deployed from reusable blueprint template" caption-side="bottom"}

Separate input files for the dev, stage and prod define the customizations, for instance, scale, configuration and region for each environment.    

Each blueprint environment is uniquely defined by its own [blueprint configuration](/docs/schematics?topic=schematics-glossary#bpb3). The configuration defines the template, its location and version, plus the inputs to customize a template for the target environment. The separation of template from its runtime configuration allows a single template to be reused many times to deploy a range of environments such as `dev`, `stage`, and `production` with multiple target regions. 

For details on how to configure blueprints refer to the section [Understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates).  

## Deployment pipelines
{: #blueprint-pipelines} 

Reuse supports the creation of software delivery deployment pipelines. Here the same or similar environment configurations are required to support the stages of a delivery pipeline to ensure that application code is tested in an environment that is as close to production as possible. 
