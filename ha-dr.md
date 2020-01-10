---

copyright:
  years: 2019, 2020
lastupdated: "2020-01-10"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs

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
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# High availability
{: #high-availability}
{: help}
{: support}

Understand the high availability features of the {{site.data.keyword.cloud_notm}} resources that you want to provision with {{site.data.keyword.bplong_notm}} and design your resources to meet the availability requirements that your business and customers need. 
{: shortdesc}

High availability is a core discipline in an IT infrastructure to keep your resources healthy and your app workloads up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failure in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms.

**Who is responsible to set up high availability for my resources?**</br>
While {{site.data.keyword.bplong_notm}} is responsible to ensure that your workspace information is available, backed up, and replicated across multiple regions so that information can be recovered after a failure, {{site.data.keyword.bpshort}} does not set up high availability for your {{site.data.keyword.cloud_notm}} resources. Instead, you must understand the features that each resource offering provides to decide what level of availability is the right one for your needs. Then, you use {{site.data.keyword.bplong_notm}} to provision and configure your {{site.data.keyword.cloud_notm}} resources in a highly available setup.  

**How can I implement high availabillity for my resources?**</br>
Review the following image to find a general approach of how you can make your resource highly available and add resiliency to account for a site or region failure. The level of availability that is right for you depends on several factors, such as the high availability features that are available for your resource, your business requirements, the Service Level Agreements that you have with your customers, and the money that you want to spend.

Supported high availability features depend on the type of {{site.data.keyword.cloud_notm}} resource that you choose to provision with {{site.data.keyword.bplong_notm}}. Some of the high availability scenarios that are shown in this image might not be available for your resource. Make sure to review the resource documentation to find supported high availability features. 
{: note}

![High availability for {{site.data.keyword.cloud_notm}} resources](images/schematics-ha-roadmap.png)

1. **Single zone resources**: By default, your {{site.data.keyword.cloud_notm}} resource is deployed into one zone. This setup does not protect your workloads or data from a zonal or regional failure. 
2. **Multiple resources across zones**: You can create multiple instances and spread these instances across zones, such as [multizone clusters in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multizone) or a [Virtual Private Cloud with subnets in different zones](/docs/vpc-on-classic?topic=solution-tutorials-vpc-multi-region). This setup protects your workloads and data in case of a zonal failure. Note that this setup might not be available for your resource. 
3. **Multiple resources across regions**: You can create multiple instances and spread these instances across regions, such as [multiple clusters in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-ha_clusters#multiple_clusters). This setup provides the highest availability for your workloads and data in case of a zonal or regional failure. Note that this setup might not be available for your resource. 




