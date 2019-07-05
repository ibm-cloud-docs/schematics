---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-05"

keywords: schematics, automation, terraform

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

# Getting started with IBM Cloud Schematics
{: #getting-started}

Enable Infrastructure as Code (IaC) with {{site.data.keyword.cloud_notm}} Schematics, and start templatizing, provisioning, and managing {{site.data.keyword.cloud_notm}} resources in your {{site.data.keyword.cloud_notm}} environment by using Terraform configuration files. 
{: shortdesc}

In this getting started tutorial, you provision a virtual server in a VPC in {{site.data.keyword.cloud_notm}} with {{site.data.keyword.cloud_notm}} Schematics by using the IBM-provided template `<name>`. 

1. From the [{{site.data.keyword.cloud_notm}} catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=devops), select **Schematics**. 
2. Click **Browse templates**. 
3. Select the `<name>` template. 
4. Review the details of the template in the **About** tab. This tab shows all {{site.data.keyword.cloud_notm}} resources that are included in the selected template, including the charges that occur when you provision this template. 
5. Select the **Create** tab. 
6. Configure your workspace. 
   1. Enter a name for your workspace. 
   2. Select a resource group. All {{site.data.keyword.cloud_notm}} resources of your template are provisioned within the selected resource group. 
   3. Enter a description for your workspace. 
   4. In the **Variables** section, enter the zone and pricing plan that you want to use for your virtual server. 
7. To create your workspace, click **Create workspace**. 



