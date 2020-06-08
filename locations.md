---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-08"

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

Review supported locations in {{site.data.keyword.bplong_notm}} and how {{site.data.keyword.bpshort}} differentiates between the data storage location and the deployment location of your {{site.data.keyword.cloud_notm}} resources.

## Where can I create {{site.data.keyword.bpshort}} workspaces?
You can choose to create {{site.data.keyword.bpshort}} workspaces in the US or Europe location by using one of the following API endpoints, or by using the **Location** drop down menu from the {{site.data.keyword.cloud_notm}} console.

|Location| API endpoint|
|------------|----------------|
|US|**Public**: </br>`https://us.schematics.cloud.ibm.com` </br> `https://schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-us.schematics.cloud.ibm.com`| 
|Europe|**Public:** </br> `https://eu.schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-eu.schematics.cloud.ibm.com`| 

For more information about how to use the private service endpoint, see [Using private endpoints](/docs/schematics?topic=schematics-private-endpoints). 
{: tip}

## Where do my {{site.data.keyword.bpshort}} actions run?

The location that you choose for your {{site.data.keyword.bpshort}} workspace determines the location where your {{site.data.keyword.bpshort}} actions, such `plan` or `apply`, run. 

|Workspace location |Location to run {{site.data.keyword.bpshort}} actions|
|------------|----------------|
|US|{{site.data.keyword.bpshort}} actions run in either the `us-south` or `us-east` location.|
|Europe|{{site.data.keyword.bpshort}} actions run in either the `eu-de` or `eu-gb` location.|

## Where is my data stored?

The location where your workspace data is stored depends on the location where you create your workspace. For more information, see [Securing your data](/docs/schematics?topic=schematics-secure-data). 

## Where are my {{site.data.keyword.cloud_notm}} resources provisioned?

The region or regions where your {{site.data.keyword.cloud_notm}} resources are provisioned depends on the region that you specify in your Terraform configuration file. You can choose to specify a region for each individual resource, or to specify a region in the `provider` block of your Terraform configuration file that is applied to all resources in your file. For more information, see [Terraform `provider` block configuration](/docs/terraform?topic=terraform-provider-reference). 

