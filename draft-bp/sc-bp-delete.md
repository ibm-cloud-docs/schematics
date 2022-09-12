---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-12"

keywords: schematics blueprints, delete blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Deleting Blueprint environments
{: #delete-blueprints}

When the application that is hosted on the Blueprint managed infrastructure is no longer needed or has been migrated into a new environment, the original Blueprint environment enters the delete lifecycle stage. In this step all the cloud resources deployed and managed by the Blueprint environment are cleaned up and destroyed, which will also stop billing for any chargeable resources. 
{: shortdesc}

Deleting a Blueprint environment is a two-stage process of first destroying all the associated cloud resources and then deleting the Blueprint configuration and definition in {{site.data.keyword.bpshort}}. This is illustrated in the diagram. 

![Deleting a Blueprint environment](../images/sc-bp-delete.svg){: caption="Deleting a Blueprint environment" caption-side="bottom"}

1. The user initiates a destroy of the cloud resources that are associated with the Blueprint. The Blueprint destroy operation iterates through all modules, destroying resources in reverse dependency order to ensure that all cloud resources are cleanly removed. On resource destroy, all modules return to an `Inactive` state indicating no cloud resources remain. For more information on running the destroy operation, see [Destroy Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-destroy). When the resources are destroyed, billing is also terminated.  
    - Optional, a fresh instance of the environment can be deployed from the saved {{site.data.keyword.bplong}} Blueprint configuration by running [Blueprint Apply](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-install) to re-create the cloud resources. 
2. The modules and Blueprint definition are deleted from {{site.data.keyword.bpshort}} using the Blueprint delete command or UI delete operation. This is only enabled once all the resources have been destroyed and the modules are in an `Inactive` state. For more information, see [Delete Blueprint](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete). 

The {{site.data.keyword.bpshort}} Blueprint environment and cloud resources are now deleted. 

## Next steps
{: #delete-nextsteps}

- You can explore more by using [Blueprint tutorials](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli&interface=cli).
- See the [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions on Blueprints.

