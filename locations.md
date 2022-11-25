---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-25"

keywords: schematics locations, schematics regions, schematics zones, schematics endpoints, schematics service endpoints

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Service locations and endpoints
{: #locations} 

Review the {{site.data.keyword.bplong_notm}} supported locations and how {{site.data.keyword.bpshort}} differentiates between the user data and template storage location, and the deployment region of your {{site.data.keyword.cloud}} resources.

## Where can I create and run {{site.data.keyword.bpshort}} Workspaces?
{: #where-wks-created}

You can choose to create {{site.data.keyword.bpshort}} Workspaces in the US or Europe geographies by using one of the following API endpoints, or by using the **Location** drop-down menu in the {{site.data.keyword.cloud_notm}} console.

For more information about the {{site.data.keyword.bpshort}} private service endpoint, see [Using private endpoints](/docs/schematics?topic=schematics-secure-data#pi-location). 
{: tip}

## Where do my {{site.data.keyword.bpshort}} Actions run?
{: #where-do-locations-run}

The location that you choose for your {{site.data.keyword.bpshort}} Workspaces determines the location where your {{site.data.keyword.bpshort}} jobs, such `plan` or `apply`, run. 

|Geography/ location |Location to run {{site.data.keyword.bpshort}} Actions|
|------------|----------------|
|North America|{{site.data.keyword.bpshort}} jobs run in either the `us-south` or `us-east` location.|
|Dallas|{{site.data.keyword.bpshort}} jobs run in the `us-south` location.|
|Washington|{{site.data.keyword.bpshort}} jobs run in the `us-east` location.|
|Europe|{{site.data.keyword.bpshort}} jobs run in either the `eu-de` or `eu-gb` location.|
|Frankfurt|{{site.data.keyword.bpshort}} jobs run in the `eu-de` location.|
|London|{{site.data.keyword.bpshort}} jobs run in the `eu-gb` location.|
{: caption="Regions where an action is?" caption-side="bottom"}

## Where is my {{site.data.keyword.bpshort}} and template data stored?
{: #where-is-data-stored}

The region your workspace data is stored is determined by the location where you create your workspace. For more information, see [Securing your data](/docs/schematics?topic=schematics-secure-data). 

## Where are my {{site.data.keyword.cloud_notm}} resources provisioned?
{: #where-are-resources-provisioned}

The region or regions where your {{site.data.keyword.cloud_notm}} resources are provisioned are determined by the region that you specify in your Terraform configuration. You can specify a region for each individual resource, or to specify a region in the `provider` block of your Terraform configuration that is applied to all resources. Multiple regions are supported by using provider aliases. For more information, see [Terraform `provider` block configuration](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference). 
