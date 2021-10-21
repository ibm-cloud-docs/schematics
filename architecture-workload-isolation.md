---

copyright:
  years: 2017, 2021
lastupdated: "2021-10-21"

keywords: schematics architecture, schematics compliance, schematics workload isolation, schematics depdendencies

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

# {{site.data.keyword.bpshort}} architecture and workload isolation
{: #compute-isolation}

Learn about the {{site.data.keyword.bpshort}} service architecture, the service dependencies, and how customer workloads are isolated from each other in {{site.data.keyword.bplong_notm}}.
{: shortdesc}

## Service architecture
{: #architecture}

The following image shows the main {{site.data.keyword.bplong_notm}} components, how they interact with each other, and what type of encryption is applied to your personal information. 
{: shortdesc}

<img src="images/schematics_architecture.png" alt="{{site.data.keyword.bplong_notm}} architecture and data encryption process" width="900" style="width: 900px; border-style: none"/>

1. A user sends a request to create an {{site.data.keyword.bplong_notm}} workspace to the {{site.data.keyword.bpshort}} API server.
2. The API server retrieves the Terraform template and input variables from your GitHub or GitLab source repository, or the tape archive file (`.tar`) that you uploaded from your local machine. 
3. All user-initiated actions, such as creating a workspace, generating a Terraform execution plan, or applying a plan are sent to RabbitMQ and added to the internal queue. RabbitMQ forwards requests to the {{site.data.keyword.bpshort}} engine to execute the action. 
4. The {{site.data.keyword.bpshort}} engine starts the process for provisioning, modifying, or deleting {{site.data.keyword.cloud}} resources. 
5. To protect customer data in transit, {{site.data.keyword.bplong_notm}} integrates with {{site.data.keyword.keymanagementserviceshort}}. {{site.data.keyword.bpshort}} uses root keys in {{site.data.keyword.keymanagementserviceshort}} to create data encryption keys (DEK). The DEK is then used to encrypt workspace transactional data, such as logs, or the Terraform `tf.state` file in transit. 
6. Workspace transactional data is stored in an {{site.data.keyword.cos_full_notm}} bucket and encrypted by using [Server-Side Encryption with {{site.data.keyword.keymanagementserviceshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) at rest.  
7. Workspace operational data, such as the workspace variables and Terraform template information, is stored in {{site.data.keyword.cloudant}} and encrypted at rest by using the default service encryption. For more information, see [Security](/docs/Cloudant?topic=Cloudant-security).

## Workload isolation
{: #workload-isolation}

Review how your workloads are isolated in {{site.data.keyword.bplong_notm}}.
{: shortdesc}

### How are my API requests to the service isolated from other API requests? 
All API requests to the {{site.data.keyword.bpshort}} API server are queued in Messages for RabbitMQ. RabbitMQ forwards these requests to the {{site.data.keyword.bpshort}} engine that processes these requests. At any given time, a maximum of n API requests are processed by the Schematics engine. By default, n equals 20, but this number is manually adjusted by the {{site.data.keyword.bpshort}} operator based on the current API workload. For every API request from a {{site.data.keyword.bpshort}} tenant, the {{site.data.keyword.bpshort}} engine creates a dedicated job that runs to completion and is then removed when the API request is fully processed. The {{site.data.keyword.bpshort}} jobs are not shared between tenants or reused later.

### How is my information in {{site.data.keyword.cloudant}} and {{site.data.keyword.cos_full_notm}} isolated from other tenant data?
{{site.data.keyword.bpshort}} does not store any personal information, but stores sensitive technical information for a workspace as described in [What information is stored in {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-secure-data#pi-data). All data that is stored in Cloudant and IBM Cloud Object Storage is encrypted in transit and at rest by using the encryption mechanism that are described in the [service architecture](#architecture). 

### How are my workloads isolated from other tenants? 
When you use {{site.data.keyword.bpshort}} to provision {{site.data.keyword.cloud}} resources, these resources are created in your personal {{site.data.keyword.cloud_notm}} account. You are responsible to manage these resources and to keep them up-to-date to avoid security vulnerabilities or downtime for your workloads. {{site.data.keyword.cloud_notm}} resources are provisioned, updated, and deleted as defined in the Terraform template and requested by the user. Because all resources are created in your personal account, resources are not shared with or reused by other {{site.data.keyword.cloud_notm}} tenants. 

## Dependencies on other services
{: #dependencies}

Review the services that {{site.data.keyword.bpshort}} uses and how {{site.data.keyword.bpshort}} connects to these services. 

|Service name|Description|Private/ public endpoint|
|---------|--------------------|---------------|
|{{site.data.keyword.at_short}}|{{site.data.keyword.bpshort}} integrates with {{site.data.keyword.at_full_notm}} to forward workspace audit events to the {{site.data.keyword.at_full_notm}} service instance that is set up and owned by the {{site.data.keyword.bpshort}} user. For more information, see [{{site.data.keyword.at_short}} events](/docs/schematics?topic=schematics-at_events). Additionally, the {{site.data.keyword.bpshort}} service team uses {{site.data.keyword.at_full_notm}} to monitor service-internal audit events.|Private|
|Business Support Service (BSS) for {{site.data.keyword.cloud_notm}} | The BSS component is used to access information about the {{site.data.keyword.cloud_notm}} account, service subscription, service usage, and billing.|Public|
|Certificate Manager|This service is used to store and manage the TLS certificates for the {{site.data.keyword.bpshort}} domains.|Public|
|Cloudant|Cloudant is used to store workspace operational data, such as the workspace variables, workspace metadata, and Terraform template information. All data is encrypted at rest by using the default service encryption.|Public|
|{{site.data.keyword.cloud_notm}} command-line |When {{site.data.keyword.bpshort}} runs command-line commands, the service connects to the service API endpoint over the public service endpoint.|Public|
|{{site.data.keyword.registrylong_notm}}|This service is used to store the container images that {{site.data.keyword.bpshort}} uses to run the service.|Public|
|{{site.data.keyword.cloud_notm}} Infrastructure (IaaS)|The IaaS service is used to provision and update classic {{site.data.keyword.cloud_notm}} infrastructure resources that are used by {{site.data.keyword.bpshort}}.|Private|
|{{site.data.keyword.cloud_notm}} Service Endpoint (CSE)|This service is used to implement the private service endpoint for {{site.data.keyword.bpshort}}. For more information, see [Using private endpoints](/docs/schematics?topic=schematics-secure-data#pi-location).|Private|
|Identity and Access Management (IAM)| To authenticate requests to the service and authorize user actions, {{site.data.keyword.bpshort}} implements service access roles in Identity and Access Management (IAM). For more information about required IAM permissions to work with the service, see [Managing user access](/docs/schematics?topic=schematics-access).|Public|
|Internet Services (CIS)|This service is used to provide the global load balancer and firewall for {{site.data.keyword.bplong_notm}}|Public|
|Key Protect|{{site.data.keyword.bpshort}} uses root keys in {{site.data.keyword.keymanagementserviceshort}} to create data encryption keys (DEK). The DEK is then used to encrypt workspace transactional data, such as logs, or the Terraform `tf.state` file in transit.|Private|
|Kubernetes Service|The {{site.data.keyword.bplong_notm}} service control plane runs in an {{site.data.keyword.containerlong_notm}} cluster and leverages the built-in security, high availability, and self-healing capabilities of the service. If an {{site.data.keyword.bplong_notm}} user creates a workspace or provisions {{site.data.keyword.cloud_notm}} resources, these resources are provisioned into the user account outside the {{site.data.keyword.bpshort}} cluster.|Public|
|{{site.data.keyword.la_full_notm}} {{site.data.keyword.bpshort}} sends service logs to {{site.data.keyword.loganalysisfull_notm}}. These logs are monitored and analyzed by the {{site.data.keyword.bpshort}} service team to detect service issues and malicious activities.|Private|
|Messages for RabbitMQ|RabbitMQ is used to queue incoming API requests, and to process these requests asynchronously.|Private|
|Monitoring {{site.data.keyword.bpshort}} with {{site.data.keyword.mon_full_notm}} |{{site.data.keyword.bpshort}} sends service metrics to {{site.data.keyword.mon_full_notm}}. These metrics are monitored by the {{site.data.keyword.bpshort}} service team to identify capacity and performance issues.|Private|
|Object Storage (COS)|This service is used to store workspace transactional data, such as the Terraform state file, logs, and user-provided data. All data is encrypted by using [Server-Side Encryption with Key Protect](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) in transit and at rest.|Private|


