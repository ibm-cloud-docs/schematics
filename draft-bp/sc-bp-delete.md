---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-10"

keywords: schematics blueprints, delete blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the beta release.
{: beta}

# Deleting blueprint environments
{: #delete-blueprints}

When the application that is hosted in the blueprint environment is no longer needed, or has been migrated into a new environment, the original environment enters the delete lifecycle stage. In this stage, all the cloud resources that are deployed and managed by the blueprint environment are cleaned up and destroyed, which stops billing for any chargeable resources. 
{: shortdesc}

Deleting a blueprint environment is currently a two-step process, resource deletion followed by deleting the {{site.data.keyword.bpshort}}blueprint configuration. In a future release an additional plan step will be added before the resource destroy to allow verification of the proposed changes prior to executing the operation.  

The first stage destroys all the deployed cloud resources, leaving the blueprint configuration in place. The second step, deletes the blueprint configuration from {{site.data.keyword.bpshort}}. This flow is illustrated in the diagram. 

![Deleting a blueprint environment](../images/sc-bp-delete.svg){: caption="Deleting a blueprint environment" caption-side="bottom"}

1. The user initiates a destroy of the cloud resources that belong to the blueprint environment. The blueprint destroy operation iterates through all the blueprint modules, destroying resources in reverse dependency order to ensure that all cloud resources are cleanly removed. On resource destroy, all the modules return to an `Inactive` state to indicate that no cloud resources remain. For more information, see [Destroy blueprint](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-destroy). When the resources get destroyed, billing gets terminated.  
    - Optional: A fresh instance of the environment can be deployed from the saved {{site.data.keyword.bplong}} blueprint configuration by running the [blueprint run apply](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-install) operation to re-create the environment and cloud resources. 

2. The blueprint modules and blueprint configuration are deleted from {{site.data.keyword.bpshort}} using the delete operation. It is only enabled if all the resources are destroyed and the modules are in an `Inactive` state. For more information, see [Delete blueprint](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-delete). 
The {{site.data.keyword.bpshort}} blueprint environment and cloud resources are now deleted. 

## Next steps
{: #delete-nextsteps}

- You can explore more by using the [blueprint tutorials](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli&interface=cli).
- See the [FAQs](/docs/schematics?topic=schematics-blueprints-faq) and [troubleshooting guide](/docs/schematics?topic=schematics-bp-create-fails) for any challenges and questions on using blueprints.

