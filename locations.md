---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-08"

keywords: schematics locations, schematics regions, schematics zones, schematics endpoints, schematics service endpoints

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Locations and service endpoints
{: #locations} 

Review supported locations in {{site.data.keyword.bplong_notm}} and how {{site.data.keyword.bpshort}} differentiates between the data storage location and the deployment location of your {{site.data.keyword.cloud}} resources.

## Where can I create {{site.data.keyword.bpshort}} workspaces?
You can choose to create {{site.data.keyword.bpshort}} workspaces in the US or Europe location by using one of the following API endpoints, or by using the **Location** drop-down menu from the {{site.data.keyword.cloud_notm}} console.

For more information about the private service endpoint, see [Using private endpoints](/docs/schematics?topic=schematics-secure-data#pi-location). 
{: tip}

## Where do my {{site.data.keyword.bpshort}} actions run?

The location that you choose for your {{site.data.keyword.bpshort}} workspace determines the location where your {{site.data.keyword.bpshort}} actions, such `plan` or `apply`, run. 

|Geography/ location |Location to run {{site.data.keyword.bpshort}} actions|
|------------|----------------|
|North America|{{site.data.keyword.bpshort}} actions run in either the `us-south` or `us-east` location.|
|Dallas|{{site.data.keyword.bpshort}} actions run in the `us-south` location.|
|Washington|{{site.data.keyword.bpshort}} actions run in the `us-east` location.|
|Europe|{{site.data.keyword.bpshort}} actions run in either the `eu-de` or `eu-gb` location.|
|Frankfurt|{{site.data.keyword.bpshort}} actions run in the `eu-de` location.|
|London|{{site.data.keyword.bpshort}} actions run in the `eu-gb` location.|

## Where is my data stored?

The location where your workspace data is stored depends on the location where you create your workspace. For more information, see [Securing your data](/docs/schematics?topic=schematics-secure-data). 

## Where are my {{site.data.keyword.cloud_notm}} resources provisioned?

The region or regions where your {{site.data.keyword.cloud_notm}} resources are provisioned depends on the region that you specify in your Terraform configuration file. You can choose to specify a region for each individual resource, or to specify a region in the `provider` block of your Terraform configuration file that is applied to all resources in your file. For more information, see [Terraform `provider` block configuration](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference). 

